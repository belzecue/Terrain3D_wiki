## Internal Data Storage

A `Terrain3DStorage` object stores all terrain data in the following containers:

### Height Maps
* `TypedArray<Image> height_maps;`
* Godot type `Array[Image]`
* Image Format: `FORMAT_RF`, 32-bit per pixel as full-precision floating-point

Defines the height value of the terrain at a given pixel. This is sent to the vertex shader on the GPU which modifies the mesh in real-time.

Editing is always done in 32-bit. We do provide an option to save as 16-bit, which converts to 32-bit on load and back to 16-bit on save.

### Control Maps
* `TypedArray<Image> control_maps;`
* Godot type: `Array[Image]`
* Image Format: `FORMAT_RGB8`, 24-bits per pixel as three 8-bit components

Defines which texture index is used at a given pixel. 

In alpha this is specified as:
* r = base texture index
* g = overlay texture index
* b = blend value between the two

We have limited the total number of textures to 32 so we can reserve bits on this map. 

In beta, this map it will look something like this:
* 5 bits - base texture
* 5 bits - overaly texture
* 3 bits - blend index 
* 3 bits - uv scale index
* 3 bits - slope index
* 3 bits - rotation index
* 1 bit - hole
* and more reserved for other paintable features or more textures

The indices above will select from an 8-index array of values such as `{ 0.0f, .125f, .25f, .334f, .5f, .667f, .8f, 1.0f };`

### Color / Roughness Maps
* `TypedArray<Image> color_maps;`
* Godot type: `Array[Image]`
* Image Format: `FORMAT_RGBA8`, 32-bits per pixel as four 8-bit components

RGB is used for color, which is `multiplied` by albedo in the shader. Multiply is a blend mode that only darkens.

A is used for a roughness modifier. A value of 0.5 means no change to the existing texture roughness. Higher than this value increases roughness, lower decreases it.

### Region Offsets

* `TypedArray<Vector2i> region_offsets;`
* Godot type: `Array[Vector2i]`

This array stores the locations of the active regions. Values are region_map offsets, not global_position locations. This array looks like `{ (0, 0), (-1, 1) }`.


### Surfaces
* TypedArray<Terrain3DSurface> surfaces;
* Godot type: `Array[Terrain3DSurface]`

This is an array of the texture sets: albedo/height, normal/roughness, albedo shade, uv scale, rotation.

------

### Generated Maps

The above arrays house all of the data for the system. These are saved onto disk and reloaded when the scene loads. Upon loading, and upon most terrain edits, these arrays are processed into a series of generated arrays, which are sent to the shader.

For instance, all of the separate terrain surfaces (texture sets) are combined into a `Texture2DArray` which is passed to the shader via a uniform. All of the region heightmaps are similarly combined. 

