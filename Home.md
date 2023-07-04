# Terrain3D Online Documentation

Currently there's no way to get live documentation into Godot for GDExtensitons, so the API documentation will live here for now. Some code is also documented.

See the Table of Contents on the right.


# Project Status

### Implemented Features

* Geometric Clipmap Mesh Terrain (See terrain design below)
* Up to 10 levels of detail (LODs)
* Written in C++ as a Godot plugin, which works with official engine builds
* Up to 16 x 16, 1024 x 1024 sized, non-contiguous regions. Add/remove regions and pay only for the memory you use.
* Sculpting: Raise, Lower, Flatten, Smooth, Grow (may be removed)
* Painting: Color, roughness, texture


### Known and Expected Issues
* Platforms - tested on windows only
* Navigation Server - Haven't tested it, but the terrain does provide full detail lod0 terrain so baking should work
* Occlusion - haven't tested it. At the least you can use manual shapes
* Physics - Terrain collision uses a HeightMap shape for each region. The demo player uses a CharacterBody and works fine.

For all other known issues, see [Issues](https://github.com/outobugi/GDExtensionTerrain/issues)

### Possible Future Features
* Vegetation - [It's on the list](https://github.com/outobugi/GDExtensionTerrain/issues/43) and non-collision based, paintable meshes (rocks, grass) will likely be added as a particle shader. Alternatives are [Scatter](https://github.com/HungryProton/scatter) once he has his particle shader going, a custom particle shader, or [MultiMeshInstance3D](https://docs.godotengine.org/en/stable/tutorials/3d/using_multi_mesh_instance.html)
* Holes - [We'll see](https://github.com/outobugi/GDExtensionTerrain/issues/60)
* Double precision floats - [Possible](https://github.com/outobugi/GDExtensionTerrain/issues/30)
* Water - Out of scope. Use [WaterWays](https://github.com/Arnklit/Waterways) for rivers, or [Realistic Water Shader](https://godotengine.org/asset-library/asset/343) for lakes.
* Object placement - [Out of scope](https://github.com/outobugi/GDExtensionTerrain/issues/47). Use [Scatter](https://github.com/HungryProton/scatter).

## Sculpting Operations:

## Painting Operations:


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
