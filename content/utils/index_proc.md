# Utilities

Unlike [modules](https://pipedocs.github.io/modules) and [functions](https://pipedocs.github.io/functions), _utilities_ are standalone image processing scripts that have been designed for use both inside and outside the pipeline context. Some utilities are wrapper scripts that combine binaries from ANTs, FSL, and AFNI to simplify certain functionalities. Other utilities are R scripts that provide functionalities outside of other common image processing libraries.

There are many undocumented utilities if you look in the code. Listed below are the
documented utilities.

## Denoising and data quality

 * [qcfc](https://pipedocs.github.io/utils/qcfc.html)
 * [qcfcDistanceDependence](https://pipedocs.github.io/utils/qcfcDistanceDependence.html)

## Image utilities: voxelwise and regional

 * [erodespare](https://pipedocs.github.io/utils/erodespare.html)
