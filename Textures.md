# Texturing The Terrain

Terrain3D supports up to 32 textures using albedo, height, normal, and roughness.

**Note:** Currently only DDS files are supported. We are working to get PNG files supported ASAP. It was overlooked as DDS files are better, though a little more challenging to set up. [Gimp](https://www.gimp.org/) is the best tool for packing the channels as necessary and exporting as DDS BC3/DXT5. There are more instructions below.


## Setting Up Textures

At the moment the only accepted texture files are DDS, channel packed as follows:
* Albedo texture
     * RGB: Albedo
     * A: Height
* Normal texture
     * RGB: Normal map (+Y OpenGL)
     * A: Roughness

Notes:
* Some normal maps are for DirectX systems, with -Y. You can convert these to OpenGL by inverting the green channel in a photo editing app. You can visually tell which type of normal map you have by whether the bumps are sticking out towards you (OpenGL) or pushed in away from you (DirectX).

<img width="800" src="https://doc.babylonjs.com/_next/image?url=%2Fimg%2Fhow_to%2FMaterials%2Fnormal_maps1.jpg&w=1920&q=75"/>

* You can pack channels in tools like photoshop, krita, or gimp. Then save as a PNG, or DDS. Working with alpha channels in photoshop can be a bit challenging so Gimp is recommended. In Gimp you can use the `Colors/Components/Compose` and `Decompose` menu options to turn RGBA channels into separate editable layers, and back into RGBA.

* You can create DDS files by exporting them directly from Gimp, exporting from Photoshop with [Intel's DDS plugin](https://www.intel.com/content/www/us/en/developer/articles/tool/intel-texture-works-plugin.html), or converting RGBA PNGs using [NVidia's Texture Tools](https://developer.nvidia.com/nvidia-texture-tools-exporter)

* Export DDS as BC3 / DXT5, linear (not srgb), Color + alpha, Generate Mipmaps

