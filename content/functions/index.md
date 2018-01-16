# Functions

_Functions_ are short, reusable, and encapsulated snippets of code that simplify specific operations within the pipeline ecosystem. For instance, functions support data I/O, metadata interfaces, and command logging. It’s not particularly important to be familiar with functions unless you are interested in developing or debugging pipeline modules and utilities.

The standard pipeline ecosystem can be loaded into a shell by reading the function library:
```
source ${XCPEDIR}/core/functions/library.sh
```
This can be helpful when developing tools for the pipeline ecosystem.

## Atlas

 * [atlas](%%BASEURL/functions/atlas.html)
 * [atlas_check](%%BASEURL/functions/atlas_check.html)
 * [atlas_parse](%%BASEURL/functions/atlas_parse.html)
 * [atlas_set](%%BASEURL/functions/atlas_set.html)
 * [load_atlas](%%BASEURL/functions/load_atlas.html)
 * [write_atlas](%%BASEURL/functions/write_atlas.html)

## Boolean

 * [cleanup](%%BASEURL/functions/cleanup.html)
 * [is_1D](%%BASEURL/functions/is_1D.html)
 * [is_image](%%BASEURL/functions/is.html)
 * [is_integer](%%BASEURL/functions/is_integer.html)
 * [is+integer](%%BASEURL/functions/is+integer.html)
 * [is_numeric](%%BASEURL/functions/is_numeric.html)
 * [is+numeric](%%BASEURL/functions/is+numeric.html)
 * [rerun](%%BASEURL/functions/rerun.html)
 * [verbose](%%BASEURL/functions/verbose.html)

## Derivatives

 * [apply_exec](%%BASEURL/functions/apply_exec.html)
 * [derivative](%%BASEURL/functions/derivative.html)
 * [derivative_cxt](%%BASEURL/functions/derivative_cxt.html)
 * [derivative_floats](%%BASEURL/functions/derivative_floats.html)
 * [derivative_inherit](%%BASEURL/functions/derivative_inherit.html)
 * [derivative_parse](%%BASEURL/functions/derivative_parse.html)
 * [derivative_set](%%BASEURL/functions/derivative_set.html)
 * [load_derivatives](%%BASEURL/functions/load_derivatives.html)
 * [write_derivative](%%BASEURL/functions/write_derivative.html)

## Execution

 * [apply_exec](%%BASEURL/functions/apply_exec.html)
 * [echo_cmd](%%BASEURL/functions/echo_cmd.html)
 * [exec_afni](%%BASEURL/functions/exec_afni.html)
 * [exec_ants](%%BASEURL/functions/exec_ants.html)
 * [exec_c3d](%%BASEURL/functions/exec_c3d.html)
 * [exec_fsl](%%BASEURL/functions/exec_fsl.html)
 * [exec_sys](%%BASEURL/functions/exec_sys.html)
 * [exec_xcp](%%BASEURL/functions/exec_xcp.html)
 * [proc_afni](%%BASEURL/functions/proc_afni.html)
 * [proc_ants](%%BASEURL/functions/proc_ants.html)
 * [proc_c3d](%%BASEURL/functions/proc_c3d.html)
 * [proc_cmd](%%BASEURL/functions/proc_cmd.html)
 * [proc_fsl](%%BASEURL/functions/proc_fsl.html)
 * [proc_xcp](%%BASEURL/functions/proc_xcp.html)

## Image processing

 * [demean_detrend](%%BASEURL/functions/demean_detrend.html)
 * [filter_temporal](%%BASEURL/functions/filter_temporal.html)
 * [remove_outliers](%%BASEURL/functions/remove_outliers.html)
 * [smooth_spatial](%%BASEURL/functions/smooth_spatial.html)
 * [smooth_spatial_prime](%%BASEURL/functions/smooth_spatial_prime.html)
 * [temporal_mask](%%BASEURL/functions/temporal_mask.html)
 * [temporal_mask_prime](%%BASEURL/functions/temporal_mask_prime.html)
 * [ts_process_prime](%%BASEURL/functions/ts_process_prime.html)
 * [zscore_image](%%BASEURL/functions/zscore_image.html)

