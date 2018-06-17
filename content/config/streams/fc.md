# Functional connectivity processing streams

The XCP system includes 6 standard processing streams for functional connectivity, with 8 additional parameters that are configurable using `xcpConfig`. These base processing streams are summarised below. All processing streams draw on FSL, AFNI, and ANTs. Base processing streams can be modified at will to suit the dataset that is to be processed. Consult module documentation for additional details.

<p align="center">
![Functional connectivity processing streams](%%IMAGE/streamsFC.png "Functional connectivity processing streams")
</p>

Subject movement introduces a substantial amount of spurious variance into the BOLD signal; if data processing fails to account for the influence of motion-related variance, then artefact may subsequently be misconstrued as effect of interest. Accordingly, a number of high-performing processing streams for removing motion artefact are made available in the XCP system. Three families of denoising streams are available:

 * High-parameter streams (`36P`) combine frame-to-frame motion estimates, mean signals from white matter and cerebrospinal fluid, the mean global signal, and quadratic and derivative expansions of these signals. (Reference 1) (Reference 2)
 * Anatomical component-based correction (`aCompCor`) identifies sources of signal variance in white matter and cerebrospinal fluid using principal component analysis (PCA). A sufficient number of signals to explain 50 percent of variance in white matter and cerebrospinal fluid are included in the denoising model, as are motion estimates and their derivatives. Global signal regression can be enabled (`aCompCor50+gsr`) or disabled (`aCompCor50`). (Reference)
 * ICA-AROMA (`aroma`) identifies sources of variance across the brain using independent component analysis (ICA), and then uses a heuristic to classify each source as either noise or signal of interest. Noise sources are included in the denoising model along with mean signal from white matter and CSF. Global signal regression can be enabled (`aroma+gsr`) or disabled (`aroma`). (Reference)
 * The 24-parameter stream (`24P`) is never recommended, as previous investigations have not found it to perform well. (Reference)
 * All streams can be supplemented with despiking or framewise censoring. (Reference)

## Available modules

<p align="center">
![Functional connectivity processing modules](%%IMAGE/streamsFCModules.png "Functional connectivity processing modules")
</p>

 * [`prestats`](%%BASEURL/modules/prestats)
 * [`coreg`](%%BASEURL/modules/coreg)
 * [`aroma`](%%BASEURL/modules/aroma)
 * [`confound`](%%BASEURL/modules/confound)
 * [`regress`](%%BASEURL/modules/regress)
 * [`reho`](%%BASEURL/modules/reho)
 * [`alff`](%%BASEURL/modules/alff)
 * [`seed`](%%BASEURL/modules/seed)
 * [`fcon`](%%BASEURL/modules/fcon)
 * [`net`](%%BASEURL/modules/net)
 * [`roiquant`](%%BASEURL/modules/roiquant)
 * [`norm`](%%BASEURL/modules/norm)
 * [`qcfc`](%%BASEURL/modules/qcfc)
 
 
