[Return to modules >](https://pipedocs.github.io/modules)

# `aroma`: ICA-AROMA module

`aroma` performs an ICA-based denoising procedure inspired by the ICA-AROMA strategy developed by Pruim and colleagues. ICA-AROMA decomposes fMRI time series data into spatial independent components using FSL's MELODIC. It then classifies each component as signal or noise on the basis of four features: correlation with motion parameters, fraction of the component in CSF, fraction of the component near the edge of the brain, and high-frequency content. If your work makes use of this module, please cite [the original study by Pruim and colleagues]((https://www.ncbi.nlm.nih.gov/pubmed/25770991)).

## Module configuration

### `aroma_dim`

_ICA model order._

The order of the ICA model refers to the number of components into which the timeseries is decomposed. A smaller model order (20-30) will typically return fewer components that are more likely to exhibit a one-to-one correspondence to intrinsic connectivity networks, while a higher model order (c. 100) will return components with finer spatial resolution, many of which correspond to each network. Rather than constraining the decomposition based on an a priori hypothesis, it is also possible to automatically estimate the model order using a data-driven approach (the Laplace approximation to the Bayesian evidence). This is the course of action taken in the original implementation of ICA-AROMA.

```bash
# Decompose into 100 independent components
aroma_dim[cxt]=100

# Estimate dimensionality from the data
aroma_dim[cxt]=auto
```

### `aroma_dtype`

_Aggressive or nonaggressive denoising._

If nonaggressive denoising is selected, then both 'signal' and 'noise' components will be included in the regression model, and the timeseries will be residualised using the partial parameter estimates corresponding to the 'noise' components only. By contrast, aggressive denoising uses full instead of partial regression of noise component timeseries. The original implementation of ICA-AROMA uses the non-aggressive option.

```bash
# Use nonaggressive denoising
aroma_dtype[cxt]=nonaggr

# Use aggressive denoising
aroma_dtype[cxt]=aggr
```

### `aroma_dmdt`

_Demean/detrend order._

Scanner drift may introduce linear or polynomial trends into time series data; these trends may be removed using a general linear model-based fit after ICA-AROMA is executed. Notably, removal of trends in this manner will also remove the time series mean from the data, which may be undesirable for analyses in which absolute intensity is important. (The time series mean can be added back during the `regress` module.) `aroma_dmdt` must be a nonnegative integer.

Demeaning the time series is desirable in instances where a Butterworth, Chebyshev, or elliptic filter will be applied.

```bash
# Demean only
aroma_dmdt[cxt]=0

# Demean and linear detrend
aroma_dmdt[cxt]=1

# Demean and linear, quadratic, and cubic detrend
aroma_dmdt[cxt]=3

# Automatically estimate detrend order
aroma_dmdt[cxt]=auto
```

 * An order of 0 results in demeaning only.
 * Entering `auto` will automatically compute an appropriate value (following AFNI) based on the duration of the scan. The formula used to compute this is: `floor(1 + TR*nVOLS / 150)`
 * To skip this step altogether, edit DMT out of the processing order.
 * If censoring is enabled, then the model will ignore the fit to any volumes flagged for censoring.

### `aroma_1ddt`

_Detrend 1D derivatives._

`aroma_1ddt` specifies whether 1-dimensional derivatives generated from the main time series should also be demeaned and detrended along with the main time series after ICA-AROMA is eecuted. For instance, this flag would demean and detrend any realignment parameters.

```bash
# Demean and detrend 1D derivatives
aroma_1ddt[cxt]=1

# Demean and detrend only the main time series
aroma_1ddt[cxt]=0
```

### `aroma_sptf` and `aroma_smo`

_Spatial smoothing parameters: ICA-AROMA only._

Endemic noise can introduce spurious or artefactual results in single voxels. The effects of noise-related artefacts can be mitigated by spatially filtering the data, thus dramatically increasing the signal-to-noise ratio. However, spatial smoothing is not without its costs: it effectively reduces volumetric resolution by blurring signals from adjacent voxels.

The original implementation of ICA-AROMA requires data to be smoothed to FWHM 6 mm. The smoothing procedure supports classification of components as either signal or noise. In order to follow the original implementation of ICA-AROMA, *do not* use these parameters; set `aroma_smo` to a value of `0`. Instead, set `prestats_smo` to `6`. This will result in persistence of the smoothed image, as in the original ICA-AROMA implementation. `aroma_sptf` and `aroma_smo`, by contrast, support an alternative processing stream. Specifically, these settings allow the smoothed image to be used for component extraction and classification, while the original, unsmoothed image is denoised and retained for further processing. In principle, this results in ICA-AROMA-like de-noising without as much loss of spatial specificity or signal exchange across parcel boundaries.
 
```bash
# No smoothing
aroma_sptf[cxt]=none
aroma_smo[cxt]=0

# Gaussian kernel (fslmaths) of FWHM 6 mm
aroma_sptf[cxt]=gaussian
aroma_smo[cxt]=6

# SUSAN kernel (FSL's SUSAN) of FWHM 6 mm
aroma_sptf[cxt]=susan
aroma_smo[cxt]=6

# Uniform kernel (AFNI's 3dBlurToFWHM) of FWHM 6 mm
aroma_sptf[cxt]=uniform
aroma_smo[cxt]=6
```

`aroma_sptf` specifies the type of spatial filter to apply for smoothing, while `aroma_smo` specifies the full-width at half-maximum (FWHM) of the smoothing kernel in mm. All spatial filtering is performed prior to ICA-AROMA, and the smoothed image is used only for ICA decomposition and component classification; the unsmoothed image is denoised.

 * Gaussian smoothing applies the same Gaussian smoothing kernel across the entire volume.
 * SUSAN-based smoothing restricts mixing of signals from disparate tissue classes (Smith and Brady, 1997).
 * Uniform smoothing applies smoothing to all voxels until the smoothness computed at every voxel attains the target value.
 * Uniform smoothing may be used as a compensatory mechanism to reduce the effects of subject motion on the final processed image (Scheinost et al., 2014).

### `aroma_rerun`

Ordinarily, each module will detect whether a particular analysis has run to completion before beginning it. If re-running is disabled, then the module will immediately skip to the next stage of analysis. Otherwise, any completed analyses will be repeated.If you change the run parameters, you should rerun any modules downstream of the change.

```bash
# Skip processing steps if the pipeline detects the expected output
aroma_rerun[cxt]=0

# Repeat all processing steps
aroma_rerun[cxt]=1
```

### `aroma_cleanup`

Modules often produce numerous intermediate temporary files and images during the course of an analysis. In many cases, these temporary files are undesirable and unnecessarily consume disk space. If cleanup is enabled, any files stamped as temporary will be deleted when a module successfully runs to completion. If a module fails to detect the output that it expects, then temporary files will be retained to facilitate error diagnosis.

```bash
# Remove temporary files
aroma_cleanup[cxt]=1

# Retain temporary files
aroma_cleanup[cxt]=0
```

[Return to modules >](https://pipedocs.github.io/modules)
