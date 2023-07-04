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

The above arrays house all of the data for the system. These are saved onto disk and reloaded when the scene loads. Upon loading, and upon most terrain edits, these arrays are processed into a series of generated arrays, which are sent to the shader.

For instance, all of the separate terrain surfaces (texture sets) are combined into a `Texture2DArray` which is passed to the shader via a uniform. All of the region heightmaps are similarly combined. 

