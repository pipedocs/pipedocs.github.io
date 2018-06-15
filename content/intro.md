# BBL Imaging Pipeline

The BBL imaging pipeline (XCP system) is a free, open-source software package for processing of multimodal neuroimages. The XCP system uses a [modular design](%%BASEURL/modules/index.html) to deploy analytic routines from leading MRI analysis platforms, including FSL, AFNI, and ANTs.

The XCP system is designed to run in the Linux bash shell. Users provide an input data set and specify parameters for the analysis that they wish to perform on that data set, and the XCP Engine parses the user-provided parameters to build a processing pipeline. The XCP system supports a number of pipeline modalities, including [functional connectivity](%%BASEURL/config/streams/fc), [volumetric anatomy](%%BASEURL/config/streams/anat), [task-based activation](%%BASEURL/config/streams/task), and perfusion.

## Neuroimage processing

<p align="center">
![Raw to processed time series](%%IMAGE/tsRawToProcessed.png "Raw to processed time series")
</p>

Neuroimage processing refers collectively to the set of strategies used to convert the "raw" images collected from the scanner into appropriate inputs to group-level statistical analyses. The results of any group-level neuroimaging analysis will be sensitive to the combination of strategies used to process the subject-level images. Reproducibility of processing strategies is therefore critical.

MRI time series collected from the scanner are typically noisy. _Processing_ mitigates the effect of artefact and moves all subject-specific images into a common atlas space to facilitate direct voxelwise comparison.

It encompasses denoising, filtering, registration, and production of any subject-level derivative maps (for instance, seed-based connectivity maps or task contrasts). Processing includes production of summary measures on both a voxelwise and ROI-wise basis. Notably, processing does not include group-level statistical analysis.

## Processing pipelines

The XCP system was designed with the importance of scientific reproducibility in mind. Often, research groups process neuroimages using a dedicated, standalone "pipeline" for each image modality. For instance, task activation, perfusion, and functional connectivity analyses might each be processed by a separate script.

<p align="center">
![Non-modular pipelines](%%IMAGE/pipelinesOld.png "Non-modular pipelines")
</p>

_A standard, non-modular approach to developing pipelines with shared routines._

However, this approach can easily lead to inconsistencies in output directory conventions, difficulty tracking pipeline versions, and limited flexibility in updating pipelines, all of which ultimately combine to compound reproducibility challenges. Often, many common routines are deployed across multiple MRI modalities, and it is in the interest of the investigator to minimise redundancy and maximise reproducibility.

<p align="center">
![A modular pipeline system](%%IMAGE/choosingModules.gif "A modular pipeline system")
</p>

_In the XCP system, select the modules that you want to use and recombine them into the pipeline that best suits your processing needs._

The modular, atomic design of the XCP system allows for streamlined reproduction and recombination of frequently used image processing strategies across multiple pipelines and modalities.

## Features

The XCP system aims to provide a multimodal library of common processing routines that will improve scientific reproducibility. Features include:

 * Standardised output directory structure and naming conventions
 * Automatic version and dependency tracking
 * Systematised quality control variables computed and collated for each analysis (for easy identification of motion and registration outliers)
 * Region-wise quantification of any voxelwise derivative map for any number of parcellation schemes or regions of interest
 * Easy addition of new regions of interest, node systems, or parcellation schemes
 * _Ad hoc_ generation of spherical ROI maps and node systems
 * Analyses either in standard/atlas or subject/native space
 * Registration of images of any modality to a common template

<p align="center">
![Standardised directory structure](%%IMAGE/directoryStructure.png "Standardised directory structure")
</p>

_The XCP system is agnostic to the directory structure of input data, but produces a consistent and intuitive output directory structure._

[Install the XCP system >](https://github.com/PennBBL/xcpEngine)

[Continue to pipeline configuration >](%%BASEURL/config)
