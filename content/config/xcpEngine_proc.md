# `xcpEngine`

`xcpEngine` is the pipeline system's front end for image processing. This is the program that parses [design](https://pipedocs.github.io/config/design) and [cohort](https://pipedocs.github.io/config/cohort) files to deploy [processing streams](https://pipedocs.github.io/config/streams) for different imaging modalities. This page documents command-line options and acceptable arguments for the front end.

## `-d`: Design file

The [design file](https://pipedocs.github.io/config/design) parametrises the [processing stream](https://pipedocs.github.io/config/streams). Standard design files are stored in `${XCPEDIR}/designs`. New design files can be generated using the `xcpConfig` system or via manual editing. [Detailed documentation is available here.](https://pipedocs.github.io/config/design)

## `-c`: Cohort file

The [cohort file](https://pipedocs.github.io/config/cohort) parametrises the pipeline's input sample, over which image processing is to be executed. Each row corresponds to a subject, and each column corresponds to a variable. [Detailed documentation is available here.](https://pipedocs.github.io/config/cohort)

## `-o`: Output path

The output path specifies the parent directory, wherein all output from the instance of the XCP system will be written. The argument to `-o` must correspond to an valid path on the current filesystem. If the indicated directory does not exist, `xcpEngine` will attempt to create it. If `xcpEngine` cannot create the indicated directory, it will abort with an error.

## `-i`: Intermediate path

The intermediate path specifies a directory that the XCP instance will use as a scratch space for storage of temporary files. Some systems operate more quickly when temporary files are written in a dedicated scratch space. The `-i` option enables a dedicated scratch space for intermediates. Like `-o`, `-i` requires an argument that is a valid path on the current filesystem. If `-i` is unspecified, temporary files will be stored in the argument to `-o`.

## `-m`: Execution mode

The execution mode instructs the front end to use either serial execution on a single machine or parallel execution on a SGE-based computing cluster. Serial execution is specified using the `s` argument (as in  `-m s`), while SGE execution is specified using the `c` argument (as in `-m c`). Currently, the pipeline defaults to serial execution in the absence of an explicit `-m` option. The `-m` option can also be used to define special job parameters for parallel execution on a computing cluster by passing a [cluster specifications file](https://pipedocs.github.io/config/cspec) as the argument to `-m`.

## `-r`: Reference directory

Ccohort files](https://pipedocs.github.io/config/cohort) can be made more portable if the input paths defined therein are defined relative to some root directory. If input paths are defined relatively rather than absolutely, then `xcpEngine` requires knowledge of the reference directory relative to which all input paths are defined. This information is provided to `xcpEngine` as the argument to the `-r` option.

## `-a`: Direct assignment

The values of single variables in a design file can be overriden via direct assignment, which is discussed in the [design file documentation](https://pipedocs.github.io/config/design). Direct assignment of variables is not generally advised. Direct assignment is enabled by passing, as the argument to `-a`, an equality with the variable to be assigned on the left-hand side and its new value on the right-hand side. For instance, to override the default value of the variable `standard` and replace it with `MNI%2x2x2`, use `-a standard=MNI%2x2x2`.

## `-t`: Trace (verbosity) level

The verbosity level instructs the XCP system and its child scripts whether it should print image processing commands as they are called. Error traces and diagnostics are printed regardless of verbosity level, but increasing the verbosity can make it easier to trace an error to its origin point.

 * `0`: The default verbosity level prints only human-readable, descriptive explanations of processing steps as they are executed.
 * `1`: Module-level trace. Any image processing commands that are called by [pipeline modules](https://pipedocs.github.io/modules) are explicitly printed to the console or log. Calls to [utility scripts](https://pipedocs.github.io/utils) are printed, but any subroutines of utilities are omitted. For module validation and enhanced diagnosis of errors at the module level.
 * `2`: Utility-level trace. Like `1`, but subroutines of [utility scripts](https://pipedocs.github.io/utils) are also explicitly printed. For utility validation and enhanced diagnosis of errors at the utility level.
 * `3`: Maximum verbosity. Absolutely every command is traced. We've found that this level of verbosity is almost never warranted and that it will usually make error diagnosis more difficult, not easier, because it's easy to lose the most relevant information in the noise.
