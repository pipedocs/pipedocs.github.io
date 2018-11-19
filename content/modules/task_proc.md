[Return to modules >](https://pipedocs.github.io//modules)

# `task`: Task-constrained analysis module

`task` executes any subject-level analysis that can be performed using FMRIB's FEAT. It accepts a FEAT design file (`.fsf`) and generalises it to every subject. It then reorganises the FEAT output directory to conform to the XCP standard.

### `task_design`

`task` distributes a template FEAT design file so that the FEAT analysis is executed for all subjects in the provided cohort file. [FEAT design files can be constructed in FMRIB's FEAT](https://fsl.fmrib.ox.ac.uk/fsl/fslwiki/FEAT/UserGuide). Subject variables defined in the cohort file may be used in the FEAT design. The `task` module will automatically set the values of the following variables in the `fsf` file using information pulled from the cohort file and inputs:

```bash
set fmri(outputdir)
set fmri(regstandard)
set fmri(alternative_example_func)
set fmri(alternative_mask)
set fmri(confoundevs)
set fmri(tr)
set fmri(npts\)
set feat_files(1)
set highres_files(1)
set confoundev_files(1)
set fmri(featwatcher_yn)
```

It is possible to use a different FSF design for every subject. This is desirable in cases where you wish to model responses or stimulus timing that varies across subjects. To use a subject-specific FSF, add a new column to the [cohort file](https://pipedocs.github.io/config/cohort.html) and title it `fsf`. Additionally, set `task_design` to `${fsf[sub]}` to reference the cohort column. This is the default setting in the example task design file that is bundled with the pipeline system.

```bash
# Use the fsf column in the cohort file as the FEAT design
task_design[cxt]=${fsf[sub]}

# Use the fractal_nback.fsf FEAT design
task_design[cxt]=fractal_nback.fsf
```

### `task_rerun`

Ordinarily, each module will detect whether a particular analysis has run to completion before beginning it. If re-running is disabled, then the module will immediately skip to the next stage of analysis. Otherwise, any completed analyses will be repeated. If you change the run parameters, you should rerun any modules downstream of the change.

```bash
# Skip processing steps if the pipeline detects the expected output
task_rerun[cxt]=0

# Repeat all processing steps
task_rerun[cxt]=1
```

### `task_cleanup`

Modules often produce numerous intermediate temporary files and images during the course of an analysis. In many cases, these temporary files are undesirable and unnecessarily consume disk space. If cleanup is enabled, any files stamped as temporary will be deleted when a module successfully runs to completion. If a module fails to detect the output that it expects, then temporary files will be retained to facilitate error diagnosis.

```bash
# Remove temporary files
task_cleanup[cxt]=1

# Retain temporary files
task_cleanup[cxt]=0
```

[Return to modules >](https://pipedocs.github.io//modules)
