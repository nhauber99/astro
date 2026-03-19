# Hauber-Astro
ML based astrophotography processing tools.
For now a placeholder and container for beta releases. Code (including training) will follow.

## Optimization Target
Only pixelwise losses such as MSE or L1 are used throughout the pipeline, along with minor modifications like applying them on multiple scales.
No GANs or perceptual losses are used, nor will they ever be. The network also does not attempt to estimate a maximum a posteriori instead of the conditional mean / median.

## Known Issues
- In some patches there may be many false positives for dim stars (especially around star clusters)
  - *Planned fix: rework the entire star and PSF pipeline*
- Brightness estimation for stars may be a bit off sometimes
  - *Planned fix: same as above*
- Some occasional remaining star halo when the target FWHM is small
  - *Planned fix: same as above*
- Artifacts occasionally appearing for large clipped stars
  - *Planned fix: same as above*
- Blurry diffraction spikes are not deconvolved to a clean line
  - *Planned fix: detect and handle diffraction spikes differently by passing guiding lines in a separate model input*
- Weird effects when applied on image borders caused by stacking (like bright patches)
  - *Planned fix: add borders to the training data*
- Target PSF has a too tight falloff
  - *Planned fix: make the profile selectable; so either smooth Airy disk approximation (current) or moffat*
- Inference is slow after using other ML based tools which use Tensorflow. This is due to Tensorflow reserving close to the entire GPU memory by default and will persist until PixInsight is restarted.
  - *Planned fix: nothing I can do about it, needs to be fixed in the other tools. Adding TF_FORCE_GPU_ALLOW_GROWTH with value true to the system environment variables improves the issue for the RC Astro tools*
 
## Current Limitations
- Iterations are fixed to 1
- Only works on mono images

## Training Data
The source dataset consists of a mix of processed images downloaded from different sources (all sources allowed for this with either their terms of service or express written permission from the site owner). These are further processed by removing the stars (via a custom model trained from scratch), then attempting to roughly reverse the nonlinearity followed by aggressive downsampling to get as clean a dataset as possible. Remaining elements, such as stars, blur, and noise, are purely synthetic.
