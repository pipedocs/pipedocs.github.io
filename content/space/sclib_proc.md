[Return to space >](https://pipedocs.github.io//space)

# Spatial coordinate libraries (`.sclib`)

A spatial coordinate library (`.sclib` file) provides a concise (but admittedly not intuitive) way of encoding information about spatial coordinates for use by the processing system. The format is hacky and lacks a good parser, but it works. It is possible that it will be superseded by a more robust and intuitive system in the future.

### The `sclib` header: defining the default space

The header of the `sclib` file defines the reference coordinate space in which all coordinates in the library are defined. The space should be defined as a subdirectory of `$BRAINSPACES`, typically located at `$XCPEDIR/space`.

[Return to space >](https://pipedocs.github.io//space)
