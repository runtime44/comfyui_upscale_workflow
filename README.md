# Runtime44 Community Upscale Workflow

<p align="center">
    <img alt="Upscale Workflow" src="https://raw.githubusercontent.com/runtime44/comfyui_upscale_workflow/canary/assets/banner.webp"/>
</p>

<p align="center">
    <a href="https://runtime44.com">Runtime44</a> - <a href="https://github.com/runtime44/comfyui_upscale_workflow/blob/canary/CHANGELOG.md">Changelog</a> - <a href="https://github.com/runtime44/comfyui_upscale_workflow/issues">Bug reports</a>
</p>

> [!NOTE]
> This workflow is a partial adaptation to ComfyUI, therefore the results might be different from those that you can expect on [Runtime44 - Mage](https://runtime44.com/mage)

## Preamble

Both this workflow, and [Mage](https://runtime44.com/mage), aims to generate the highest quality image, whilst remaining faithful to the original image.
Although the goal is the same, the execution is different, hence why you will most likely have different results between this and [Mage](https://runtime44.com/mage), the latter being optimized to run some processes in parallel on multiple GPUs and a different diffusion pipeline.

## Dependencies
- [Runtime44 ComfyUI Nodes](https://github.com/runtime44/comfyui_r44_nodes) `git clone https://github.com/runtime44/comfyui_r44_nodes.git`
- [ComfyUI Impact Pack](https://github.com/ltdrdata/ComfyUI-Impact-Pack) `git clone https://github.com/ltdrdata/ComfyUI-Impact-Pack.git`
- [rgthree-comfy](https://github.com/rgthree/rgthree-comfy) `git clone https://github.com/rgthree/rgthree-comfy.git`
- [Comfyroll Studio](https://github.com/Suzie1/ComfyUI_Comfyroll_CustomNodes) `git clone https://github.com/Suzie1/ComfyUI_Comfyroll_CustomNodes.git`
- [ComfyUI Custom Scripts](https://github.com/pythongosssss/ComfyUI-Custom-Scripts) `git clone https://github.com/pythongosssss/ComfyUI-Custom-Scripts.git`
- [ComfyUI ControlNet Auxiliary Preprocessors](https://github.com/Fannovel16/comfyui_controlnet_aux) `git clone https://github.com/Fannovel16/comfyui_controlnet_aux.git`
- [ComfyUI KJNodes](https://github.com/kijai/ComfyUI-KJNodes) `git clone https://github.com/kijai/ComfyUI-KJNodes.git`

To install these dependencies, you have two options:
1. Using [ComfyUI Manager](https://github.com/ltdrdata/ComfyUI-Manager) _(recommended)_
2. Manually installing in your `custom_nodes` directory.

If you opt for the manual install, make sure that your virtual env is activated and that you install the `requirements.txt` for each of these packages

## Models
This workflow requires a few models in different categories to work.

### Diffusion
We recommend using a mix between SD1.5 and SDXL for the diffusion, but you are free to use whichever model you like.
For reference, here are the model that were used:
- Realistic Vision 6.0 (SD1.5)
- DreamshaperXL Lightning (SDXL Lightning)

### Image Upscale
Each **upscale model** has a specific scaling factor (2x, 3x, 4x, ...) that it is optimized to work with. If you go above or below that scaling factor, a standard resizing method will be used (in the case of our custom node, **lanczos**). While being convenient, it could also reduce the quality of the image.
Therefore, we recommend finding a 2x, 3x and 4x model.

Thankfully, because of the upscaling chain that is built into the workflow, if you wish to upscale 8x, you do not need to use a 8x model. You can just use a 2x model and the chain will distribute the load between up to 4 nodes.

You can mix upscale models depending on your needs

For reference, here are the upscale models that were used:
- 2x RealESRGAN_plus
- 4x foolhardy_Remacri

You can find more models on [OpenModelDB](https://openmodeldb.info)

### ControlNet
- SD1.5 Tile
- SDXL Tile _(optional)_

## Process
This workflow applies the principles of _double sampling_, where the first sampling is exaggerated to generate an image with a lot of noise and added elements (in our case using SD 1.5), and the second sampling is used as a refiner and fixes all the issues generated at the previous step, while using it as a starting point

### First Sampling
In this workflow, we are making use of masks to separate persons from their environment (it would work similarly with anything that you can segment in an image). This allows for more control over the amount of details that we want to generate on the image.
For that, we use the [Mask Sampler](https://github.com/runtime44/comfyui_r44_nodes) with the two main inputs being the latent image and the mask.

Because this step aims to generate as many details as possible from the upscaled image, we use a heavy ControlNet strength to contain SD hallucinations.
If you feel like there are too much added elements to your image, feel free to increase that value

### Second Sampling
Here, we use two sampling passes:
1. Medium denoise, low ControlNet, adding noise, returning with noise
2. Low denoise, no ControlNet, no noise addition, returning without noise

During these two passes, the sampler also changes.

### Last details
Finally, we apply the last details to our image, this is where you can use the [Image Enhance](https://github.com/runtime44/comfyui_r44_nodes) node, or the [Color Match](https://github.com/runtime44/comfyui_r44_nodes) _(recommended)_ to add the finishing touches to your image

## License
This workflow is distributed under the [GNU AGPLv3](./LICENSE.md) license to generate an image with a lot of noise and added elements

## Credits
- [ComfyUI](https://github.com/comfyanonymous/ComfyUI)
