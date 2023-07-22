# Terrain3D Online Documentation

All documentation lives in this wiki for now, as there currently is no way to get documentation into the live help inside of Godot for GDExtensions.

See the readme for [Requirements](https://github.com/outobugi/Terrain3D#requirements) and [Installation](https://github.com/outobugi/Terrain3D#installation--setup).

# Compatible Engine Versions

The main branch is currently compatible with 4.1. The binary releases will likely work on 4.1.1 and future minor versions, and building shouldn't be an issue as long as you update godot-cpp.

We have tagged [4.0.3](https://github.com/outobugi/Terrain3D/releases/tag/v0.8-alpha_gd4.0.3), but are not maintaining a branch. As we continue to develop the code, if you wish to try and build the current master branch against 4.0, undo [these changes](https://github.com/outobugi/Terrain3D/commit/da455147d18674d02ba4b88bd575b58de472c617).


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
* Sculpting: Raise, Lower, Flatten, Expand (Multiply away from 0), Reduce (Divide towards 0), Smooth
* Painting: Texture, Color, Wetness (roughness) with Height blending
* Undo/Redo
* [Import/Export data](Importing-&-Exporting-Data)


### Interaction With Godot Modules
* **Platforms** - Works on Windows, Linux, OSX. Even one successful report on Android (Samsung galaxy tab, but there's a [shader issue](https://github.com/outobugi/Terrain3D/issues/137))
* **Running the game from the editor** - Works
* **Exported games** - Works
* **Physics** - Works within regions you define in your world. No collision outside of those.
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
* **Holes** - Visual is likely. Collision is less likely. [See #60](https://github.com/outobugi/Terrain3D/issues/60)
* **Non-destructive layers** - Used for things like river beds, roads or paths that follow a curve and tweak the terrain. It's possible, but very low priority. 
* **Object placement** - For objects with collision. [Out of scope](https://github.com/outobugi/Terrain3D/issues/47). Use [Scatter](https://github.com/HungryProton/scatter).
* **Water** - Out of scope. Use [WaterWays](https://github.com/Arnklit/Waterways) for rivers, or [Realistic Water Shader](https://godotengine.org/asset-library/asset/343) for lakes.