## Metadata: JSON interface

 * [json_get](%%BASEURL/functions/json_get.html)
 * [json_get_array](%%BASEURL/functions/json_get_array.html)
 * [json_keys](%%BASEURL/functions/json_keys.html)
 * [json_merge](%%BASEURL/functions/json_merge.html)
 * [json_multiset](%%BASEURL/functions/json_multiset.html)
 * [json_object](%%BASEURL/functions/json_object.html)
 * [json_print](%%BASEURL/functions/json_print.html)
 * [json_rm](%%BASEURL/functions/json_rm.html)
 * [json_set](%%BASEURL/functions/json_set.html)
 * [json_set_array](%%BASEURL/functions/json_set_array.html)
 * [import_metadata](%%BASEURL/functions/import_metadata.html)

## Pipeline

 * [abort_stream](%%BASEURL/functions/abort_stream.html)
 * [add_reference](%%BASEURL/functions/add_reference.html)
 * [cleanup](%%BASEURL/functions/cleanup.html)
 * [cluster_submit](%%BASEURL/functions/cluster_submit.html)
 * [doi2bib](%%BASEURL/functions/doi2bib.html)
 * [import_image](%%BASEURL/functions/import_image.html)
 * [rerun](%%BASEURL/functions/rerun.html)
 * [subject_parse](%%BASEURL/functions/subject_parse.html)

## Shell

 * [abspath](%%BASEURL/functions/abspath.html)
 * [arithmetic](%%BASEURL/functions/arithmetic.html)
 * [contains](%%BASEURL/functions/contains.html)
 * [join_by](%%BASEURL/functions/join_by.html)
 * [lc_prefix](%%BASEURL/functions/lc_prefix.html)
 * [match_exact](%%BASEURL/functions/match_exact.html)
 * [matching](%%BASEURL/functions/matching.html)
 * [ninstances](%%BASEURL/functions/ninstances.html)
 * [printx](%%BASEURL/functions/printx.html)
 * [repeat](%%BASEURL/functions/repeat.html)
 * [rln](%%BASEURL/functions/rln.html)
 * [strslice](%%BASEURL/functions/strslice.html)

## Signposts

 * [routine](%%BASEURL/functions/routine.html)
 * [routine_end](%%BASEURL/functions/routine_end.html)
 * [subroutine](%%BASEURL/functions/subroutine.html)

## Space

 * [set_space](%%BASEURL/functions/set_space.html)
 * [space_get](%%BASEURL/functions/space_get.html)
 * [space_set](%%BASEURL/functions/space_set.html)
 * [transform_set](%%BASEURL/functions/transform_set.html)
 * [warpspace](%%BASEURL/functions/warpspace.html)

## Variables

 * [assign](%%BASEURL/functions/assign.html)
 * [configure](%%BASEURL/functions/configure.html)
 * [configures](%%BASEURL/functions/configures.html)
 * [define](%%BASEURL/functions/define.html)
 * [final](%%BASEURL/functions/final.html)
 * [input](%%BASEURL/functions/input.html)
 * [output](%%BASEURL/functions/output.html)
 * [process](%%BASEURL/functions/process.html)
 * [processed](%%BASEURL/functions/processed.html)
 * [qc](%%BASEURL/functions/qc.html)
 * [quality_metric](%%BASEURL/functions/quality_metric.html)
 * [require](%%BASEURL/functions/require.html)
 * [write_config](%%BASEURL/functions/write_config.html)
 * [write_config_safe](%%BASEURL/functions/write_config_safe.html)
 * [write_derivative](%%BASEURL/functions/write_derivative.html)
 * [write_output](%%BASEURL/functions/write_output.html)
