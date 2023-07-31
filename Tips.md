# Painting

* Right-click any surface to bring it into edit mode.
* Middle-click any surface to clear/delete it. You can only delete the last surface in the list.


# Shaders

* The terrain shader is set to `cull_back`, meaning back faces are not rendered. Nor do they block light. If you have a day/night cycle and the sun sets below the horizon, it will shine through the terrain. Enable the custom shader and change the second line to `cull_disabled` and the horizon will block sunlight. This does cost performance.