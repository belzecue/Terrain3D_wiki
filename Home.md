# Terrain3D Online Documentation

All API documentation lives here for now, as there is no way to get documentation into the live help inside of Godot for GDExtensions.

See the wiki Contents on the right.

See the readme for [Requirements](https://github.com/outobugi/Terrain3D#requirements) - [Installation](https://github.com/outobugi/Terrain3D#installation--setup) - [Contributing](https://github.com/outobugi/Terrain3D#contributing)

# Project Status

The project is an Alpha. It is pretty solid now, and mostly just missing advanced features. We are starting to experiment with it in our games. Below are specific details.

### Implemented Features

* Geometric Clipmap Mesh Terrain (See [Terrain System Design](#terrain-system-design) below)
* Written in C++ as a Godot plugin, which works with official engine builds
* Up to 10 levels of detail (LODs)
* Up to 16 x 16, 1024 x 1024 sized, non-contiguous regions. Add/remove regions and pay only for the VRAM you use
* Up to 32 textures
* UV scaling and rotation per texture (will likely change to paintable scale and rotation)
* Sculpting: Raise, Lower, Flatten, Expand (Multiply away from 0), Reduce (Divide towards 0), Smooth ([bug](https://github.com/outobugi/Terrain3D/issues/112))
* Painting: Texture, Color, Wetness (roughness)
* Undo/Redo
* [Import/Export data](https://github.com/outobugi/Terrain3D/wiki/Importing-&-Exporting-Data)


### Interaction With Godot Modules
* **Platforms**- Tested on windows only
* **Running the game in the editor** - Works for our demo
* **Exported games** - Untested and probably doesn't work. There may be issues with building, as our build scripts make no distinction on the build mode. And there might be issues loading textures and images see [Build w/o the editor for release templates](https://github.com/outobugi/Terrain3D/issues/76) and [Initial import errors](https://github.com/outobugi/Terrain3D/issues/20)
* **Physics**- Terrain collision uses a HeightMap shape for each region. The demo player uses a CharacterBody and works fine.
* **Navigation Server** - Haven't tested it, but the terrain does provide full lod0 collision, so baking should work.
* **Occlusion**- Haven't tested it. At the least you can use manual shapes.
* **SDGFI**- Seems to work fine.
* **Lightmap baking** - There is no UV2 to bake lightmaps onto, so that's out.

### Interaction with 3rd Party Tools
* Heightmaps from Unity, WorldMachine, Unreal and other tools, see [Importing data](https://github.com/outobugi/Terrain3D/wiki/Importing-&-Exporting-Data)
* **Hungry Proton's [Scatter](https://github.com/HungryProton/scatter)** - we provide an a script you can use to place objects on the terrain

For all other known issues, see [Issues](https://github.com/outobugi/Terrain3D/issues)

### Possible Future Features

See our [roadmap](https://github.com/users/outobugi/projects/1/views/1) for details.

* Advanced texturing techniques - eg. Paintable uv scale, slope, 2-layer texture blending, 3D projection. We intend to implement all of these and adopt techniques provided by The Witcher 3 team. (See [Terrain System Design](#terrain-system-design) below)
* Vegetation - [It's on the list](https://github.com/outobugi/Terrain3D/issues/43) and non-collision based, paintable meshes (rocks, grass) will likely be added as a particle shader. Alternatives are [Scatter](https://github.com/HungryProton/scatter) once he has his particle shader going, a custom particle shader, or [MultiMeshInstance3D](https://docs.godotengine.org/en/stable/tutorials/3d/using_multi_mesh_instance.html)
* Holes - [We'll see](https://github.com/outobugi/Terrain3D/issues/60)
* Double precision floats - [Possible](https://github.com/outobugi/Terrain3D/issues/30)
* Water - Out of scope. Use [WaterWays](https://github.com/Arnklit/Waterways) for rivers, or [Realistic Water Shader](https://godotengine.org/asset-library/asset/343) for lakes.
* Object placement - [Out of scope](https://github.com/outobugi/Terrain3D/issues/47). Use [Scatter](https://github.com/HungryProton/scatter).


## Terrain System Design

Some terrain systems generate grid of mesh chunks, centered around the camera. As the camera moves forward, old meshes far behind are destroyed and new meshes in front are created.

Like The Witcher 3, this system uses a geometry clipmap, where the mesh components are generated once, and at periodic intervals is recentered on the camera location. Terrain3D updates if the camera moves 50% the width of the size of LOD0 (`clipmap_size`, which defaults to 48 units). On each update, the vertex heights are adjusted by the GPU reading from the terrain heightmap. LODs are built into the clipmap mesh so don't require any additional consideration once put in place and heights are adjusted.

### Reference Material
* [GPU Gems 2: Terrain Rendering Using GPU-Based Geometry Clipmaps](https://developer.nvidia.com/gpugems/gpugems2/part-i-geometric-complexity/chapter-2-terrain-rendering-using-gpu-based-geometry)

* [Geometry clipmaps: simple terrain rendering with level of detail](https://mikejsavage.co.uk/blog/geometry-clipmaps.html)

* GDC 2014: [The Witcher 3 Clipmap Terrain and Texturing](https://archive.org/details/GDC2014Gollent) - [Slides](https://ubm-twvideo01.s3.amazonaws.com/o1/vault/GDC2014/Presentations/Gollent_Marcin_Landscape_Creation_and.pdf)
