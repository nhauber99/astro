# Hauber-Astro
ML based astrophotography processing tools.

For now a placeholder and container for beta releases. Code (including training) will follow.

## Instructions

### Installation
*For now only Windows is supported. GPU acceleration is used by default if available.*

Add https://photometric.io/pixinsight as PixInsight Respository
1. Resources -> Updates -> Manage Repositories -> Add
2. Resources -> Updates -> Check for Updates
3. Restart PixInsight

### Usage
**Main Parameters:**
- **Target Star FWHM:** How large should the resulting stars be?
- **Denoise Strength:** How much noise should be removed?

**Output Parameters:**
- **Star Detection:** Choose one of the options to see what the star detection model produces. Has no real value apart from troubleshooting issues. The second combobox specifies whether the values should be kept the same (producing values larger than 1), clamped to [0,1] or be normalized.
- **PSF Estimation:** Outputs the spatially varying estimated PSF. Could be used by other external tools in the future.
- **Deconvolution:** This is the main output, by default it replaces the original image or you can choose to create a new one.

**Advanced Parameters:**
- **Overlap:** Keep default.
- **Blend:** Keep default.
- **PSF Samples:** The default will work well, decrease for crops of a larger image, increase if the PSF is varying a lot over the whole image.
- **Dim Star Rejection:** Sets the threshold for rejecting dim stars the network often hallucinates in the current version. This will be done differently in future versions and is a temporary measure.


## Known Issues
- In some patches there may be many false positives for dim stars (especially around star clusters)
  - *Planned fix: rework the entire star and PSF pipeline*
- Brightness estimation for stars may be a bit off sometimes
  - *Planned fix: same as above*
- Some occasional remaining star halo when the target FWHM is small
  - *Planned fix: same as above*
- Artifacts occasionally appearing for large clipped stars
  - *Planned fix: same as above*
- "Egg shaped" PSFs are not correctly handled (not deconvolved to symmetric PSFs) / very long PSFs cause stars to be detected as double stars.
  - *Planned fix: same as above as well as adapting the PSF generation pipeline to be able to generate these*
- Blurry diffraction spikes are not deconvolved to a clean line
  - *Planned fix: detect and handle diffraction spikes differently by passing guiding lines in a separate model input*
- Weird effects when applied on image borders caused by stacking (like bright patches)
  - *Planned fix: add borders to the training data*
- Target PSF has a too tight falloff
  - *Planned fix: make the profile selectable; so either smooth Airy disk approximation (current) or moffat*
- Drizzle integration noise is not cleanly removed
  - *Planned fix: increase drizzle noise kernel augmentation*
- Inference is slow after using other ML based tools which use Tensorflow. This is due to Tensorflow reserving close to the entire GPU memory by default and will persist until PixInsight is restarted.
  - *Planned fix: nothing I can do about it, needs to be fixed in the other tools. Adding TF_FORCE_GPU_ALLOW_GROWTH with value true to the system environment variables improves the issue for the RC Astro tools*
 
If you encounter any new issues, feel free to let me know. If you're willing to send me the source image, it would greatly help me to troubleshoot the issue. Any images you send me would only be used in the test set and not be used in training (it makes sense to keep them out of the training set either way).
 
### Current Limitations
- Iterations are fixed to 1
- Only works on mono images

## Documentation

### Optimization Target
Only pixelwise losses such as MSE or L1 are used throughout the pipeline, along with minor modifications like applying them on multiple scales.
No GANs or perceptual losses are used, nor will they ever be. The network also does not attempt to estimate a maximum a posteriori instead of the conditional mean / median.

### Training Data
The source dataset consists of a mix of processed images downloaded from different sources (all sources allowed for this with either their terms of service or express written permission from the site owner). These are further processed by removing the stars (via a custom model trained from scratch), then attempting to roughly reverse the nonlinearity followed by aggressive downsampling to get as clean a dataset as possible. Remaining elements, such as stars, blur, and noise, are purely synthetic.
