# `prestats`: fMRI preprocessing module

`prestats` is an omnibus module for preprocessing of functional MR images. `prestats` incorporates removal of volumes, skull-stripping, outlier removal, temporal masking, motion estimation and correction, temporal and spatial filtering, and slice time correction.

## Omnibus modules

Omnibus modules defy modular logic to an extent: they do not comprise a single, well-encapsulated processing step. Instead, they include a number of _routines_, each of which corresponds to a common processing step. These routines can be combined and re-ordered within the parent module. Much like the `pipeline` variable specifies the inclusion and order of modules in the pipeline, the module-level `process` variable specifies the inclusion and order of routines within an omnibus module. An example is provided here for the `prestats` omnibus module:

```
pipeline=prestats,coreg,confound,regress,fcon,reho,alff,net,roiquant,seed,norm,qcfc
prestats_process[1]=DVO-MPR-STM-MCO-BXT-DMT
```

In the example here, the `pipeline` variable is defined as a standard functional connectivity stream that begins with `prestats`.

 * `prestats_process`: the name of the variable specifying the inclusion and order of routines
 * `[1]`: the scope of the `pipeline_process` variable, that is, the first module of the `pipeline`
 * `DVO-MPR-STM-MCO-BXT-DMT`: a series of three-letter codes for module routines to be called within `prestats`, offset by hyphens (`-`) and ordered in the same order that they are to be executed

### Available routine codes

#### `DVO`

_Discard terminal volumes_. This routine discards either the first or last several volumes from the sequence in order to account for scanner equilibration.

#### `MPR`

_Estimate motion parameters_. This routine realigns all acquired MR volumes to a reference (the midpoint of the time series) and uses the transformation matrices to estimate 6 motion parameters: 3 translational (x, y, z) and 3 rotational (yaw, pitch, roll). (Unlike MCO, MPR discards the realigned time series.)

#### `STM`

_Execute slice-time correction_. This routine modifies each volume of the time series to account for differences in the acquisition time of different slices. Slice time correction can bias motion estimates, so STM should be performed after MPR but before MCO.

#### `MCO`

_Correct for subject motion_. This routine realigns all acquired MR volumes to a reference volume. If the motion parameters have already been computed, it will use the volume with lowest motion as the reference.

#### `BXT`

_Extract brain voxels_. This routine uses FSL's `BET` to identify brain voxels and remove any non-brain voxels from the functional time series.

#### `DSP`

_Remove outliers_. This routine uses AFNI's `3dDespike` first to identify any outliers in the time series and then to interpolate over those outliers.

#### `DMT`

_Demean and de-trend time series_. This routine removes the image mean, as well as linear or polynomial trends, from the time series.

#### `SPT`

_Spatially filter/smooth the image_. This routine applies a spatial smoothing kernel to the time series in order to increase the similarity of signals in nearly voxels and remove local noise.

#### `TMP`

_Temporally filter the image_. This routine applies a temporal filter to the image, removing frequencies of no interest from each voxel's time series.

## Module configuration

### `prestats_dvol`

_Number of volumes to discard_.

Context: `DVO`.

When the module enters the `DVO` routine, a number of volumes equal to `prestats_dvol` will be removed from the functional time series. If `prestats_dvol` is positive, volumes will be removed from the beginning of the scan. If `prestats_dvol` is negative, volumes will be removed from the end of the scan.

In functional MRI studies, the initial volumes are typically discarded in order to account for T1 equilibration at the beginning of a scan. The exact number of volumes to discard may depend on acquisition parameters such as repetition time.

```bash
# remove 2 volumes from the start of the sequence
prestats_dvol[cxt]=2

# remove 2 volumes from the end of the sequence
prestats_dvol[cxt]=-2
```

`prestats_dvol` must be an integer.

### `prestats_stime` and `prestats_sdir`

When each 3D volume of a 4D timeseries is acquired, not all 2D slices of that volume are acquired simultaneously. To account for the temporal lag between acquisition of slices, FSL's `slicetimer` tool interpolates intensities using a Fourier transform when the module enters the `STM` routine. This correction is most important in event-related or lag analyses, and may be less important for time-averaged connectivity analyses and block designs.

```bash
# No slice time correction
prestats_stime[cxt]=none

# Bottom-up slice acquisition
prestats_stime[cxt]=up

# Top-down slice acquisition
prestats_stime[cxt]=down

# Interleaved slice acquisition
prestats_stime[cxt]=interleaved

# Slice acquisition along x axis
prestats_sdir[cxt]=X

# Slice acquisition along y axis
prestats_sdir[cxt]=Y

# Slice acquisition along z axis
prestats_sdir[cxt]=Z

# Use a custom order file
prestats_stime[cxt]=custom
prestats_stime_order[cxt]=true
prestats_stime_opath[cxt]=/path/to/custom/file

# Use a custom timing file
prestats_stime[cxt]=custom
prestats_stime_timing[cxt]=true
prestats_stime_tpath[cxt]=/path/to/custom/file
```

