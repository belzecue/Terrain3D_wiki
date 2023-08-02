# General

* Always run Godot with the [console](Troubleshooting#debug-logs) open so you can see errors and messages from the engine. The output panel is slow and inadequate.

# Painting

* Right-click any surface to bring it into edit mode.
* Middle-click any surface to clear/delete it. You can only delete the last surface in the list.


# Shaders

* **Debug Shaders** - The shader override will give you a copy of whatever you currently see on the terrain. If you turn on the various debug views, clear any existing shader override, then enable it and you'll get that debug shader. Debug shaders are typically inserted at the end with all of the normal shader parameters included but overwritten.

* **Day/Night** - The terrain shader is set to `cull_back`, meaning back faces are not rendered. Nor do they block light. If you have a day/night cycle and the sun sets below the horizon, it will shine through the terrain. Enable the shader override and change the second line to `cull_disabled` and the horizon will block sunlight. This does cost performance.

* **Remove the mesh outside of regions** - You can hide the terrain outside of your active regions by modifying the override shader. All of these options work on an NVidia card, you only need one:
```
    vertex() {
        // after the UV2 calculation
        if(get_regionf(UV2).z < 0.) {
            VERTEX.x=0./0.;
            //VERTEX=vec3(0./0.);
            //VERTEX=vec3(sqrt(-1));
        }
    }
```