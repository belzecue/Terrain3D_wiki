# Texturing The Terrain

Terrain3D supports up to 32 textures using albedo, height, normal, and roughness.

## Texture Format

* **All textures must be the same size**.

* Texture files need to be channel packed as follows:
     * Albedo texture
          * RGB: Albedo
          * A: Height
     * Normal texture
          * RGB: Normal map (+Y OpenGL)
          * A: Roughness

* Supported formats
     * DDS files: BC3 / DXT5, linear (not srgb), Color + alpha, Generate Mipmaps. These are accepted instantly by Godot with no settings. **Highly recommended**
     * PNG files: Standard RGBA. In Godot you must go to the Import tab and select: `Mode: VRAM Compressed`, `Normal Map: Disabled`, `Mipmaps Generate: On`, then reimport each file.
     * Other formats like TGA are probably supported as long as you match similar settings above.

* You can pack channels in tools like Photoshop, Krita, or [Gimp](https://www.gimp.org/). Working with alpha channels in Photoshop and Krita can be a bit challenging so Gimp is recommended. See [How to Channel Pack Images with Gimp](#how-to-channel-pack-images-with-gimp) below. Then save as a PNG, or DDS (recommended) using the above settings. 

* You can create DDS files by exporting them directly from Gimp, exporting from Photoshop with [Intel's DDS plugin](https://www.intel.com/content/www/us/en/developer/articles/tool/intel-texture-works-plugin.html), or converting RGBA PNGs using [NVidia's Texture Tools](https://developer.nvidia.com/nvidia-texture-tools-exporter)

* Some normal maps are for DirectX systems, with -Y. You can convert these to OpenGL by inverting the green channel in a photo editing app. You can visually tell which type of normal map you have by whether the bumps are sticking out towards you (OpenGL) or pushed in away from you (DirectX).

<img width="800" src="https://doc.babylonjs.com/_next/image?url=%2Fimg%2Fhow_to%2FMaterials%2Fnormal_maps1.jpg&w=1920&q=75"/>

## How to Channel Pack Images with Gimp

1. Open up your RGB Albedo and greyscale Height files (or Normal and Roughness).

2. On the RGB file select `Colors/Components/Decompose`. Select `RGB`. Keep `Decompose to layers` checked. Now on the resulting image you have three greyscale layers for RGB. This would be a good time to invert the green channel if you need to convert a Normalmap from DirectX to Opengl.

3. Copy the greyscale file and paste it as a new layer into this decomposed file. Name the new layer `alpha`.

4. Select `Colors/Components/Compose`. Select `RGBA` and ensure each named layer connects to the correct channel.

5. Your new image is channel packed properly. Now export the file with the following settings. DDS is highly recommended.

### Exporting As DDS
* Change `Compression` to `BC3 / DXT5`
* `Mipmaps` to `Generate Mipmaps`. 
* Insert into Godot and you're done.

![image](https://github.com/outobugi/Terrain3D/assets/632766/030d87a7-43ed-4fed-bac6-c69bb6fcd11a)

### Exporting As PNG
* Change `automatic pixel format` to `8bpc RGBA`. 
* In Godot you must go to the Import tab and select: `Mode: VRAM Compressed`, `Normal Map: Disabled`, `Mipmaps Generate: On`, then click `Reimport`.

![image](https://github.com/outobugi/Terrain3D/assets/632766/d30fa7c4-177c-491f-bcaa-962f9a1f0c6b)
