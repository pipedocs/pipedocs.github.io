[Return to config >](https://pipedocs.github.io//config)

# Processing streams

Neuroimage processing refers collectively to the set of strategies used to convert the "raw" images collected from the scanner into appropriate inputs to group-level statistical analyses. A _processing stream_ is the specific set of routines that are selected to extract a desired modality of analytic data from a neuroimage.

Complementary bodies of information can be obtained from the same source image if the image is subject to different processing streams. For instance, a functional image can be processed either using an [activation](https://pipedocs.github.io/config/streams/task) stream, which returns information about the responses of brain regions to a stimulus, or using a [connectivity](https://pipedocs.github.io/config/streams/fc) stream, which returns information about synchrony among different brain regions and can be performed in the presence or absence of a controlled stimulus.

In the XCP system, a standard set of processing streams is available for each supported imaging modality. Each processing stream is parametrised by a [design file](https://pipedocs.github.io/config/design). Detailed information about standard implementations of multimodal processing streams is available at the links below, organised by imaging modality.

[Anatomical >](https://pipedocs.github.io/config/streams/anat)

[Functional Connectivity >](https://pipedocs.github.io/config/streams/fc)

[Functional Activation >](https://pipedocs.github.io/config/streams/task)

[Perfusion >](https://pipedocs.github.io/config/streams/cbf) \[UNRELEASED\]

[Diffusion >](https://pipedocs.github.io/config/streams/diffusion) \[UNRELEASED\]

[Return to config >](https://pipedocs.github.io//config)
