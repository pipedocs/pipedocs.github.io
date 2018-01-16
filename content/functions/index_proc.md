# Functions

_Functions_ are short, reusable, and encapsulated snippets of code that simplify specific operations within the pipeline ecosystem. For instance, functions support data I/O, metadata interfaces, and command logging. It’s not particularly important to be familiar with functions unless you are interested in developing or debugging pipeline modules and utilities.

The standard pipeline ecosystem can be loaded into a shell by reading the function library:
```
source ${XCPEDIR}/core/functions/library.sh
```
This can be helpful when developing tools for the pipeline ecosystem.

## Atlas

 * [atlas](https://pipedocs.github.io/functions/atlas.html)
 * [atlas_check](https://pipedocs.github.io/functions/atlas_check.html)
 * [atlas_parse](https://pipedocs.github.io/functions/atlas_parse.html)
 * [atlas_set](https://pipedocs.github.io/functions/atlas_set.html)
 * [load_atlas](https://pipedocs.github.io/functions/load_atlas.html)
 * [write_atlas](https://pipedocs.github.io/functions/write_atlas.html)

## Boolean

 * [cleanup](https://pipedocs.github.io/functions/cleanup.html)
 * [is_1D](https://pipedocs.github.io/functions/is_1D.html)
 * [is_image](https://pipedocs.github.io/functions/is.html)
 * [is_integer](https://pipedocs.github.io/functions/is_integer.html)
 * [is+integer](https://pipedocs.github.io/functions/is+integer.html)
 * [is_numeric](https://pipedocs.github.io/functions/is_numeric.html)
 * [is+numeric](https://pipedocs.github.io/functions/is+numeric.html)
 * [rerun](https://pipedocs.github.io/functions/rerun.html)
 * [verbose](https://pipedocs.github.io/functions/verbose.html)

## Derivatives

 * [apply_exec](https://pipedocs.github.io/functions/apply_exec.html)
 * [derivative](https://pipedocs.github.io/functions/derivative.html)
 * [derivative_cxt](https://pipedocs.github.io/functions/derivative_cxt.html)
 * [derivative_floats](https://pipedocs.github.io/functions/derivative_floats.html)
 * [derivative_inherit](https://pipedocs.github.io/functions/derivative_inherit.html)
 * [derivative_parse](https://pipedocs.github.io/functions/derivative_parse.html)
 * [derivative_set](https://pipedocs.github.io/functions/derivative_set.html)
 * [load_derivatives](https://pipedocs.github.io/functions/load_derivatives.html)
 * [write_derivative](https://pipedocs.github.io/functions/write_derivative.html)

## Execution

 * [apply_exec](https://pipedocs.github.io/functions/apply_exec.html)
 * [echo_cmd](https://pipedocs.github.io/functions/echo_cmd.html)
 * [exec_afni](https://pipedocs.github.io/functions/exec_afni.html)
 * [exec_ants](https://pipedocs.github.io/functions/exec_ants.html)
 * [exec_c3d](https://pipedocs.github.io/functions/exec_c3d.html)
 * [exec_fsl](https://pipedocs.github.io/functions/exec_fsl.html)
 * [exec_sys](https://pipedocs.github.io/functions/exec_sys.html)
 * [exec_xcp](https://pipedocs.github.io/functions/exec_xcp.html)
 * [proc_afni](https://pipedocs.github.io/functions/proc_afni.html)
 * [proc_ants](https://pipedocs.github.io/functions/proc_ants.html)
 * [proc_c3d](https://pipedocs.github.io/functions/proc_c3d.html)
 * [proc_cmd](https://pipedocs.github.io/functions/proc_cmd.html)
 * [proc_fsl](https://pipedocs.github.io/functions/proc_fsl.html)
 * [proc_xcp](https://pipedocs.github.io/functions/proc_xcp.html)

## Image processing

 * [demean_detrend](https://pipedocs.github.io/functions/demean_detrend.html)
 * [filter_temporal](https://pipedocs.github.io/functions/filter_temporal.html)
 * [remove_outliers](https://pipedocs.github.io/functions/remove_outliers.html)
 * [smooth_spatial](https://pipedocs.github.io/functions/smooth_spatial.html)
 * [smooth_spatial_prime](https://pipedocs.github.io/functions/smooth_spatial_prime.html)
 * [temporal_mask](https://pipedocs.github.io/functions/temporal_mask.html)
 * [temporal_mask_prime](https://pipedocs.github.io/functions/temporal_mask_prime.html)
 * [ts_process_prime](https://pipedocs.github.io/functions/ts_process_prime.html)
 * [zscore_image](https://pipedocs.github.io/functions/zscore_image.html)

## Metadata: JSON interface

 * [json_get](https://pipedocs.github.io/functions/json_get.html)
 * [json_get_array](https://pipedocs.github.io/functions/json_get_array.html)
 * [json_keys](https://pipedocs.github.io/functions/json_keys.html)
 * [json_merge](https://pipedocs.github.io/functions/json_merge.html)
 * [json_multiset](https://pipedocs.github.io/functions/json_multiset.html)
 * [json_object](https://pipedocs.github.io/functions/json_object.html)
 * [json_print](https://pipedocs.github.io/functions/json_print.html)
 * [json_rm](https://pipedocs.github.io/functions/json_rm.html)
 * [json_set](https://pipedocs.github.io/functions/json_set.html)
 * [json_set_array](https://pipedocs.github.io/functions/json_set_array.html)
 * [import_metadata](https://pipedocs.github.io/functions/import_metadata.html)

## Pipeline

 * [abort_stream](https://pipedocs.github.io/functions/abort_stream.html)
 * [add_reference](https://pipedocs.github.io/functions/add_reference.html)
 * [cleanup](https://pipedocs.github.io/functions/cleanup.html)
 * [cluster_submit](https://pipedocs.github.io/functions/cluster_submit.html)
 * [doi2bib](https://pipedocs.github.io/functions/doi2bib.html)
 * [import_image](https://pipedocs.github.io/functions/import_image.html)
 * [rerun](https://pipedocs.github.io/functions/rerun.html)
 * [subject_parse](https://pipedocs.github.io/functions/subject_parse.html)

## Shell

 * [abspath](https://pipedocs.github.io/functions/abspath.html)
 * [arithmetic](https://pipedocs.github.io/functions/arithmetic.html)
 * [contains](https://pipedocs.github.io/functions/contains.html)
 * [join_by](https://pipedocs.github.io/functions/join_by.html)
 * [lc_prefix](https://pipedocs.github.io/functions/lc_prefix.html)
 * [match_exact](https://pipedocs.github.io/functions/match_exact.html)
 * [matching](https://pipedocs.github.io/functions/matching.html)
 * [ninstances](https://pipedocs.github.io/functions/ninstances.html)
 * [printx](https://pipedocs.github.io/functions/printx.html)
 * [repeat](https://pipedocs.github.io/functions/repeat.html)
 * [rln](https://pipedocs.github.io/functions/rln.html)
 * [strslice](https://pipedocs.github.io/functions/strslice.html)

## Signposts

 * [routine](https://pipedocs.github.io/functions/routine.html)
 * [routine_end](https://pipedocs.github.io/functions/routine_end.html)
 * [subroutine](https://pipedocs.github.io/functions/subroutine.html)

## Space

 * [set_space](https://pipedocs.github.io/functions/set_space.html)
 * [space_get](https://pipedocs.github.io/functions/space_get.html)
 * [space_set](https://pipedocs.github.io/functions/space_set.html)
 * [transform_set](https://pipedocs.github.io/functions/transform_set.html)
 * [warpspace](https://pipedocs.github.io/functions/warpspace.html)

## Variables

 * [assign](https://pipedocs.github.io/functions/assign.html)
 * [configure](https://pipedocs.github.io/functions/configure.html)
 * [configures](https://pipedocs.github.io/functions/configures.html)
 * [define](https://pipedocs.github.io/functions/define.html)
 * [final](https://pipedocs.github.io/functions/final.html)
 * [input](https://pipedocs.github.io/functions/input.html)
 * [output](https://pipedocs.github.io/functions/output.html)
 * [process](https://pipedocs.github.io/functions/process.html)
 * [processed](https://pipedocs.github.io/functions/processed.html)
 * [qc](https://pipedocs.github.io/functions/qc.html)
 * [quality_metric](https://pipedocs.github.io/functions/quality_metric.html)
 * [require](https://pipedocs.github.io/functions/require.html)
 * [write_config](https://pipedocs.github.io/functions/write_config.html)
 * [write_config_safe](https://pipedocs.github.io/functions/write_config_safe.html)
 * [write_derivative](https://pipedocs.github.io/functions/write_derivative.html)
 * [write_output](https://pipedocs.github.io/functions/write_output.html)
