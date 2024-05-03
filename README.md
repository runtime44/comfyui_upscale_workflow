# Runtime44 Community Upscale Workflow

<p align="center">
    <img alt="Upscale Workflow" src="https://raw.githubusercontent.com/runtime44/comfyui_upscale_workflow/canary/assets/banner.webp"/>
</p>

<p align="center">
    <a href="https://runtime44.com">Runtime44</a> - <a href="https://github.com/runtime44/comfyui_upscale_workflow/blob/canary/CHANGELOG.md">Changelog</a> - <a href="https://github.com/runtime44/comfyui_upscale_workflow/issues">Bug reports</a>
</p>

> [!NOTE]
> This workflow is partially adapted to ComfyUI, therefore the results might be different from those that you can expect on [Runtime44 - Mage](https://runtime44.com/mage)

## Dependencies
- [Runtime44 ComfyUI Nodes](https://github.com/runtime44/comfyui_r44_nodes) `git clone https://github.com/runtime44/comfyui_r44_nodes.git`
- [ComfyUI Impact Pack](https://github.com/ltdrdata/ComfyUI-Impact-Pack) `git clone https://github.com/ltdrdata/ComfyUI-Impact-Pack.git`
- [rgthree-comfy](https://github.com/rgthree/rgthree-comfy) `git clone https://github.com/rgthree/rgthree-comfy.git`
- [Comfyroll Studio](https://github.com/Suzie1/ComfyUI_Comfyroll_CustomNodes) `git clone https://github.com/Suzie1/ComfyUI_Comfyroll_CustomNodes.git`
- [ComfyUI Custom Scripts](https://github.com/pythongosssss/ComfyUI-Custom-Scripts) `git clone https://github.com/pythongosssss/ComfyUI-Custom-Scripts.git`
- [ComfyUI ControlNet Auxiliary Preprocessors](https://github.com/Fannovel16/comfyui_controlnet_aux) `git clone https://github.com/Fannovel16/comfyui_controlnet_aux.git`

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

## License
This workflow is distributed under the [GNU AGPLv3](./LICENSE.md) license

## Credits
- [ComfyUI](https://github.com/comfyanonymous/ComfyUI)