`prestats_stime` specifies the order of slice acquisition, while `prestats_sdir` specifies the direction of slice acquisition.

### `prestats_sptf` and `prestats_smo`

Endemic noise, for instance due to physiological signals or scanner activity, can introduce spurious or artefactual results in single voxels. The effects of noise-related artefacts can be mitigated by spatially filtering the data, thus dramatically increasing the signal-to-noise ratio. However, spatial smoothing is not without its costs: it effectively reduces volumetric resolution by blurring signals from adjacent voxels.

`prestats_sptf` specifies the type of spatial filter to apply for smoothing, while `prestats_smo` specifies the full-width at half-maximum (FWHM) of the smoothink kernel in mm. All spatial filtering is contained in the `SPT` routine.

 * Gaussian smoothing applies the same Gaussian smoothing kernel across the entire volume.
 * SUSAN-based smoothing restricts mixing of signals from disparate tissue classes (Smith and Brady, 1997).
 * Uniform smoothing applies smoothing to all voxels until the smoothness computed at every voxel attains the target value.
 * Uniform smoothing may be used as a compensatory mechanism to reduce the effects of subject motion on the final processed image (Scheinost et al., 2014).
 
 ```bash
 # No smoothing
 prestats_sptf[cxt]=none
 prestats_smo[cxt]=0
 
 # Gaussian kernel (fslmaths) of FWHM 6 mm
 prestats_sptf[cxt]=gaussian
 prestats_smo[cxt]=6
 
 # SUSAN kernel (FSL's SUSAN) of FWHM 4 mm
 prestats_sptf[cxt]=susan
 prestats_smo[cxt]=4
 
 # Uniform kernel (AFNI's 3dBlurToFWHM) of FWHM 5 mm
 prestats_sptf[cxt]=uniform
 prestats_smo[cxt]=5
 ```

### `prestats_tmpf`

For functional connectivity analysis, filtering during the `prestats` module is not recommended. (Filter during the `regress` module instead.) Bandpass filtering the analyte timeseries but not nuisance regressors re-introduces noise-related variance at removed frequencies when the timeseries is residualised with respect to the regressors via linear fit (Hallquist et al., 2014). (The XCP Engine is designed so as to make this involuntary reintroduction of noise impossible.) Instead, the recommended approach is filtering both the timeseries and the nuisance regressors immediately prior to fitting and residualisation (Hallquist et al., 2014). To enable this analytic pathway, select no filter and instead use the `regress` module's built-in temporal filter.

 * _FFT_-based filters, as implemented in AFNI's `3dBandpass`, use a fast Fourier transform to attenuate frequencies. An FFT-based filter may not be suitable for use in designs that incorporate iterative motion censoring, since it will include interpolated frequencies in its calculations."
 * A _Gaussian_ filter, as implemented in FSL, uses a Gaussian-weighted least-squares fit to remove frequencies of no interest from the data. This filter has a very slow frequency roll-off.
 * _Chebyshev_ and _elliptic_ filters more ideally discriminate accepted and attenuated frequencies than do _Butterworth_ filters, but they introduce ripples in either the passband (Chebyshev I), stopband (Chebyshev II), or both (elliptic) that result in some signal distortion.
 * `prestats_tmpf_order` specifies the filter order. (Relevant only for Butterworth, Chebyshev, and elliptic filters.)
 * `prestats_tmpf_pass` specifies whether the filter is forward-only (`prestats_tmpf_pass[cxt]=1`, analogous to `filter` or `lfilter` in NumPy or MATLAB) or forward-and-reverse (`prestats_tmpf_pass[cxt]=2`, analogous to `filtfilt` in NumPy or MATLAB, recommended). (Relevant only for Butterworth, Chebyshev, and elliptic filters.)
 * `prestats_tmpf_ripple` specifies the pass-band ripple, while `prestats_tmpf_ripple2` specifies the stop-band ripple. (`ripple` relevant only for Chebyshev I or elliptic filter, `ripple2` relevant only for Chebyshev II or elliptic filter.)

