# Utilities

Unlike [modules](https://pipedocs.github.io/modules) and [functions](https://pipedocs.github.io/functions), _utilities_ are standalone image processing scripts that have been designed for use both inside and outside the pipeline context. Some utilities are wrapper scripts that combine binaries from ANTs, FSL, and AFNI to simplify certain functionalities. Other utilities are R scripts that provide functionalities outside of other common image processing libraries.

## Denoising and data quality

 * [censor](https://pipedocs.github.io/utils/censor.html)
 * [dvars](https://pipedocs.github.io/utils/dvars.html)
 * [fd](https://pipedocs.github.io/utils/fd.html)
 * [interpolate](https://pipedocs.github.io/utils/interpolate.html)
 * [maskOverlap](https://pipedocs.github.io/utils/maskOverlap.html)
 * [mbind](https://pipedocs.github.io/utils/mbind.html)
 * [nodeCoverage](https://pipedocs.github.io/utils/nodeCoverage.html)
 * [qcanat](https://pipedocs.github.io/utils/qcanat.html)
 * [qcfc](https://pipedocs.github.io/utils/qcfc.html)
 * [regslicer](https://pipedocs.github.io/utils/regslicer.html)
 * [tmask](https://pipedocs.github.io/utils/tmask.html)
 * [tmask2spkreg](https://pipedocs.github.io/utils/tmask2spkreg.html)
 * [voxts](https://pipedocs.github.io/utils/voxts.html)

## Image utilities: voxelwise and regional

 * [erodespare](https://pipedocs.github.io/utils/erodespare.html)
 * [layerLabels](https://pipedocs.github.io/utils/layerLabels.html)
 * [perfusion](https://pipedocs.github.io/utils/perfusion.html)
 * [quantifyAtlas](https://pipedocs.github.io/utils/quantifyAtlas.html)
 * [rs2rq](https://pipedocs.github.io/utils/rs2rq.html)
 * [sfilter](https://pipedocs.github.io/utils/sfilter.html)
 * [sulcalDepth](https://pipedocs.github.io/utils/sulcalDepth.html)
 * [telescope](https://pipedocs.github.io/utils/telescope.html)
 * [tswmean](https://pipedocs.github.io/utils/tswmean.html)
 * [unique](https://pipedocs.github.io/utils/unique.html)
 * [val2mask](https://pipedocs.github.io/utils/val2mask.html)
 * [val2pct](https://pipedocs.github.io/utils/val2pct.html)
 
## Metadata and pipelines

 * [atlasMetadata](https://pipedocs.github.io/utils/atlasMetadata.html)
 * [forkPipeline](https://pipedocs.github.io/utils/forkPipeline.html)
 * [qstatus](https://pipedocs.github.io/utils/qstatus.html)
 * [removeDuplicates](https://pipedocs.github.io/utils/removeDuplicates.html)
 * [repairMetadata](https://pipedocs.github.io/utils/repairMetadata.html)
 * [spaceMetadata](https://pipedocs.github.io/utils/spaceMetadata.html)
 * [warprouter](https://pipedocs.github.io/utils/warprouter.html)

## Networks and connectomics

 * [adjmat2pajek](https://pipedocs.github.io/utils/adjmat2pajek.html)
 * [com2map](https://pipedocs.github.io/utils/com2map.html)
 * [com2mat](https://pipedocs.github.io/utils/com2mat.html)
 * [glconsensus](https://pipedocs.github.io/utils/glconsensus.html)
 * [lag](https://pipedocs.github.io/utils/lag.html)
 * [missingIdx](https://pipedocs.github.io/utils/missingIdx.html)
 * [mtd](https://pipedocs.github.io/utils/mtd.html)
 * [quality](https://pipedocs.github.io/utils/quality.html)
 * [roi2ts](https://pipedocs.github.io/utils/roi2ts.html)
 * [ts2adjmat](https://pipedocs.github.io/utils/ts2adjmat.html)
 * [withinBetween](https://pipedocs.github.io/utils/withinBetween.html)

## Spatial coordinates

 * [cmass](https://pipedocs.github.io/utils/cmass.html)
 * [coor2map](https://pipedocs.github.io/utils/coor2map.html)
 * [distmat](https://pipedocs.github.io/utils/distmat.html)
 * [mni2vox](https://pipedocs.github.io/utils/mni2vox.html)
 * [pointTransform](https://pipedocs.github.io/utils/pointTransform.html)
 * [sclib2csv](https://pipedocs.github.io/utils/sclib2csv.html)
 * [vox2mni](https://pipedocs.github.io/utils/vox2mni.html)

## Statistics

 * [featureCorrelation](https://pipedocs.github.io/utils/featureCorrelation.html)

## Template construction

 * [renormalisePriorsPreserveCSF](https://pipedocs.github.io/utils/renormalisePriorsPreserveCSF.html)
 * [standardSpace](https://pipedocs.github.io/utils/standardSpace.html)
 * [templateConstruct](https://pipedocs.github.io/utils/templateConstruct.html)

## Time series processing

 * [1dDespike](https://pipedocs.github.io/utils/1dDespike.html)
 * [1dDMDT](https://pipedocs.github.io/utils/1dDMDT.html)
 * [1dGenfilter](https://pipedocs.github.io/utils/1dGenfilter.html)
 * [1dDespike](https://pipedocs.github.io/utils/1dDespike.html)
 * [dmdt](https://pipedocs.github.io/utils/dmdt.html)
 * [genfilter](https://pipedocs.github.io/utils/genfilter.html)
 * [tfilter](https://pipedocs.github.io/utils/tfilter.html)
