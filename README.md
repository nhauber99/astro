# Hauber-Astro
ML based astrophotography processing tools.
For now a placeholder and container for beta releases. Code (including training) will follow.

## Known Issues
- In some patches there may be many false positives for dim stars (especially around star clusters)
  - *Planned fix: rework the entire star and PSF pipeline*
- Brightness estimation for stars may be a bit off sometimes
  - *Planned fix: rework the entire star and PSF pipeline*
- Blurry diffraction spikes are not deconvolved to a clean line
  - *Planned fix: detect and handle diffraction spikes differently by passing guiding lines in a separate model input*
- Target PSF has a too tight falloff
  - *Planned fix: make the profile selectable; so either smooth Airy disk approximation (current) or moffat*
- Inference is slow after using other ML based tools which use Tensorflow. This is due to Tensorflow reserving close to the entire GPU memory by default and will persist until PixInsight is restarted.
  - *Planned fix: nothing I can do about it, needs to be fixed in the other tools. Adding TF_FORCE_GPU_ALLOW_GROWTH with value true to the system environment variables improves the issue for the RC Astro tools*
 
## Current Limitations
- Denoising strength is fixed to 1
- Iterations are fixed to 1
- Only works on mono images
