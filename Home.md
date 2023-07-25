# Terrain3D Online Documentation

All documentation lives in this wiki for now, as there currently is no way to get documentation into the live help inside of Godot for GDExtensions.

See the readme for [Requirements](https://github.com/outobugi/Terrain3D#requirements) and [Installation](https://github.com/outobugi/Terrain3D#installation--setup).

# Compatible Engine Versions

In general you want to exact match the plugin version to the engine version. It may work with minor variations (eg 4.1 and 4.1.1), but that entirely depends on Godot's development at any given moment. You'll know it won't work if it crashes on startup. We release binaries for the current engine, but if you need to match a specific version, you'll need to [build the plugin yourself](Building-From-Source).

The main branch is currently compatible with 4.1.

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

| Feature | Status | 
| ------------- | ------------- | 
| **Platforms** | Works on Windows, Linux, OSX. Android [pending](https://github.com/outobugi/Terrain3D/issues/137)
| Running through the editor | Works
| Exported games | Works
| **Data** |
| Large terrains | 8k x 8k works, maybe a little more. 16k x 8k and higher works in memory, but cannot be saved due to an [engine bug](https://github.com/outobugi/Terrain3D/issues/159).
| Importing / Exporting | Works. See [Importing data](Importing-&-Exporting-Data)
| Double precision floats | [Probable](https://github.com/outobugi/Terrain3D/issues/30)
| **Rendering** |
| Frustum Culling | The terrain is made up of several meshes, so half can be culled if the camera is on the ground.
| Occlusion Culling | Untested. At the least you can use manual shapes.
| SDGFI | Seems to work fine.
| Lightmap baking | Not possible. There is no a static mesh, nor UV2 channel to bake lightmaps onto.
| **Physics** |
| Godot | Works within regions you define in your world. No collision outside of those.
| Jolt | Pending support from Jolt. See [#152](https://github.com/outobugi/Terrain3D/discussions/152).
| **Navigation Server** | Untested. Heard one report that it crashed. The terrain does provide full lod0 collision, so baking should work maybe after a bug fix.
| **Environment** |
| Foliage | [Pending](https://github.com/outobugi/Terrain3D/issues/43). Non-collision based, paintable meshes (rocks, grass) will likely be added as a particle shader. Alternatives are [Scatter](https://github.com/HungryProton/scatter) once he has his particle shader working, your own particle shader, or [MultiMeshInstance3D](https://docs.godotengine.org/en/stable/tutorials/3d/using_multi_mesh_instance.html)
| Object placement | [Out of scope](https://github.com/outobugi/Terrain3D/issues/47). For placing objects with collision, use [Scatter](https://github.com/HungryProton/scatter). We provide [a script](https://github.com/outobugi/Terrain3D/blob/main/project/addons/terrain_3d/extras/project_on_terrain3d.gd) that allows Scatter to detect our terrain.
| Holes | Visual holes [pending](https://github.com/outobugi/Terrain3D/issues/60). Collision holes unlikely, but we'll provide instructions for allowing a player to pass through the collision.
| Advanced texturing techniques | Pending. eg. Paintable uv scale / slope / rotation, 2-layer texture blending, 3D projection. We intend to implement all of these and adopt techniques provided by The Witcher 3 team. (See [System Design](System-Design))
| Non-destructive layers | Used for things like river beds, roads or paths that follow a curve and tweak the terrain. It's [possible](https://github.com/outobugi/Terrain3D/issues/129), but low priority.
| Water | Out of scope. Use [WaterWays](https://github.com/Arnklit/Waterways) for rivers, or [Realistic Water Shader](https://godotengine.org/asset-library/asset/343) for lakes.


See our [roadmap](https://github.com/users/outobugi/projects/1/views/1) for more details and priority, and [Contributing](Contributing) if you'd like to help out.
