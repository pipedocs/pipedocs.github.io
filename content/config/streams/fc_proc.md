[Return to streams >](https://pipedocs.github.io/config/streams)

# Functional connectivity streams

The XCP system includes 6 standard processing streams for functional connectivity, with 8 additional parameters that are configurable using `xcpConfig`. These base processing streams are summarised below. All processing streams draw on FSL, AFNI, and ANTs. Base processing streams can be modified at will to suit the dataset that is to be processed. Consult module documentation for additional details.

<p align="center">
![Functional connectivity processing streams](https://pipedocs.github.io/content/images/streamsFC.png "Functional connectivity processing streams")
</p>

Subject movement introduces a substantial amount of spurious variance into the BOLD signal; if data processing fails to account for the influence of motion-related variance, then artefact may subsequently be misconstrued as effect of interest. Accordingly, a number of high-performing processing streams for removing motion artefact are made available in the XCP system. Three families of denoising streams are available:

 * High-parameter streams (`36P`) combine frame-to-frame motion estimates, mean signals from white matter and cerebrospinal fluid, the mean global signal, and quadratic and derivative expansions of these signals.
   * [Reference 1](https://www.ncbi.nlm.nih.gov/pubmed/22926292)
   * [Reference 2](https://www.ncbi.nlm.nih.gov/pubmed/23994314)
 * Anatomical component-based correction (`aCompCor`) identifies sources of signal variance in white matter and cerebrospinal fluid using principal component analysis (PCA). A sufficient number of signals to explain 50 percent of variance in white matter and cerebrospinal fluid are included in the denoising model, as are motion estimates and their derivatives. Global signal regression can be enabled (`aCompCor50+gsr`) or disabled (`aCompCor50`).
   * [Reference 1](https://www.ncbi.nlm.nih.gov/pubmed/17560126)
   * [Reference 2](https://www.ncbi.nlm.nih.gov/pubmed/24657780)
 * ICA-AROMA (`aroma`) identifies sources of variance across the brain using independent component analysis (ICA), and then uses a heuristic to classify each source as either noise or signal of interest. Noise sources are included in the denoising model along with mean signal from white matter and CSF. Global signal regression can be enabled (`aroma+gsr`) or disabled (`aroma`).
   * [Reference](https://www.ncbi.nlm.nih.gov/pubmed/25770991)
 * The 24-parameter stream (`24P`) is never recommended, as previous investigations have not found it to perform well.
   * [Reference 1](https://www.ncbi.nlm.nih.gov/pubmed/28302591)
   * [Reference 2](https://www.ncbi.nlm.nih.gov/pubmed/29278773)
 * All streams can be supplemented with despiking or framewise censoring.
   * [Reference 1](https://www.ncbi.nlm.nih.gov/pubmed/17490845)
   * [Reference 2](https://www.ncbi.nlm.nih.gov/pubmed/22019881)

## Available modules

<p align="center">
![Functional connectivity processing modules](https://pipedocs.github.io/content/images/streamsFCModules.png "Functional connectivity processing modules")
</p>

 * [`prestats`](https://pipedocs.github.io/modules/prestats)
 * [`coreg`](https://pipedocs.github.io/modules/coreg)
 * [`aroma`](https://pipedocs.github.io/modules/aroma)
 * [`confound`](https://pipedocs.github.io/modules/confound)
 * [`regress`](https://pipedocs.github.io/modules/regress)
 * [`reho`](https://pipedocs.github.io/modules/reho)
 * [`alff`](https://pipedocs.github.io/modules/alff)
 * [`seed`](https://pipedocs.github.io/modules/seed)
 * [`fcon`](https://pipedocs.github.io/modules/fcon)
 * [`net`](https://pipedocs.github.io/modules/net)
 * [`roiquant`](https://pipedocs.github.io/modules/roiquant)
 * [`norm`](https://pipedocs.github.io/modules/norm)
 * [`qcfc`](https://pipedocs.github.io/modules/qcfc)

## Processing routines

### Removal of initial volumes

### Motion realignment

### Slice-time correction

### Brain extraction

### Demeaning / detrending

### Censoring or despiking

### Coregistration

### ICA-AROMA

### 6 realignment parameters

### Mean WM / CSF signal

### Mean global signal

### aCompCor

### Mathematical expansions

### Temporal filtering

### Spatial smoothing

### Functional networks

### Seed-based correlation

### Regional homogeneity

### ALFF (Amplitude of low-frequency fluctuations)

### Regional quantification

### Normalisation

### Quality assessment

[Return to streams >](https://pipedocs.github.io/config/streams)
