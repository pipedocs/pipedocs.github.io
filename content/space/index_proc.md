# Coordinate spaces

A coordinate space (or _space_) is a frame of reference that associates anatomical and physiological landmarks with numerical coordinates. For instance, a coordinate space could define the location of the right temporal pole using the Cartesian ordered triple (39, 26, -30).

## Overview

Each brain image is situated in a particular coordinate space. When images are acquired, they are typically acquired in an arbitrary _native_ coordinate space. There is no necessary consistency or congruence between native spaces. For instance, the coordinates (0,0,0) might correspond to the anterior commissure in the native space of Image 1, while the same coordinates correspond to the genu of the corpus callosum in Image 2. This discrepancy can occur as a consequence either of differences in the position of the brain within the field of view during image acquisition or of individual differences in brain morphology. This means that anatomical comparison between images in native space is a non-trivial problem.

To facilitate comparisons, scientists typically (1) define a _standard_ coordinate space and (2) compute functions, or _transformations_ that map coordinates in each native space to coordinates in the standard space. The standard space is often defined to represent a sample average of the brain images in a data set. Some of the best-known examples of standard spaces are the MNI152 standard space and the Talairach standard space. The standard space is also called a _template_, anatomical template, or template image. Traditionally, neuroimagers used the same standard space, such as MNI152 space, regardless of the characteristics of the data sample that they were studying. This could potentially lead to bias if the sample used to generate the standard space was not well matched to the sample under investigation. However, with the advent of powerful and publicly available tools for template construction (such as ANTs), it is becoming increasingly common for investigators to create a new template for each data sample that they study.

## Setup

The pipeline install directory includes subdirectories called `space` and `atlas`. By default, the `space` directory contains a subdirectory for each standard spaces that the pipeline can use. These subdirectories contain all files and metadata necessary to define a coordinate space. Additional standard spaces can be installed by downloading the repository for each space from the `brainspaces` GitHub organisation and moving the downloaded directory into the `space` subdirectory of the pipeline install.

To use a particular standard space in a processing stream, configure the `Standard space` option in the configuration menu.

This will set the variable `standard` to include (1) the name of that space's subdirectory in `space` and (2) the resolution of that space (without leading zeros or decimal points). The name of the the space is separated from the resolution of the space with the `%` delimiter:
```bash
standard=MNI%1x1x1 # MNI 1mm isotropic
standard=PNC%9375x9375x1 # PNC space, 0.9375x0.9375x1 voxel size
```
To use multiple standard spaces, use the `via` keyword in the variable definition:
```bash
standard=MNI%2x2x2_via_OASIS%1x1x1
```
This will define the 2mm isotropic MNI space as the standard while also loading in the 1mm isotropic OASIS space and its attendant transformation functions. This approach is useful, for instance, if anatomical processing was performed using a custom template but you wish to register functional data to the MNI template.

## Main topics

 * [Spatial metadata](https://pipedocs.github.io/space/metadata.html): An overview of the way that the pipeline system tracks the coordinate space of each brain image.
 * [warpspace](https://pipedocs.github.io/functions/warpspace.html): The function that the pipeline system uses to shift an image or a set of coordinates between coordinate spaces. This is an integrative layer built on the ANTs utilities `antsApplyTransforms` and `antsApplyTransformsToPoints`.
 * [Template construction](https://pipedocs.github.io/utils/templateConstruct.html): A guide to using the pipeline system to facilitate the creation of a custom standard space for a new data set.
 * [Brain atlases and parcellations](https://pipedocs.github.io/space/atlas.html): An overview of the way that the pipeline system uses brain atlases and parcellations (to construct networks and to convert voxelwise values to regional values), as well as instructions for adding new parcellations.
