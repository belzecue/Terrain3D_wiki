## Internal Data Storage

A `Terrain3DStorage` object stores all data in the following containers:

### Height Maps

* Type: GD `Array[Image]` / C++ `TypedArray<Image>` 
* Image Format: `FORMAT_RH`, 16-bit per pixel as a half-precision floating-point

Defines the height value of the terrain at a given pixel. This is sent to the vertex shader on the GPU which modifies the mesh in realtime.


### Control Maps

* Type: GD `Array[Image]` / C++ `TypedArray<Image>` 
* Image Format: `FORMAT_RGBA8`, 32-bits per pixel as four 8-bit components

Defines which texture index is used at a given pixel. 

*Eventually* this map will be used for paintable UV scaling, slope, and other things.


### Color / Roughness Maps

* Type: GD `Array[Image]` / C++ `TypedArray<Image>` 
* Image Format: `FORMAT_RGBA8`, 32-bits per pixel as four 8-bit components

RGB is used for color, which is `multiplied` by albedo in the shader. Multiply is a blend mode that only darkens.

A is used for roughness. Reduce roughness to make the texture more glossy.


### Region Offsets

* Type: GD `Array[Vector2i]` / C++ `TypedArray[Vector2i>` 

This array stores the locations of the active regions. Values are region_map offsets, not global_position locations, and look like `(0, 0), (-1, 1)`.


### Surfaces

* Type: GD `Array[Terrain3DSurface]` / C++ `TypedArray[Terrain3DSurface>` 

This is an array of the texture sets: albedo/height, normal/roughness.


### Generated Maps

The above arrays house all of the data for the system. These are saved onto disk and reloaded when the scene loads. Upon loading and every time various components are rebuilt, these arrays are processed into a series of generated arrays.

For instance, all of the separate terrain surfaces (texture sets) are combined into a `Texture2DArray` to be sent to the GPU. All of the region heightmaps are similarly combined. 


## Importing Data

We can import any supported image format Godot can read into any of the map types. These types include:
* [Image formats for external files](https://docs.godotengine.org/en/4.0/tutorials/assets_pipeline/importing_images.html#supported-image-formats): `bmp`, `dds`, `exr`, `hdr`, `jpg`, `jpeg`, `png`, `tga`, `svg`, `webp`.
* [Image formats stored in a Godot resource file](https://docs.godotengine.org/en/4.0/classes/class_image.html#enum-image-format): `.tres`, `.res`.
* R16 Height map aka RAW: For 16-bit heightmaps read/writable by World Machine, Unity, Krita, Photoshop, etc. Rename the extension to `.r16`. Min/Max heights and image size are not stored in the file, so you must store them externally (or in the name).

Only EXR or R16 are recommended for heightmaps. PNG is sufficient for external editing of control and color maps.

Upon import, you can specify the location of import on the greater 16k^2 world. e.g. You could import multiple maps as separate or combined islands.

It will slice or pad odd sized images into region sized (default is 1024x1024) chunks. e.g. You could import a 4k x 2k EXR into the height map, several 1k x 1k PNGs into the color map, and a 5123 x 3769 jpg into the control map and position them so they overlap.


## Exporting Data

We can export any map as `r16`, `exr`, `png`, `jpg`, `webp`, or native godot resource formats `tres` for text and `res` for binary (with ResourceSaver::FLAG_COMPRESS enabled).

The general exporter takes the smallest rectangle that will fit around all active regions in the 16k^2 world and exports that as an image. So if you have a 1k x 1k island in the NW corner, and a 2k x 3k island in the center, with a 1k strait between them, the resulting export image will be something like 4k x 5k.

We currently do not offer region by region export, but there is an API where you can retrieve any given region, then you can use Image to save it externally yourself.
