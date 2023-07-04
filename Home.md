# Terrain3D Online Documentation

Currently there's no way to get live documentation into Godot for GDExtensitons, so all API documentation will live here for now. 

See the wiki Contents on the right.

See the readme for [Requirements](https://github.com/outobugi/GDExtensionTerrain#requirements) - [Installation](https://github.com/outobugi/GDExtensionTerrain#installation--setup) - [Contributing](https://github.com/outobugi/GDExtensionTerrain#contributing)

# Project Status

### Implemented Features

* Geometric Clipmap Mesh Terrain (See [Terrain System Design](#terrain-system-design) below)
* Written in C++ as a Godot plugin, which works with official engine builds
* Up to 10 levels of detail (LODs)
* Up to 16 x 16, 1024 x 1024 sized, non-contiguous regions. Add/remove regions and pay only for the memory you use.
* Sculpting: Raise, Lower, Flatten, Smooth, Grow (may be removed)
* Painting: Color, roughness, texture
* Undo/Redo
* Import/Export data [See how](https://github.com/outobugi/GDExtensionTerrain/wiki/Importing-&-Exporting-Data)


### Known and Expected Issues
* System Platforms - Tested on windows only
* Physics - Terrain collision uses a HeightMap shape for each region. The demo player uses a CharacterBody and works fine.
* Navigation Server - Haven't tested it, but the terrain does provide full detail lod0 terrain so baking should work
* Occlusion - Haven't tested it. At the least you can use manual shapes

For all other known issues, see [Issues](https://github.com/outobugi/GDExtensionTerrain/issues)

### Possible Future Features
* Advanced texturing techniques - eg. Paintable uv scale, slope, 2-layer texture blending, 3D projection. We intend to implement all of these and adopt techniques provided by The Witcher 3 team. (See [Terrain System Design](#terrain-system-design) below)
* Vegetation - [It's on the list](https://github.com/outobugi/GDExtensionTerrain/issues/43) and non-collision based, paintable meshes (rocks, grass) will likely be added as a particle shader. Alternatives are [Scatter](https://github.com/HungryProton/scatter) once he has his particle shader going, a custom particle shader, or [MultiMeshInstance3D](https://docs.godotengine.org/en/stable/tutorials/3d/using_multi_mesh_instance.html)
* Holes - [We'll see](https://github.com/outobugi/GDExtensionTerrain/issues/60)
* Double precision floats - [Possible](https://github.com/outobugi/GDExtensionTerrain/issues/30)
* Water - Out of scope. Use [WaterWays](https://github.com/Arnklit/Waterways) for rivers, or [Realistic Water Shader](https://godotengine.org/asset-library/asset/343) for lakes.
* Object placement - [Out of scope](https://github.com/outobugi/GDExtensionTerrain/issues/47). Use [Scatter](https://github.com/HungryProton/scatter).


## Terrain System Design

Some terrain systems generate grid of mesh chunks, centered around the camera. As the camera moves forward, old meshes far behind are destroyed and new meshes in front are created.

Like The Witcher 3, this system uses a geometry clipmap, where the mesh components are generated once, and at periodic intervals is recentered on the camera location. Terrain3D updates if the camera moves 50% the width of the size of LOD0 (`clipmap_size`, which defaults to 48 units). On each update, the vertex heights are adjusted by the GPU reading from the terrain heightmap. LODs are built into the clipmap mesh so don't require any additional consideration once put in place and heights are adjusted.

### Reference Material
* [GPU Gems 2: Terrain Rendering Using GPU-Based Geometry Clipmaps](https://developer.nvidia.com/gpugems/gpugems2/part-i-geometric-complexity/chapter-2-terrain-rendering-using-gpu-based-geometry)

* [Geometry clipmaps: simple terrain rendering with level of detail](https://mikejsavage.co.uk/blog/geometry-clipmaps.html)

* GDC 2014: [The Witcher 3 Clipmap Terrain and Texturing](https://archive.org/details/GDC2014Gollent) - [Slides](https://ubm-twvideo01.s3.amazonaws.com/o1/vault/GDC2014/Presentations/Gollent_Marcin_Landscape_Creation_and.pdf)