```bash
# Gaussian filter
prestats_tmpf[cxt]=gaussian

# FFT filter
prestats_tmpf[cxt]=fft

# Second-order Butterworth filter
prestats_tmpf[cxt]=butterworth
prestats_tmpf_order[cxt]=2
prestats_tmpf_pass[cxt]=2

# First-order Chebyshev I filter with pass-band ripple 0.5
prestats_tmpf[cxt]=chebyshev1
prestats_tmpf_order[cxt]=1
prestats_tmpf_pass[cxt]=2
prestats_tmpf_ripple[cxt]=0.5

# First-order elliptic filter with pass-band ripple 0.5 and stop-band ripple 20
prestats_tmpf[cxt]=elliptic
prestats_tmpf_order[cxt]=1
prestats_tmpf_pass[cxt]=2
prestats_tmpf_ripple[cxt]=0.5
prestats_tmpf_ripple2[cxt]=20
```

### `prestats_hipass` and `prestats_lopass`

Any frequencies below the low-pass cutoff and above the high-pass cutoff will be counted as pass-band frequencies; these will be retained by the filter when it is applied during the `TMP` routine.

Functional connectivity between regions of interest is typically determined on the basis of synchrony in low-frequency fluctuations (Biswal et al., 1995); therefore, removing higher frequencies using a low-pass filter may effectively remove noise from the timeseries while retaining signal of interest. For a contrasting view, see Boubela et al. (2013). Set `prestats_lopass` to `n` (Nyquist) to allow all low frequencies to pass.

High-pass filters can be used to remove very-low-frequency drift from an acquisition; this is a form of scanner noise. The demean/detrend option additionally removes linear and polynomial drift. Set `prestats_hipass` to 0 to allow all high frequencies to pass.

```bash
# Band-pass filter with pass-band 0.01-0.08 Hz
prestats_hipass[cxt]=0.01
prestats_lopass[cxt]=0.08

# High-pass-only filter (>0.01 Hz)
prestats_hipass[cxt]=0.01
prestats_lopass[cxt]=n

# Low-pass-only filter (<0.1 Hz)
prestats_hipass[cxt]=0
prestats_lopass[cxt]=0.1
```

### `prestats_fit`

The fractional intensity threshold determines how much of an image will be retained after non-brain voxels are zeroed during the `BXT` routine. A more liberal mask can be obtained using a lower fractional intensity threshold. The fractional intensity threshold should be a positive number greater than 0 and less than 1.

```bash
# Fractional intensity threshold of 0.3
prestats_fit[cxt]=0.3
```

### `prestats_bbgthr`

The brain-background threshold determines how much of an image will be retained after non-brain voxels are zeroed during the `BXT` routine. A more liberal mask can be obtained using a lower brain-background threshold. The brain-background threshold should be a positive number greater than 0 and less than 1.

```bash
# Brain-background threshold of 0.1
prestats_bbgthr[cxt]=0.1
```

### `prestats_dmdt`

Scanner drift may introduce linear or polynomial trends into time series data; these trends may be removed using a general linear model-based fit during the `DMT` routine. Notably, removal of trends in this manner will also remove the timeseries mean from the data, which may be undesirable for analyses in which absolute intensity is important. (The time series mean can be added back during the `regress` module.) `prestats_dmdt` must be a nonnegative integer.

 * An order of 0 results in demeaning only.
 * Entering `auto` will automatically compute an appropriate value (following AFNI) based on the duration of the scan. The formula used to compute this is: `floor(1 + TR*nVOLS / 150)`
 * For another data-driven approach to detrending, remove DMT from prestats and apply a high-pass Gaussian filter in this module. (A filter with higher rolloff can be added later during the regress module.)
 * To skip this step altogether, edit DMT out of the processing order.
 * If censoring is enabled, then the model will ignore the fit to any volumes flagged for censoring.

```bash
# Demean only
prestats_dmdt[cxt]=0

# Demean and linear detrend
prestats_dmdt[cxt]=1

# Demean and linear, quadratic, and cubic detrend
prestats_dmdt[cxt]=3

# Automatically estimate detrend order
prestats_dmdt[cxt]=auto
```

### `prestats_1ddt`

`prestats_1ddt` specifies whether 1-dimensional derivatives generated from the main time series should also be demeaned and detrended along with the main time series during the `DMT` routine. For instance, this flag would demean and detrend any realignment parameters.

```bash
# Demean and detrend 1D derivatives
prestats_1ddt[cxt]=1

# Demean and detrend only the main time series
prestats_1ddt[cxt]=0
```

### `prestats_censor`

Censoring high-motion volumes prevents them from exerting inordinate influence upon connectivity results. It is a comparatively aggressive and highly effective preprocessing strategy, but its effects on connectivity dynamics are not well understood at this time. (Naive sliding windows, beware!)

Censoring iteratively, during processing, is analogous to the "reprocessing" reported by Power et al. (2014). That is, censored valumes are not taken into account during demeaning, detrending, or confound regression. During temporal filtering, they are replaced using interpolation based on Lomb-Scargle spectral decomposition, and they are recensored after processing is completed. This method is recommended over simple censoring.

