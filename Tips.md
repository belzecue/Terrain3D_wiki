# Painting

* Right-click any surface to bring it into edit mode.
* Middle-click any surface to clear/delete it. You can only delete the last surface in the list.


# Shaders

* The shader override will give you a copy of whatever you currently see on the terrain. If you turn on the various debug views, clear any existing shader override, then enable it and you'll get that debug shader. Debug shaders are typically inserted at the end with all of the normal shader parameters included but overwritten.

* The terrain shader is set to `cull_back`, meaning back faces are not rendered. Nor do they block light. If you have a day/night cycle and the sun sets below the horizon, it will shine through the terrain. Enable the shader override and change the second line to `cull_disabled` and the horizon will block sunlight. This does cost performance.