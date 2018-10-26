[Return to streams >](https://pipedocs.github.io/config/streams)

# Functional activation stream

The XCP system includes a standard processing stream for functional activation (task modelling), with further configuration available using [FSL's `feat` front end](https://fsl.fmrib.ox.ac.uk/fsl/fslwiki/FEAT). The base processing stream is summarised below. The processing stream draws on FSL, AFNI, and ANTs. Consult module documentation for additional details.

<p align="center">
![Functional activation processing streams](https://pipedocs.github.io/content/images/streamsTask.png "Functional activation processing streams")
</p>

## Available modules

<p align="center">
![Functional activation processing modules](https://pipedocs.github.io/content/images/streamsTaskModules.png "Functional activation processing modules")
</p>

 * [`prestats`](https://pipedocs.github.io/modules/prestats)
 * [`task`](https://pipedocs.github.io/modules/task)
 * [`coreg`](https://pipedocs.github.io/modules/coreg)
 * [`roiquant`](https://pipedocs.github.io/modules/roiquant)
 * [`norm`](https://pipedocs.github.io/modules/norm)

Preprocessing for task analysis can be executed either in the `task` module (through FEAT, currently recommended) or using the pipeline system's `prestats` module.

## Processing routines

### Removal of initial volumes

_Module_: [`task`](https://pipedocs.github.io/modules/task) or [`prestats`](https://pipedocs.github.io/modules/prestats)

Either the initial or final volumes of the sequence are discarded in order to account for scanner equilibration.

### Motion realignment

_Module_: [`task`](https://pipedocs.github.io/modules/task) or [`prestats`](https://pipedocs.github.io/modules/prestats)

FSL's `mcflirt` aligns all acquired volumes to a reference volume that is representative of the entire sequence in order to adjust for subject movement during image acquisition. The alignment matrices are used to estimate 6 motion parameters: 3 translational (x, y, z) and 3 rotational (roll, pitch, yaw).

[Reference](https://www.ncbi.nlm.nih.gov/pubmed/12377157)

### Slice-time correction

_Module_: [`task`](https://pipedocs.github.io/modules/task) or [`prestats`](https://pipedocs.github.io/modules/prestats)

When a new 3D volume is acquired, not all 2D slices of that 3D volume are typically acquired at the same time. Slice-time correction edits each slice of the image to account for differences in acquisition time.

### Brain extraction

_Module_: [`task`](https://pipedocs.github.io/modules/task) or [`prestats`](https://pipedocs.github.io/modules/prestats)

Brain extraction uses FSL's `bet` to identify brain voxels and remove any non-brain voxels from the functional image.

[Reference](https://www.ncbi.nlm.nih.gov/pubmed/12391568)

### Temporal filter

_Module_: [`task`](https://pipedocs.github.io/modules/task) or [`prestats`](https://pipedocs.github.io/modules/prestats)

A temporal filter removes frequencies of no interest from the functional time series. Functional connectivity is typically driven by synchrony among low-frequency signals, so temporal filters often remove higher frequencies. The acceptable frequency band (filter pass-band) is configurable in `xcpConfig`.

### Spatial smoothing

_Module_: [`task`](https://pipedocs.github.io/modules/task) or [`prestats`](https://pipedocs.github.io/modules/prestats)

Spatial smoothing mitigates noise at the voxel level by enforcing spatial autocorrelation among adjacent voxels. By the same token, however, spatial smoothing also effectively reduces image resolution. The size of the smoothing kernel is configurable in `xcpConfig`. For each selected kernel size, the pipeline will produce derivatives with that level of smoothing.

### GLM fit

_Module_: [`task`](https://pipedocs.github.io/modules/task)

Any number or set of controlled stimuli that have been presented to the subject as part of an in-scanner task can be modelled by convolving the stimulus time series with a haemodynamic response function. The modelled stimuli are then fit to each voxel's observed time series in FSL using a multiple regression or general linear model (GLM). FSL's FEAT implements the GLM as part of FILM, which also can prewhiten voxelwise time series to account for autocorrelation.

[Reference 1](https://www.ncbi.nlm.nih.gov/pubmed/15501092)

[Reference 2](https://www.ncbi.nlm.nih.gov/pubmed/21979382)

### Coregistration

_Module_: [`coreg`](https://pipedocs.github.io/modules/coreg)

Coregistration is the estimation of an affine transform that aligns a representative brain map of the functional sequence to a high-resolution anatomical brain image acquired of the same subject. `xcpConfig` supports selection of the representative brain map: it can be either the single volume with the least amount of motion or the average of all volumes. `xcpConfig` also allows for refinement of the brain extraction step by combining the coregistration with the anatomical brain mask.

### Regional quantification

_Module_: [`roiquant`](https://pipedocs.github.io/modules/roiquant)

Regional quantification converts voxelwise derivative maps (for instance, parameter estimates and contrasts derived from the linear model fit) into regional values based on any number of provided parcellations. It is implemented in the XCP system's [`roiquant` module](https://pipedocs.github.io/modules/roiquant).

### Normalisation

_Module_: [`norm`](https://pipedocs.github.io/modules/norm)

Image normalisation shifts derivative maps (and potentially the primary image) into a standard sample-level or population-level space to facilitate comparisons between subjects. The normalisation step applies the affine coregistration and any diffeomorphic warps computed as part of the [anatomical stream](https://pipedocs.github.io/config/streams/anat).

[Return to streams >](https://pipedocs.github.io/config/streams)
