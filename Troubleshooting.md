
## Debug Logs

Terrain3D has debugging logs for everything. Make sure you're running Godot with the console open. In the Terrain3D node, set debug_level to `debug`. 

To get help on anything you can't solve yourself, attach a full logs from your console. As long as Godot doesn't crash, it saves the log files on your drive. In Godot select, `Editor, Open Editor Data/Settings Menu`. On windows this opens `%appdata%\Godot` eg(`C:\Users\%username%\AppData\Roaming\Godot`). Look under `app_userdata\<your_project_name>\logs`. Otherwise you can copy and paste from the console window.

## Debug Collision

Collision is generated at runtime using the physics server. Normally these collision shapes are not visible. 

To see collision in the editor, enable `Terrain3D.debug_show_collision`, or in the inspector, Debug `show_collision`. It won't regenerate the collision when you edit, but you can disable and reenable this flag to regenerate.

To see collision in game, enable `Terrain3D.debug_show_collision`, and in the editor menu, enable `Debug/Visible Collision Shapes`.

## Common Issues

#### When I import textures it reports `Albedo/Normal textures do not have same size!` and the terrain turns white

This means some textures you have already set up are different sizes than the new texture you're setting up. Eg. Textures 1-4 are 2k and you are inserting a new texture using 1k or 4k files. Make sure all textures are the same size.

