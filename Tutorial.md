# How to Use?

## Intended Usage
- **Only use it on linear images.**
- Use the tool either directly on the stacked result, after background extraction or after color calibration.
- Do not use it on already denoised images.
- Do not use it more than once.
- **Deviating from this is possible and may even yield visually pleasing results, but this will actively compromise the integrity of your data.**

## Parameters
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
