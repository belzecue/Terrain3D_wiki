This page is for tool & plugin developers and more advanced gamedevs who want to know how best to integrate their other systems with Terrain3D.


## Detecting Terrain3D

* If collision is enabled in game (default) or in the editor (debug only), you can run a raycast and if it hits, it will return a `Terrain3D` object. See more below in the raycast section.

* To determine if Terrain3D is installed and active, [ask Godot](https://docs.godotengine.org/en/stable/classes/class_editorinterface.html#class-editorinterface-method-is-plugin-enabled). (Note this works only after [this commit](https://github.com/outobugi/Terrain3D/commit/af2c8eb3b56b5485e32a6fcbcd7a2d8b4d0f96c1))

```
     var ei: EditorInterface = EditorScript.new().get_editor_interface()
     print("Terrain3D installed: ", ei.is_plugin_enabled("terrain_3d"))
```

* Your script can provide a NodePath and allow the user to select their Terrain3D node as was done in [the script](https://github.com/outobugi/Terrain3D/blob/df901b4fd51a81175e4f5177c33318a8a4b19c36/project/addons/terrain_3d/extras/project_on_terrain3d.gd#L13) provided for use with Scatter.

* You can search the current scene tree for [nodes of type](https://docs.godotengine.org/en/stable/classes/class_node.html#class-node-method-find-children) "Terrain3D".
```
     var terrain
     if Engine.is_editor_hint(): 
          # In editor
          terrain = get_tree().get_edited_scene_root().find_children("*", "Terrain3D")
     else:
          # In game
          terrain = get_tree().get_current_scene().find_children("*", "Terrain3D")

```


## Detecting Terrain Height

There are two ways to determine the height in Terrain3D.

### 1) Raycasting

Normally, collision is not generated in the editor. It can be if `Terrain3D.debug_show_collision` is enabled. Then the terrain will have a StaticBody with collision and you can do a normal raycast. This also works fine while running in a game.

This debug option will generate collision one time until it is disabled and reenabled. If the terrain is sculpted before then, the generated collision will be inaccurate. On a Core-i9 12900H, generating collision takes about 145ms per region, so updating it several times per second while sculpting is not practical. Currently all regions are regenerated, rather than only modified regions so it is not optimal.

This will generate collision in the editor. There is no collision outside of regions. See below.


### 2) Querying

The much more optimal way to detect height is to just ask the Terrain3DStorage for the height directly:

```
     var height: float = terrain_node.storage.get_height(global_location)
```

This is ideal for one lookup. However, if you wish to look up thousands of heights, it will be significantly faster if you retrieve the heightmap Image for the region and query it directly.

```
     var region_index: int = terrain.storage.get_region_index(global_location)
     var img: Image = terrain.storage.get_map_region(Terrain3DStorage.TYPE_HEIGHT, region_index)
     for y in img.get_height():
          fox x in img.get_width():
               Color pixel = img.get_pixel(x, y)
```


### Regions

Terrain3D provides users non-contiguous 1024x1024 sized regions on a 16x16 region grid. So a user might have a 1k x 2k island in one corner of the world and a 4k x 4k island elsewhere. In between are empty regions, visually flat space where they could place an ocean. No collision is generated there. 

If you run a raycast outside of the regions, it won't hit anything.

If you query `get_height()` outside of regions, it will return 0.

You can determine if a given location is within a region by using this function. It will return -1 if the XZ location is not within a region. Y is ignored.

```
var region_index: int = Terrain3DStorage.get_region_index(global_location)
```
