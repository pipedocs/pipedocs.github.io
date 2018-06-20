Pipeline tutorial
=================

## 0. Before you begin

To complete the pipeline tutorial, first [install the pipeline and any dependencies](https://github.com/PennBBL/xcpEngine) and ensure that `XCPEDIR` points to the pipeline install directory. Next, download [the example dataset](https://figshare.com/s/d0161bac47f98eb1830b) and assign a new environment variable, `DATADIR`, to point to the example dataset. Then, define `output_root` to point to a directory where you have write access -- this is where the pipeline will write its output. Finally, decide whether the pipeline should run on a SGE-based computing cluster; if so, define `cluster_arg="-m c"`. For instance,

```bash
export DATADIR=/data/joy/BBL-extend/xcpWorkshop/input
output_root=/data/joy/BBL-extend/xcpWorkshop/output/fc_$(whoami)
cluster_arg="-m c"
```

## 1. Running the pipeline

Let's start by running the pipeline's functional connectivity processing stream. The pipeline is run with a call to the system's front end, `xcpEngine`. Call `${XCPEDIR}/xcpEngine` to bring up the [list of available options](%%BASEURL/config/xcpEngine). These will be discussed in greater detail in the next section of the tutorial. For now, let's instruct `xcpEngine` to run the functional connectivity stream.

```bash
${XCPEDIR}/xcpEngine \
   -d ${XCPEDIR}/designs/fc-36P.dsn \
   -c ${DATADIR}/cohortParticipantsFunc.csv  \
   -o ${output_root} \
   -t 1 \
   -r ${DATADIR} \
   ${cluster_arg} \
   -a 'standard=MNI%2x2x2'
```

This call will bring up 2 validation steps: (1) a command-line check and (2) a dependency check. During the command-line check (preceded by the notification `"Constructing a pipeline based on user specifications"`), the pipeline engine validates each of the command-line options passed by the user. During the dependency check (preceded by the notification `"Checking general dependencies"`), the pipeline engine verifies that all of its dependencies are installed and well-defined in its environment.

```
Constructing a pipeline based on user specifications
····································································
· [D][${XCPEDIR}/designs/fc-36P.dsn]
· [C][${DATADIR}/cohortParticipantsFunc.csv]
· [O][${output_root}]
· [M][Executing in cluster mode]
····································································

Checking general dependencies
····································································
· Version AFNI           AFNI_2011_12_21_1014 
· Version ANTs           2.1.0.post346-g34df7 
· Version FSL            5.0.8 
· Version C3D            1.0.0 
· Version JQ             jq-1.5
· Version XCP Engine     xcpEngine-v0.7.0-prerelease
· R version              3.1.1 
· R scripting front-end  3.1.1 
· · RNifti version       0.7.1
· · optparse version     1.4.4
· · pracma version       1.9.3
· · signal version       0.7.6
····································································
All general dependencies are present.

Checking environment
All environmental variables are defined.

Checking module-specific dependencies
```

The pipeline will also log all dependency versions in a JSON file located in the group-level output directory. You can verify that the pipeline is logging dependency versions and paths by running the command shown. You can also use this metadata file to compare versions across pipeline runs and ensure consistency.

```bash
${XCPEDIR}/thirdparty/jq/jq-linux64 '.' ${output_root}/group/dependencies/*_pipelineDescription.json
```

After the input and environment validation completes, the pipeline will launch with a console splash, a notification of the [module]($$BASEURL/modules) (or processing step) that it's currently running, and (if you're running in cluster mode) a tally of the number of jobs left before that module is complete.

```
###################################################################
#  ☭  ☭  ☭  ☭  ☭  ☭  ☭  ☭  ☭  ☭  ☭  ☭  ☭  ☭  ☭  ☭  ☭  ☭  ☭  ☭  ☭  #
#                                                                 #
#  ☭                      XCP ★ ★ ★ ENGINE                     ☭  #
#                                                                 #
#  ☭  ☭  ☭  ☭  ☭  ☭  ☭  ☭  ☭  ☭  ☭  ☭  ☭  ☭  ☭  ☭  ☭  ☭  ☭  ☭  ☭  #
###################################################################



###################################################################
#  ☭  ☭  ☭  ☭  ☭  ☭  ☭  ☭  ☭  ☭  ☭  ☭  ☭  ☭  ☭  ☭  ☭  ☭  ☭  ☭  ☭  #
#                                                                 #
                  CURRENTLY PROCESSING MODULE:                     
                         ★ LOCALISER ★                             
#                                                                 #
#  ☭  ☭  ☭  ☭  ☭  ☭  ☭  ☭  ☭  ☭  ☭  ☭  ☭  ☭  ☭  ☭  ☭  ☭  ☭  ☭  ☭  #
###################################################################



[Submitting modules to queue]



...
Waiting for 5 tasks
at 16:23:21 on 19 Jun 2018
...
```

