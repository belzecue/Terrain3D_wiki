# Terrain3D Online Documentation

Currently there's no way to get live documentation into Godot for GDExtensitons, so documentation will live here for now. Some code is also documented.

See the Table of Contents on the right.

# Implemented Features

## Geometric Clipmap Mesh Terrain 
(See terrain design below)

* Up to 10 levels of detail (LODs)
* Written in C++ as a Godot plugin, which works with official engine builds


## Sculpting Operations:

## Painting Operations:

## Internal Formats
Defined in [Godot Image Formats](https://docs.godotengine.org/en/4.0/classes/class_image.html#enum-image-format)
* Height maps: `FORMAT_RH`, 16-bit per pixel as a half-precision floating-point
* Control maps: `FORMAT_RGBA8`, 32-bits per pixel as four 8-bit components
* Color maps: `FORMAT_RGBA8`

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

## Terrain System Design

Some terrain systems generate mesh chunk on a grid centered around the camera, destroying old meshes as the camera moves and adding new ones.

Like The Witcher 3, this system uses a geometry clipmap, where the mesh is generated once, and at periodic intervals is recentered on the camera location. On every interval, the vertex heights are adjusted by the GPU, corresponding to terrain data. The data is stored in a heightmap and is wrapped so the terrain is repeated infinitely. LODs are built into the clipmap mesh so doesn't require much additional consideration.

Reference Material
GPU Gems 2: Terrain Rendering Using GPU-Based Geometry Clipmaps https://developer.nvidia.com/gpugems/gpugems2/part-i-geometric-complexity/chapter-2-terrain-rendering-using-gpu-based-geometry

Geometry clipmaps: simple terrain rendering with level of detail

https://mikejsavage.co.uk/blog/geometry-clipmaps.html

The Witcher 3 Clipmap Terrain and Texturing

https://archive.org/details/GDC2014Gollent
https://ubm-twvideo01.s3.amazonaws.com/o1/vault/GDC2014/Presentations/Gollent_Marcin_Landscape_Creation_and.pdf
