# Hauber-Astro
ML based astrophotography processing tools.
For now a placeholder and container for beta releases. Code (including training) will follow.

## Known Issues
- In some patches there may be many false posisives for dim stars (especially around star clusters)
  - *Planned fix: rework the entire star and PSF pipeline*
- Brightness estimation for stars may be a bit off sometimes
  - *Planned fix: rework the entire star and PSF pipeline*
- Blurry diffraction spikes are not deconvolved to a clean line
  - *Planned fix: detect and handle diffraction spikes differently by passing guiding lines in a separate model input*
- Target PSF has a too tight falloff
  - *Planned fix: make the profile selectable; so either smoth Airy disk approximation (current) or moffat*
 
## Current Limitations
- Denoising strength is fixed to 1
- Iterations are fixed to 1
