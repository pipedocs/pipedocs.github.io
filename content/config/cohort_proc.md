[Return to config >](https://pipedocs.github.io/config)

Pipeline cohort file
====================

A pipeline cohort file defines the experimental sample -- the set of subjects that the pipeline should process.

 The cohort file is formatted as `.csv` and contains:

 * A column corresponding to each category of input
 * A header naming each category of input
 * A row corresponding to each subject

## Examples

Cohort files can usually be prepared using a simple command-line call. In the future, the pipeline system will be expanded to support automated generation of cohorts given a known standard directory structure (e.g., HCP, OpenFMRI, BIDS, BBL). The contents of a cohort file will vary depending upon:

 * The imaging modality
 * The experimental objective
 * Available inputs

Examples for a few common processing cases are provided below.

### Subject identifiers

In general, all cohort files should contain a unique set of identifier variables for each unique subject. The pipeline system uses identifier variables to generate a unique output path for each input. To cast a cohort field as an identifier, give it the name `id<i>` in the cohort header, where `<i>` is a nonnegative integer. In the illustrative example, `id0` might correspond to the subject's identifier, `id1` to the time point (as in a longitudinal study), and `id2` to the scan within the session. So `DSQ,001,002` would denote the second scan during subject DSQ's first time point.

```
id0,id1,id2
ACC,001,001
ACC,002,001
DSQ,001,001
DSQ,001,002
DSQ,002,001
DSQ,002,002
CAT,001,001
```

#### Guidelines and specifications

 * There are no upper or lower limits to the number of identifier variables that can be provided, but in general it is recommended that they be ordered hierarchically. That is, subject should precede time point and not the other way around.
 * Identifiers can comprise any combination of alphanumeric characters and underscores. Any other characters should be excised or mapped to the set of valid characters.

#### Missing identifiers

If no identifier columns are provided in the cohort file, then the pipeline system will automatically assign a single identifier to each subject. Automatic identifiers are integers beginning with `9001` and ascending in increments of 1 for each row. So row 1 is assigned `9001`, row 2 is assigned `9002`, and row 2133 is assigned `11133`.

### Path definitions

Paths defined in a cohort file can be specified either as absolute paths or as relative paths. For portability, relative path definitions are recommended where possible. If relative paths are provided, then the call to `xcpEngine` should include the `-r` flag, which accepts as its argument the path relative to which cohort paths were defined. For instance, the provided example would yield a value of `/data/studies/example/raw/ACC_001_001_anat.nii.gz` for `img`.
```
-r /data/studies/example/

id0,id1,id2,img
ACC,001,001,raw/ACC_001_001_anat.nii.gz
```

### Anatomical processing

For anatomical processing, the cohort file is quite minimal: only the subject's anatomical image is required in addition to the set of identifiers. The subject's anatomical image should receive the header `img`.

```
id0,id1,id2,img
ACC,001,001,raw/ACC_001_001_anat.nii.gz
DSQ,001,001,raw/DSQ_001_001_anat.nii.gz
DSQ,001,002,raw/DSQ_001_002_anat.nii.gz
CAT,001,001,raw/CAT_001_001_anat.nii.gz
```

### Functional processing

For functional processing, the cohort file should include not only the subject's functional time series, but also the outputs from anatomical processing. If anatomical processing was performed within the pipeline system, or if it was performed using the ANTs Cortical Thickness pipeline, only one additional column is required. This column should receive the header `antsct` and should include the path to the immediate directory containing the output from anatomical processing. The subject's functional time series should receive the header `img`.
```
id0,id1,id2,img,antsct
ACC,001,001,raw/ACC_001_001_rest.nii.gz,proc/ACC_001_001_antsct
DSQ,001,001,raw/DSQ_001_001_rest.nii.gz,proc/DSQ_001_001_antsct
DSQ,001,002,raw/DSQ_001_002_rest.nii.gz,proc/DSQ_001_002_antsct
CAT,001,001,raw/CAT_001_001_rest.nii.gz,proc/CAT_001_001_antsct
```

If anatomical processing was performed externally, it will be necessary to ensure that all inputs required for a functional processing stream are provided with the appropriate headers. These include:

 * `struct       :` The subject's fully processed, bias field-corrected, brain-extracted anatomical image.
 * `segmentation :` A 3- or 6-class segmentation of the subject's anatomical image into tissue classes. 
 * `xfm_affine   :` An ANTs-formatted affine transformation from the subject's anatomical space to a template.
 * `xfm_warp     :` An ANTs-formatted deformation field from the subject's anatomical space to a template.
 * `ixfm_affine  :` An ANTs-formatted affine transformation from a template to the subject's anatomical space.
 * `ixfm_warp    :` An ANTs-formatted deformation field from a template to the subject's anatomical space.
 
(If you include `antsct` in your cohort file, you can still use and access any of the 6 above variables.)

## Subject variables

Each of the columns in the cohort file becomes a _subject variable_ at runtime. Subject variables can be used in the [design file](https://pipedocs.github.io/config/design.html) to assign a parameter subject-specific values. For instance, the `coreg_segmentation` parameter in the `coreg` [module](https://pipedocs.github.io/modules/index.html) can be assigned the `segmentation` subject variable. To indicate that the assignment is a subject variable, include the array index `[sub]` in the variable's name as shown.
``` bash
coreg_segmentation[2]=${segmentation[sub]}
```

[Return to config >](https://pipedocs.github.io/config)
