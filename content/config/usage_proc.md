[Return to config >](https://pipedocs.github.io/config)

# Use cases for the XCP pipeline

The XCP pipeline supports a number of use cases for anatomical and functional image processing. This page is concerned with configuring analysis of a new dataset, as well as configuring common modifications to existing datasets or existing analyses. For general information about standard processing streams (e.g., activation, connectivity, anatomical), see [streams documentation](https://pipedocs.github.io/config/streams).

## Run a new sample or dataset

To run a new sample or dataset through the pipeline, you will need to create a new [cohort file](https://pipedocs.github.io/config/cohort.html). Each line of the cohort file should correspond to a different subject in the sample, and each column should correspond to either a subject identifier or an input path. Input paths can be defined either absolutely or relatively. View the [documentation](https://pipedocs.github.io/config/cohort.html) for detailed specifications. The new cohort file should be passed as an argument to the XCP front end `-c` option.

## Add to an existing sample

Sometimes, you will want to add new subjects to an existing sample that has already been processed using the pipeline system. This might be the case, for instance, if you acquire new data for an ongoing study.

If you are adding new subjects to an existing sample, _do not_ create a new cohort file for only the new subjects. If you do this, the pipeline system will separately collate group-level data for new and existing subjects. Instead, edit the existing cohort file for the pipeline, and append each new subject to the file as a new line. The pipeline system automatically detects whether it has produced expected output for each subject, so it will not repeat any processing that has already completed. Check your design file to verify that any module-level `rerun` variables are set to a value of `0`. Otherwise, the pipeline will re-run for all subjects, even those that have already completed.

## Add a new atlas or parcellation

To add a new atlas or parcellation to a particular analysis, first ensure that you have the atlas files and metadata in your `$BRAINATLAS` directory (`${XCPEDIR}/atlas` by default). If files or metadata are missing, download the missing files from the brainspaces organisation.

Next, add the new atlas to the collated spatial metadata file. To do this, run the [metadata repair utility](https://pipedocs.github.io/utils/repairMetadata.html): `${XCPEDIR}/utils/repairMetadata`

Finally, ensure that your design file correctly instructs the pipeline to include the new parcellation. Look for any [module-level variables](https://pipedocs.github.io/config/variables/scope.html) called `*_atlas`. As appropriate, ensure that those variables are set either to `all` (indicating that all atlases, including the new one, should be run) or a comma-separated series that includes the name of the new atlas. (This is the name of the directory in `$BRAINATLAS` from the first step above.)

## Add new seeds for connectivity mapping

For analysis of functional connectivity, it will sometimes be desirable to compute the voxelwise connectivity to a new seed region of interest. To do this, check your design file to ensure that the `pipeline` includes a `seed` module, and that the `seed` module is executed after any pre-processing or denoising steps.

Next, determine the [coordinate library files](https://pipedocs.github.io/space/sclib.html) used in the current analysis. Paths to these will be stored in the `seed_lib` variable. If `seed_lib` references a [subject-level variable](https://pipedocs.github.io/config/variables/scope.html) (with `[sub]`), then the path to the library will be stored in the cohort file and may be different for each subject. If `seed_lib` does not specify a containing directory, then the seed library is in `${BRAINATLAS}/coor` (by default `${XCPEDIR}/atlas/coor`).

After locating the coordinate library, edit it, adding new lines for each seed in accordance with the [spatial coordinate library specifications](https://pipedocs.github.io/space/sclib.html).

## Fork an existing pipeline

Sometimes, it can be useful to run the same sample through multiple pipeline variants, for instance to evaluate efficacy of different denoising strategies in the sample under consideration. In many cases, these pipeline variants will share many processing steps. For instance, many denoising strategies might include the same pre-processing and co-registration protocols ([`prestats`](https://pipedocs.github.io/modules/prestats) and [`coreg`](https://pipedocs.github.io/modules/coreg) modules). In this case, it is desirable not to commit unnecessary computational resources to redundant processing. This can be achieved by sharing the same module outputs among mutliple pipelines.

Sharing a few modules among multiple pipelines is called _forking_ a pipeline. A pipeline is forked using the [`forkPipeline` utility](https://pipedocs.github.io/utils/forkPipeline). Before a pipeline can be forked, it is necessary to run a pipeline variant that includes all of the shared modules. After this _template_ pipeline is run, consult documentation for [`forkPipeline`](https://pipedocs.github.io/utils/forkPipeline).

## Standard processing streams

The XCP system supports image processing streams for multiple modalities, which are linked here for reference:

 * [Functional connectivity](https://pipedocs.github.io/config/streams/fc)
 * [Anatomical](https://pipedocs.github.io/config/streams/anat)
 * [Functional activation](https://pipedocs.github.io/config/streams/task)

[Return to config >](https://pipedocs.github.io/config)