## 2. Arguments

While the pipeline is running, let's break down the call that we made to the XCP Engine. We passed a total of 6 arguments to the pipeline.

* A [design file](%%BASEURL/config/design), `-d ${XCPEDIR}/designs/fc-36P.dsn`
* A [cohort file](%%BASEURL/config/cohort), `-c ${DATADIR}/cohortParticipantsFunc.csv`
* An [output path](%%BASEURL/config/xcpEngine#xcpengine--o-output-path), `-o ${output_root}`
* A [verbosity level](%%BASEURL/config/xcpEngine#xcpengine--t-trace-verbosity-level), `-t 1`
* A [reference directory](%%BASEURL/config/xcpEngine#xcpengine--r-reference-directory), `-r ${DATADIR}`
* An [execution mode](%%BASEURL/config/xcpEngine#xcpengine--m-execution-mode), `-m c`

Let's discuss each of these, and how a user might go about selecting or preparing them.

### Design file

The design file parametrises the image processing stream. The XCP system supports multimodal image processing, including _inter alia_ [functional connectivity](%%BASEURL/config/streams/fc), [functional activation](%%BASEURL/config/streams/task), and [volumetric anatomy](%%BASEURL/config/streams/anat). How does the system know which of these available processing streams it should execute? It parses the parameters provided by the user in the design file. In our example, we parametrised the stream using the design file `${XCPEDIR}/designs/fc-36P.dsn`. We'll take a closer look at that file in a moment, but for now, let's see how to configure a new design file.

Design files can be built using the XCP configuration script, `xcpConfig`. Run `${XCPEDIR}/xcpConfig` at this time. You will be presented with a console prompt asking you to select an imaging modality. For now, let's look at the available options for functional connectivity processing. Enter the key `2`, corresponding to the value `functional connectivity`, and press ENTER. After a notice that you have selected functional connectivity configuration, you should see a set of menu items along with their default values. Let's take a look at a few of these items.

Enter `1` to bring up the dialogue for selection of a denoising stream. The denoising stream, or confound model, instructs the pipeline engine how it should model and remove noise from the functional MRI dataset. For processing functional connectivity, 6 denoising streams are available by default. You can see a view details of each stream by entering an empty string (``) in the dialogue. A side-by-side overview is also available (here on the documentation site)[%%BASEURL/config/streams/fc]. (Also be sure to consult [our benchmarking paper](https://www.ncbi.nlm.nih.gov/pubmed/28302591) and [similar work from the Fornito group](https://www.ncbi.nlm.nih.gov/pubmed/29278773) for findings regarding head-to-head performance assessments.)

For now, let's keep the 36-parameter stream that's selected by default. Enter `1` to select the `36P` stream and return to the main menu. You'll see that, in addition to selecting a denoising stream, you have the option to specify a censoring regime, a pass-band for the temporal filter, and a kernel size for spatial smoothing, among a few other options. For details about each option, select it and enter an empty string (``).

Next, let's change our design file to run some [seed-based correlation analyses](%%BASEURL/modules/seed) of the posterior cingulate cortex and ventromedial prefrontal cortex. Next to the menu option "Seed-based analysis", you will note that the current value is "None". Since we want to run seed-based analyses, let's change this option. Enter the key `5` to bring up the dialogue for seed-based correlation analysis. When prompted for a path to a [spatial coordinates library](%%BASEURL/space/sclib), enter `${XCPEDIR}/space/coor/MNI.sclib`. (If you're at the BBL, enter this path instead: `${DATADIR}/MNI.sclib`.) When you return to the main menu, you'll now see that this library is set to be run through seed-based analysis. Congratulations -- you have now changed the configuration of the pipeline!

Next, let's change the standard space of our new pipeline. It turns out that this dataset has been registered to the MNI template, so let's instruct the pipeline to use 2mm isotropic MNI as the standard space. Select `9` to enter the standard space dialogue, and look for the option corresponding to `MNI%2x2x2`. Select this option and return to the main menu.

To confirm your configuration, you just need to save your design file. Select option `10`, "Finalise pipeline". You'll be prompted to specify the path to your new design file. It's recommended that you write it to your home directory (`~`) using a name that you will remember.

Let's take a look at the design file that you just created. (Run `cat` on it, or open it in your favourite text editor.) The design has automatically been configured to run a [functional connectivity stream](%%BASEURL/config/streams/fc) to your specifications. Near the top of the file, you will find a variable called `pipeline` that should look something like: `prestats,coreg,confound,regress,fcon,reho,alff,net,roiquant,seed,norm,qcfc`. The `pipeline` variable tells the XCP system which modules it should run, and what order they should be run in.

Underneath the `pipeline` variable, you will find code blocks corresponding to each of the modules defined in `pipeline`. If you're curious as to what effects any of the variables have, just look up the variables in the documentation for the relevant pipeline module. For instance, if you want to know what the `roiquant_atlas` variable does, you can find it in the [`roiquant` module documentation](%%BASEURL/modules/roiquant). More advanced users may wish to configure the pipeline by editing design files directly rather than using the `xcpConfig` front end.

### Cohort file and reference directory

The design file instructs the pipeline as to how inputs should be processed, but the [cohort file](%%BASEURL/config/cohort) (also called a subject list) actually informs the pipeline where to find the inputs. Let's look at the cohort file that we used for this analysis.

```
id0,id1,antsct,img
19441,9768,derivatives/anat-antsct/19441/9768/struc,sub-19441/ses-9768/func/sub-19441_ses-9768_task-rest_bold.nii.gz
19442,9766,derivatives/anat-antsct/19442/9766/struc,sub-19442/ses-9766/func/sub-19442_ses-9766_task-rest_bold.nii.gz
19457,9767,derivatives/anat-antsct/19457/9767/struc,sub-19457/ses-9767/func/sub-19457_ses-9767_task-rest_bold.nii.gz
19459,9771,derivatives/anat-antsct/19459/9771/struc,sub-19459/ses-9771/func/sub-19459_ses-9771_task-rest_bold.nii.gz
19461,9773,derivatives/anat-antsct/19461/9773/struc,sub-19461/ses-9773/func/sub-19461_ses-9773_task-rest_bold.nii.gz
19463,9765,derivatives/anat-antsct/19463/9765/struc,sub-19463/ses-9765/func/sub-19463_ses-9765_task-rest_bold.nii.gz
19465,9793,derivatives/anat-antsct/19465/9793/struc,sub-19465/ses-9793/func/sub-19465_ses-9793_task-rest_bold.nii.gz
```

The cohort file is formatted as a `.csv` with 4 variables and 7 observations (subjects). The first line of the cohort file is a header that defines each of the variables. Subject identifiers are placed in columns starting with `id` and ending with a non-negative integer. For instance, the first identifier (`id0`) of the first subject is `19441`, and the second identifier (`id1`) of the first subject is `9768`.

The inputs for each subject are defined in the remaining columns, here `antsct` and `img`. `antsct` defines the path to the output files of the subject's processed ANTs Cortical Thickness pipeline (which has already been run for you, but can also be run as part of the [anatomical stream](%%BASEURL/config/streams/anat)). `img` defines the path to the main image that this pipeline will analyse. Since this is the cohort for a functional connectivity stream, the main image will be a functional image (in this case, resting state). Now, try to find the first subject's image (`ls sub-19463/ses-9765/func/sub-19463_ses-9765_task-rest_bold.nii.gz`). You will probably receive an error. This is because the paths in this cohort file have been defined relative to a reference directory.

If we look at our call to `xcpEngine`, we can see that we passed it the argument `-r ${DATADIR}`. This argument instructs `xcpEngine` to search within `${DATADIR}` for cohort paths. If you run `ls ${DATADIR}/sub-19463/ses-9765/func/sub-19463_ses-9765_task-rest_bold.nii.gz`, you should now see the first subject's functional image. Using relative paths in the cohort file is useful because it makes the cohort file more portable. If you move the entire directory where the data are stored (or you share the data with a collaborator), you (or your collaborators) won't need to make any changes to the cohort file. Instead, you will only need to change the argument to `-r`, the reference directory.

Now, let's suppose that we have already processed these 7 subjects through the pipeline system, and we acquire data for a new, 8th subject. Let's say this new subject has identifiers `19469` and `9799`. To process this new subject, DO NOT CREATE A NEW COHORT FILE. Instead, edit your existing cohort file and add the new subject as a new line at the end of the file. For our example subject, the corresponding line in the cohort file might be something like `19469,9799,derivatives/anat-antsct/19469/9799/struc,sub-19469/ses-9799/func/sub-19469_ses-9799_task-rest_bold.nii.gz`. Why edit the existing cohort file instead of creating a new one?

* The pipeline will automatically detect that it has already run for the 7 other subjects, so it will not waste computational resources on them.
* The pipeline will then collate group-level data across all 8 subjects. If you were to create a new cohort file with just the new subject, group-level data would be pulled from only that subject. Not much of a group, then.

## 3. Output files

To see what the remaining arguments to `xcpEngine` do, we will need to look at the pipeline's output. By now, the pipeline that you launched earlier will hopefully have executed to completion. Let's take a look at the output directory that you defined using the `-o` option, `${output_root}`. If you list the contents of `${output_root}`, you will find 7 subject-level output directories (corresponding to the values of the `id0` variable in the cohort file) and one group-level output directory (called `group`). (You can change the group-level output path using the additional command-line argument `-a out_group=<where you want the group-level output>`.)

Begin by looking at the subject-level output. Navigate to the first subject's output directory, `${output_root}/19441/9768`. In this directory, you will find:

* A subject-specific copy of the design file that you used to run the pipeline, evaluated and modified to correspond to this particular subject (`19441,9768`). (In the XCP system, the process of mapping the template design file to each subject is called _localisation_, and the script that handles this is called the _localiser_.)
* An atlas directory (`19441_9768_atlas`). Inside the atlas directory, each parcellation that has been analysed will exist as a NIfTI file, registered to the subject's [native space](%%BASEURL/space).
  * The pipeline can process any number of atlases. For each provided atlas, the pipeline will produce regional values for any measures of interest (including networks/adjacency matrices, regional homogeneity, or activation maps).
  * To verify that the registration succeeded, take a look at the atlas's alignment with a representative image of the sequence. Run the `fslview` command to examine the atlas overlaid on the representative image. There should be good concordance between the images, with the exception of temporopolar and orbitofrontal regions. (This deficit occurs because we did not run distortion correction on this example dataset.)
```bash
fslview ${output_root}/19441/9768/prestats/19441_9768_referenceVolume.nii.gz ${output_root}/19441/9768/19441_9768_atlas/19441_9768_schaefer400.nii.gz &
```
* A subdirectory corresponding to each pipeline module, as defined in the `pipeline` variable in the [design file](%%BASEURL/config/design). For the most part, these directories store images and files that the pipeline uses to verify successful processing.
  * Take a look inside the [`fcon`](%%BASEURL/modules/fcon) subdirectory. Inside, there will be a separate subdirectory for each of the atlases that the pipeline has processed. For instance, in the `power264` subdirectory (corresponding to the [264-node Power atlas](https://www.ncbi.nlm.nih.gov/pubmed/22099467)), there will be files suffixed `ts.1D` and `network.txt`.
  * `ts.1D` contains 264 columns corresponding to each node of the atlas; each column contains a region's functional time series.
  * `network.txt` contains the functional connectivity matrix or connectome for the Power atlas, formatted as a vector to remove redundant edges.
* A log directory (`19441_9768_logs`). Inside the log directory, open the file whose name ends with `_LOG`. This is where all of the pipeline's image processing commands are logged. [The verbosity of this log can be modified using the argument to the `-t` option](%%BASEURL/config/xcpEngine#xcpengine--t-trace-verbosity-level). It is recommended that you use a verbosity level of either 1 or 2. For most cases, 1 will be sufficient, but 2 can sometimes provide additional, lower-level diagnostic information.
* A quality file (`19441_9768_quality.csv`). The contents of the quality file will be discussed in detail later, along with group-level outputs.
* A spatial metadata file (`19441_9768_spaces.json`). The pipeline uses this to determine how to move images between different [coordinate spaces](%%BASEURL/space).
* The final output of processing (`19441_9768.nii.gz`). This is the primary functional image, after all image processing steps have been applied to it. However, this file usually isn't as useful for analysis as are its derivatives, which brings us to . . .
* An index of derivative images (`19441_9768_derivatives.json`).
  * Let's look at the content of the derivatives file now. Run the command shown, and find the entry for `reho`. This JSON object corresponds to the voxelwise map of this subject's regional homogeneity (_ReHo_).
  ```bash
  ${XCPEDIR}/thirdparty/jq/jq-linux64 '.' ${output_root}/19441/9768/19441_9768_derivatives.json
  ```
  * The map can be found in the path next to the `Map` attribute. (You can open this in `fslview` if you would like.)
  * The `Provenance` attributes tell us that the map was produced as part of the 6th pipeline module, `reho`.
  * The `Space` attribute tells us that the map is in 2mm isotropic MNI space.
  * The `Statistic` attribute instructs the pipeline's `roiquant` module that it should compute the mean value within each parcel of each atlas when converting the voxelwise derivative into an ROI-wise derivative.
  * The `Type` attribute is used by the pipeline when it makes decisions regarding interpolations and other processing steps.
  * There will actually be a separate index for each coordinate space that has been processed. Note that there's also a `19441_9768_derivatives-19441_9768_fc.json`, which has the same metadata for derivatives in the subject's native functional space.

Next, let's examine the group-level output. Navigate to `${output_root}/group`. In this directory, you will find:

* The dependency metadata from earlier (`dependencies/*pipelineDescription.json`). (A new time-stamped metadata file is generated for each run of the pipeline.)
* An error logging directory (`error`). This should hopefully be empty!
* A log directory (`log`), analogous to the log directory from the subject level.
* Module-level directories, in this case for the `roiquant` and `qcfc` modules.
  * Let's look at the group-level `roiquant` output. Like the subject-level `net` output, there will be a separate subdirectory for each atlas that has been processed.
  * Inside the atlas-level subdirectory, there will be files corresponding to any derivatives that had a non-`null` value for their `Statistic` attribute. For instance, the ReHo that we looked at earlier (`Statistic: mean`) has been quantified regionally and collated across all subjects in the file ending with the suffix `RegionalMeanReho.csv`. You may wish to examine one of these files; they are ready to be loaded into R or any other environment capable of parsing `.csv`s.
* A sample quality file for the modality (`fc_quality.csv`).
  * The `qcfc` module's subdirectory will contain reports analogous to those from our . These aren't really useful for a sample of only 7 subjects, so we won't look at them here.
* Collated subject-level quality indices (`n7_quality.csv`, not to be confused with the sample-level quality file). If you examine this file, you will find the quality indices that the functional connectivity stream tracks. This file can be used to establish exclusion criteria when building a final sample, for instance on the basis of subject movement or registration quality.
* An audit file (`n7_audit.csv`). This file indicates whether each pipeline module has successfully run for each subject. `1` indicates successful completion, while `0` indicates a nonstandard exit condition.

## 4. Anatomy of the pipeline system

Now, let's pull this information together to consider how the pipeline system operates.

1. The front end, `xcpEngine`, parses the provided [design](%%BASEURL/config/design) and [cohort](%%BASEURL/config/cohort) files. The design file can be created using the script `xcpConfig`.
2. The _localiser_ uses the information in the cohort file to generate a subject-specific version of the design file for each subject. (The localiser shifts processing from the sample level to the subject level; this is called the _localisation_ or _map_ step.)
3. `xcpEngine` parses the `pipeline` variable in the design file to determine what [modules](%%BASEURL/modules) (or processing routines) it should run. Different imaging and data modalities (e.g., anatomical, functional connectivity, task activation) will make use of a different series of modules.
4. `xcpEngine` submits a copy of each module for each subject in the cohort using that subject's local design file. Modules run in series, with all subjects running each module in parallel. As it runs, each module writes derivatives and metadata to its output directory.
5. To collate subject-level data or perform group-level analysis, the pipeline uses the _delocaliser_. Shift of processing from the subject level to the sample level is called _delocalisation_ or a _reduce_ step.

## 5. Getting help

To get help, the correct channel to use is [Github](https://github.com/PennBBL/xcpEngine/issues). Open a new issue and describe your problem. If the problem is highly dataset-specific, you can contact the development team by email, but Github is almost always the preferred channel for communicating about pipeline functionality. You can also use the issue system to request new pipeline features or suggest changes.
