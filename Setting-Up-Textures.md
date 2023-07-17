# Texturing The Terrain

Terrain3D supports up to 32 textures using albedo, height, normal, and roughness.

## Setting Up Textures

* Texture files need to be channel packed as follows:
     * Albedo texture
          * RGB: Albedo
          * A: Height
     * Normal texture
          * RGB: Normal map (+Y OpenGL)
          * A: Roughness

* All textures must be the same size.

* Supported formats
     * DDS files: BC3 / DXT5, linear (not srgb), Color + alpha, Generate Mipmaps. These are accepted instantly by Godot with no settings. 
     * PNG files: Standard RGBA. In Godot you must go to the Import tab and select: `Mode: VRAM Compressed`, `Normal Map: Disabled`, `Mipmaps Generate: On`, then reimport each file.
     * Other formats like TGA are probably supported as long as you match similar settings above.

* You can pack channels in tools like Photoshop, Krita, or [Gimp](https://www.gimp.org/). Then save as a PNG, or DDS (recommended) using the above settings. Working with alpha channels in photoshop can be a bit challenging so Gimp is recommended. In Gimp you can use the `Colors/Components/Compose` and `Decompose` menu options to turn RGBA channels into separate editable layers, and back into RGBA.

* You can create DDS files by exporting them directly from Gimp, exporting from Photoshop with [Intel's DDS plugin](https://www.intel.com/content/www/us/en/developer/articles/tool/intel-texture-works-plugin.html), or converting RGBA PNGs using [NVidia's Texture Tools](https://developer.nvidia.com/nvidia-texture-tools-exporter)

* Some normal maps are for DirectX systems, with -Y. You can convert these to OpenGL by inverting the green channel in a photo editing app. You can visually tell which type of normal map you have by whether the bumps are sticking out towards you (OpenGL) or pushed in away from you (DirectX).

<img width="800" src="https://doc.babylonjs.com/_next/image?url=%2Fimg%2Fhow_to%2FMaterials%2Fnormal_maps1.jpg&w=1920&q=75"/>

