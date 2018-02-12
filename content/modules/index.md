# Modules

Pipeline _modules_ are self-contained image processing routines. An image processing pipeline is created by combining desired modules; modules are the building blocks of an image processing pipeline. Each module either (a) processes the main brain image (the _analyte_), for instance by filtering or denoising it, or (b) uses the analyte to produce another dataset, called a _derivative_. (It’s also possible for modules to use derivatives to produce further derivatives).

## Functional image processing

Modules that process the analyte for functional data.

 * [`prestats`](%%BASEURL/modules/prestats.html): Omnibus module for functional preprocessing. Incorporates removal of volumes, skull-stripping, outlier removal, temporal masking, motion estimation and correction, temporal and spatial filtering, slice time correction.
 * [`confound`](%%BASEURL/modules/confound.html): Generates a matrix of nuisance time series for confound regression. Supports most frequently used denoising models, including realignment parameters, tissue-based time series, PCA-derived time series, the global signal, and temporal expansions.
 * [`aroma`](%%BASEURL/modules/aroma.html): Implements ICA-AROMA-like denoising of functional time series data.
 * [`regress`](%%BASEURL/modules/regress.html): Executes confound regression using the matrix generated by the `confound` module. Censors any volumes flagged for poor data quality. Incorporates spatial and temporal filtering.
 * [`task`](%%BASEURL/modules/task.html): Implements `prestats`-like functionality in preparation for task-based activation analysis in FSL’s FEAT. (Also produces derivatives.)
 
## Functional derivatives

Modules that generate derivative maps from functional data.

 * [`reho`](%%BASEURL/modules/reho.html): Computes voxelwise regional homogeneity.
 * [`alff`](%%BASEURL/modules/alff.html): Computes the amplitude of low-frequency fluctuations voxelwise.
 * [`task`](%%BASEURL/modules/task.html): Computes voxelwise activation maps for task-based models using FSL’s FEAT. Computes PEs, CoPEs, varCoPEs, z-statistics, and percent signal change maps. Distributes and augments any FEAT `.fsf` design. (Also supports preprocessing.)
 * [`cbf`](%%BASEURL/modules/cbf.html): Performs quantification of cerebral blood flow for ASL sequences (e.g., PCASL).
 * [`seed`](%%BASEURL/modules/seed.html): Computes functional connectivity to an _a priori_ set of seed regions using seed-based correlation analysis.

## Anatomical image processing

Modules that process the analyte for anatomical data.

 * [`struc`](%%BASEURL/modules/struc.html): Omnibus module for volumetric anatomical preprocessing. Leverages ANTs to execute N4 bias field correction, SyN diffeomorphic registration, Atropos segmentation (prior-driven or priorless), or the complete ANTs Cortical Thickness pipeline.
 
## Anatomical derivatives

Modules that generate derivative maps from anatomical data.

 * [`jlf`](%%BASEURL/modules/jlf.html): Uses the ANTs Joint Label Fusion algorithm to produce a high-resolution anatomical segmentation of the subject’s anatomical data. Generates a subject-specific atlas of anatomical landmarks that can be used for regional quantification or network mapping.
 * [`gmd`](%%BASEURL/modules/gmd.html): Computes voxelwise grey matter density.
 * [`sulc`](%%BASEURL/modules/sulc.html): (highly experimental) Computes voxelwise sulcal depth.
 * [`cortcon`](%%BASEURL/modules/cortcon.html): (highly experimental) Computes the voxelwise GM-WM tissue intensity contrast across the GM-WM cortical interface.

## Registration

Modules that generate transforms between different coordinate spaces, or that apply those transforms.

 * [`coreg`](%%BASEURL/modules/coreg.html): Co-registers the primary analyte image to a high-resolution reference image of the same subject’s brain by computing an affine transformation between the analyte and the high-resolution reference. (Combine with the deformation field obtained from `struc` to warp any analyte to a standard space.)
 * [`struc`](%%BASEURL/modules/struc.html): Computes transforms between a high-resolution anatomical image and a template image representing a standard coordinate space using the top-performing SyN algorithm. (Combine with the affine matrix obtained from `coreg` to warp any analyte to a standard space.)
 * [`norm`](%%BASEURL/modules/norm.html): Applies the requisite transforms (computed by `struc` and/or `coreg`) to shift all derivative maps from subject native space to a standard coordinate space.
 
## Regional quantification

 * [`roiquant`](%%BASEURL/modules/roiquant.html): Uses provided brain atlases or parcellations to compute, for each voxelwise derivative, a value for each region of interest in each provided brain atlas or parcellation. Converts voxelwise derivatives to regional derivatives.

## Connectomics and networks

Modules that map or analyse brain networks.

 * [`fcon`](%%BASEURL/modules/fcon.html): Computes the functional connectivity between each pair of regions in each provided brain atlas or parcellation to produce an adjacency matrix for the functional connectome. Computes dynamic FC using the MTD.
 * [`net`](%%BASEURL/modules/net.html): Operates on an adjacency matrix. Detects community structure and calculates node-wise, edge-wise, community-wise, and graph-wise network properties. Inspired by BCT.

## Quality assessment

Modules that produce estimates of data quality.

 * [`qcfc`](%%BASEURL/modules/qcfc.html): Quality assessment for functional connectivity. Generates voxelwise plots, QC-FC measures, and QC-FC estimates of distance-dependence to facilitate diagnosis of motion-related contamination and assessment of denoising efficacy.
 * [`qcanat`](%%BASEURL/modules/qcanat.html): Quality assessment for anatomical images inspired by QAP.