```bash
# Disable censoring: flag but do not censor poor-quality volumes
prestats_censor[cxt]=0

# Enable iterative censoring/reprocessing
prestats_censor[cxt]=1
```

This value will propagate to all future modules from the first instance of prestats that includes motion realignment!

### `prestats_framewise`

A number of criteria are available for measuring the quality of each frame or volume of a functional time series. `prestats_framewise` is a list of the metrics that should be used and the maximum allowable threshold for each index.

 * If censoring is enabled, volumes that exceed the threshold will be marked for censoring. If censoring is disabled, then they will be flagged and tabulated but not censored (for potential use as subject-level exclusion criteria).
 * Separate different metrics with commas (`,`), and separate each metric from its maximum allowable threshold with a colon (`:`).
 * Framewise displacement is a simple metric of the level of subject motion that occurs during each acquisition volume. It is equal to the sum of absolute values of the 6 realignment parameters, in millimeters.
 * Relative RMS displacement is another metric of framewise subject motion. It uses the RMS deviation matrix formulation of the distance between the affine transforms used to realign a pair of adjacent volumes to a reference volume as a proxy for a subject's overall level of motion during the acquisition of each volume (Jenkinson, FMRIB TR99MJ1).
 * Standardised DVARS is an indicator of the global rate of change of the BOLD signal, approximately the framewise variance over voxels of the temporal derivative.
 * FD and RMS can be measured framewise (FD/frame or RMS/frame: `fd` and `rms`) or time-wise (FD/s or RMS/s: `fds` or `rmss`).

```bash
# RMS framewise threshold 0.2 mm/frame and DVARS framewise threshold 2
prestats_framewise[cxt]=rms:0.2,dvars:2

# FD framewise threshold 0.25 mm/frame and DVARS framewise threshold 2
prestats_framewise[cxt]=fd:0.25,dvars:2

# FD time-wise threshold 0.15 mm/s and DVARS framewise threshold 2
prestats_framewise[cxt]=fds:0.15,dvars:2
```

### `prestats_censor_contig`

`prestats_censor_contig` is relevant only if censoring is enabled. `prestats_censor_contig` specifies the minimum number of contiguous volumes required per non-censored epoch. If the number of contiguous volumes in a non-censored epoch is less than `prestats_censor_contig`, then those volumes will also be censored.

```bash
# Censor all epochs shorter than 5 contiguous volumes
prestats_censor_contig[cxt]=5
```

### `prestats_rerun`

Ordinarily, each module will detect whether a particular analysis has run to completion before beginning it. If re-running is disabled, then the module will immediately skip to the next stage of analysis. Otherwise, any completed analyses will be repeated.If you change the run parameters, you should rerun any modules downstream of the change.

```bash
# Skip processing steps if the pipeline detects the expected output
prestats_rerun[cxt]=0

# Repeat all processing steps
prestats_rerun[cxt]=1
```

### `prestats_cleanup`

Modules often produce numerous intermediate temporary files and images during the course of an analysis. In many cases, these temporary files are undesirable and unnecessarily consume disk space. If cleanup is enabled, any files stamped as temporary will be deleted when a module successfully runs to completion. If a module fails to detect the output that it expects, then temporary files will be retained to facilitate error diagnosis.

```bash
# Remove temporary files
prestats_cleanup[cxt]=1

# Retain temporary files
prestats_cleanup[cxt]=0
```

### `prestats_process`

Specifies the order in preprocessing routines will be executed. Exercise discretion when using this option; unless you have a compelling reason for doing otherwise, it is recommended you use the default order.

The preprocessing order should be a string of concatenated three-character routine codes separated by hyphens (`-`). Each substring encodes a particular preprocessing routine; this feature should primarily be used to selectively run only parts of the preprocessing routine.

Permitted codes include:

 * `DVO`: discard first n volumes
 * `MPR`: compute realignment parameters (do not realign)
 * `MCO`: correct for subject motion (realign)
 * `STM`: slice timing correction
 * `BXT`: brain extraction
 * `DSP`: despike BOLD timeseries
 * `DMT`: demean/detrend BOLD timeseries
 * `SPT`: spatial filter
 * `TMP`: temporal filter"

```bash
# Default processing routine for functional connectivity
prestats_process[cxt]=DVO-MPR-STM-MCO-BXT-DMT

# Default processing routine for functional connectivity, with despiking enabled
prestats_process[cxt]=DVO-MPR-STM-MCO-BXT-DSP-DMT

# Full preprocessing routine
prestats_process[cxt]=DVO-MPR-STM-MCO-BXT-DSP-DMT-TMP-SPT

# Brain is already extracted, slice-timing and realignment already completed
prestats_process[cxt]=DSP-DMT
```
