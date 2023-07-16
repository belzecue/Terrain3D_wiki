# Terrain3D Online Documentation

All API documentation lives in this wiki for now, as there currently is no way to get documentation into the live help inside of Godot for GDExtensions.

See the readme for [Requirements](https://github.com/outobugi/Terrain3D#requirements) and [Installation](https://github.com/outobugi/Terrain3D#installation--setup).

The main branch is currently compatible with 4.1. We have [tagged 4.0.3](https://github.com/outobugi/Terrain3D/releases/tag/v0.8-alpha_gd4.0.3), but are not maintaining a branch. As we continue to expand the code, if you wish to try and build it against 4.0, you can try by undoing [these changes](https://github.com/outobugi/Terrain3D/commit/da455147d18674d02ba4b88bd575b58de472c617).

# Project Status

The project is in Alpha. It is pretty solid now, and mostly just missing advanced features. We are starting to experiment with it in our games. Below are specific details.

We need help developing the plugin. See [Contributing](Contributing) if you would like to participate.

For all other known issues not on this list, see [Issues](https://github.com/outobugi/Terrain3D/issues).

### Implemented Features

* Geometric Clipmap Mesh Terrain (See [System Design](System-Design))
* Written in C++ as a Godot plugin. Works with official engine builds
* Up to 10 levels of detail (LODs)
* Up to 16 x 16, 1024 x 1024 sized, non-contiguous regions. Add/remove regions and pay only for the VRAM you use
* Up to 32 textures using albedo, normal, roughness, and height
* UV scaling and rotation per texture (will likely change to paintable scale and rotation)
* Sculpting: Raise, Lower, Flatten, Expand (Multiply away from 0), Reduce (Divide towards 0), Smooth (beware of a [bug](https://github.com/outobugi/Terrain3D/issues/112))
* Painting: Texture, Color, Wetness (roughness) with Height blending
* Undo/Redo
* [Import/Export data](Importing-&-Exporting-Data)


### Interaction With Godot Modules
* **Platforms** - Tested on windows only
* **Running the game in the editor** - Works for our demo
* **Exported games** - Untested and probably doesn't work. There may be issues with building, as our build scripts make no distinction on the build mode. And there might be issues loading textures and images see [Build w/o the editor for release templates](https://github.com/outobugi/Terrain3D/issues/76) and [Initial import errors](https://github.com/outobugi/Terrain3D/issues/20)
* **Physics** - Terrain collision uses a HeightMap shape for each region. The demo player uses a CharacterBody and works fine.
* **Navigation Server** - Haven't tested it, but the terrain does provide full lod0 collision, so baking should work.
* **Occlusion** - Haven't tested it. At the least you can use manual shapes.
* **SDGFI** - Seems to work fine.
* **Lightmap baking** - There is no a static mesh, nor UV2 channel to bake lightmaps onto. So no lightmap baking.

### Interaction With 3rd Party Tools
* **Heightmaps** from Unity, WorldMachine, Unreal and other tools, see [Importing data](Importing-&-Exporting-Data)
* **Hungry Proton's [Scatter](https://github.com/HungryProton/scatter)** - we provide [a script](https://github.com/outobugi/Terrain3D/blob/main/project/addons/terrain_3d/extras/project_on_terrain3d.gd) you can use to place objects on the terrain

### Possible Future Features

See our [roadmap](https://github.com/users/outobugi/projects/1/views/1) for details and priority, and [Contributing](Contributing) if you'd like to help out.

* **Double precision floats** - [Probable](https://github.com/outobugi/Terrain3D/issues/30)
* **Advanced texturing techniques** - eg. Paintable uv scale / slope / rotation, 2-layer texture blending, 3D projection. We intend to implement all of these and adopt techniques provided by The Witcher 3 team. (See [System Design](System-Design))
* **Foliage** - [It's on the list](https://github.com/outobugi/Terrain3D/issues/43). Non-collision based, paintable meshes (rocks, grass) will likely be added as a particle shader. Alternatives are [Scatter](https://github.com/HungryProton/scatter) once he has his particle shader working, your own particle shader, or [MultiMeshInstance3D](https://docs.godotengine.org/en/stable/tutorials/3d/using_multi_mesh_instance.html)
* **Object placement** - [Out of scope](https://github.com/outobugi/Terrain3D/issues/47). Use [Scatter](https://github.com/HungryProton/scatter).
* **Holes** - [We'll see](https://github.com/outobugi/Terrain3D/issues/60)
* **Non-destructive layers** - Used for things like river beds, roads or paths that follow a curve and tweak the terrain. It's possible, but very low priority. 
* **Water** - Out of scope. Use [WaterWays](https://github.com/Arnklit/Waterways) for rivers, or [Realistic Water Shader](https://godotengine.org/asset-library/asset/343) for lakes.

