**Note:** If you are building from source, that page has it's own [troubleshooting section](Building-From-Source#troubleshooting).

## Debug Logs

Terrain3D has debugging logs for everything. Make sure you're running Godot with the console open (you ran `Godot_v4.*_console.exe`). In the Terrain3D node, set debug_level to `debug`. 

To get help on anything you can't solve yourself, attach a full logs from your console or file system. As long as Godot doesn't crash, it saves the log files on your drive. In Godot select, `Editor, Open Editor Data/Settings Menu`. On windows this opens `%appdata%\Godot` eg(`C:\Users\%username%\AppData\Roaming\Godot`). Look under `app_userdata\<your_project_name>\logs`. Otherwise you can copy and paste from the console window.

## Debug Collision

Collision is generated at runtime using the physics server. Normally these collision shapes are not visible. 

To see collision in the editor, enable `Terrain3D.debug_show_collision`, or in the inspector, Debug `show_collision`. It won't regenerate the collision when you edit, but you can disable and reenable this flag to regenerate.

To see collision in game, enable `Terrain3D.debug_show_collision`, and in the editor menu, enable `Debug/Visible Collision Shapes`.

## Common Issues

### Importing Textures

#### `WARNING: Terrain3D::_texture_is_valid: Invalid format. Expected DXT5 RGBA8.`

Read [Setting Up Textures](Setting-Up-Textures) to learn how to properly channel pack your textures.

#### `Albedo/Normal textures do not have same size!` and the terrain turns white

All textures must be the same dimensions. Read [Setting Up Textures](Setting-Up-Textures) and review your textures for odd sizes. If the first set is 2k, all other textures need to be 2k as well.
