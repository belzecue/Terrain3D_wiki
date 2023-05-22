## 1. Install dependencies

Follow Godot's instructions to set up your system to build Godot. You don't need the Godot source code, so stop before then. You only need the build tools, specifically `scons`, `python`, a compiler, and any other tools these pages identify. They provide easy installation instructions.

* [Windows](https://docs.godotengine.org/en/latest/contributing/development/compiling/compiling_for_windows.html)
* [Linux/BSD](https://docs.godotengine.org/en/latest/contributing/development/compiling/compiling_for_linuxbsd.html)
* [OSX](https://docs.godotengine.org/en/latest/contributing/development/compiling/compiling_for_macos.html)


## 2. Download this repository

You can either grab the zip file, or clone it on the command line. Only type in the commands after the $ prompts.

```
$ git clone git@github.com:outobugi/GDExtensionTerrain.git

Cloning into 'GDExtensionTerrain'...
Enter passphrase for key:
remote: Enumerating objects: 125, done.
remote: Counting objects: 100% (125/125), done.
remote: Compressing objects: 100% (79/79), done.
remote: Total 125 (delta 56), reused 94 (delta 36), pack-reused 0
Receiving objects: 100% (125/125), 42.20 KiB | 194.00 KiB/s, done.
Resolving deltas: 100% (56/56), done.

$ cd GDExtensionTerrain

GDExtensionTerrain$ git submodule init
Submodule 'godot-cpp' (https://github.com/godotengine/godot-cpp.git) registered for path 'godot-cpp'

GDExtensionTerrain$ git submodule update
Cloning into 'C:/GD/_plugins/TerrainGDEX/godot-cpp'...
Submodule path 'godot-cpp': checked out '9d1c396c54fc3bdfcc7da4f3abcb52b14f6cce8f'

```
Note the version it checked out: **9d1c396**...

This number is important for the next section.

## 3. Identify the appropriate godot-cpp version

Your version of godot-cpp should match the version of your Godot engine. e.g. Godot Engine 4.0.2 and godot-cpp 4.0.2. In the past this was very strict. At this time, it seems that 4.0 and 4.02 are interchangeable. Use one of these steps to find the correct commit hash.


### Using tags
On the [repository page](https://github.com/godotengine/godot-cpp), click the branch selector, then `Tags` to identify available tags that match the Godot engine binary you wish to use. If your engine version is in this list, e.g. `godot-4.0.2-stable`, great, move on to step 4. Otherwise explore the commit history on the website or command line as shown below.

![image](https://user-images.githubusercontent.com/632766/234943310-efa46891-00da-46c1-9200-598d6635fcc7.png)


### Using the commit history
If your engine version doesn't have a tag assigned, you can look at the [godot-cpp commit history](https://github.com/godotengine/godot-cpp/commits/master
) for a commit that syncs the repository to the upstream engine version. Search for entries named `Sync with upstream commit...`.

Eg, from Godot 4.0-stable.

![image](https://user-images.githubusercontent.com/632766/226097806-8a7d17da-7765-44a1-a142-23e291fcb2d3.png)

Clicking the `...` on the right expands the description which shows `4.0-stable`. This is the correct entry, and you want the commit string on the right in blue. Click the two overlapping squares on the right to copy the commit hash (`9d1c396`).


### Using the command line
Alternatively you can use git to search on the command line. This will search the server (origin) for all commit messages with `stable` allowing you to find the commits. Make sure to grab the top commit message (`9d1c396..`), not the upstream commit shown at the bottom, which is from the Godot repository.
```
GDExtensionTerrain/godot-cpp$ git log origin -Gstable
commit 9d1c396c54fc3bdfcc7da4f3abcb52b14f6cce8f (HEAD -> master, tag: godot-4.0-stable, origin/master, origin/HEAD, origin/4.0)
Author: Rémi Verschelde <rverschelde@gmail.com>
Date:   Wed Mar 1 15:32:44 2023 +0100

    gdextension: Sync with upstream commit 92bee43adba8d2401ef40e2480e53087bcb1eaf1 (4.0-stable)

[truncated]
```

Note, for updating you may need to update your git repo:
```
$ git fetch
From https://github.com/godotengine/godot-cpp
 * [new tag]         godot-4.0.1-stable -> godot-4.0.1-stable
 * [new tag]         godot-4.0.2-stable -> godot-4.0.2-stable
```
Now we can search the logs above, or use the two new tags that were found, eg, godot-4.0.2-stable.


## 4. Check out the correct version
Once you have the proper tag or commit string, you just need to check it out. If using a commit string, you may use either the full hash or just the first 6-8 characters, so `9d1c396` would also match 4.0-stable.

These examples will change the godot-cpp repository to 4.0-stable and 4.02-stable, respectively:

```
GDExtensionTerrain$ cd godot-cpp

GDExtensionTerrain/godot-cpp$ git checkout 9d1c396c54fc3bdfcc7da4f3abcb52b14f6cce8f
HEAD is now at 9d1c396 gdextension: Sync with upstream commit 92bee43adba8d2401ef40e2480e53087bcb1eaf1 (4.0-stable)
```

or

```
GDExtensionTerrain/godot-cpp$ git checkout godot-4.0.2-stable
Previous HEAD position was 9d1c396 gdextension: Sync with upstream commit 92bee43adba8d2401ef40e2480e53087bcb1eaf1 (4.0-stable)
HEAD is now at 7fb46e9 gdextension: Sync with upstream commit 7a0977ce2c558fe6219f0a14f8bd4d05aea8f019 (4.0.2-stable)

```

## 5. Build the extension

```
GDExtensionTerrain/godot-cpp$ cd ..

GDExtensionTerrain$ scons
```

Upon success you should see something like this at the end:

```
Creating library project\addons\terrain\bin\libterrain.windows.debug.x86_64.lib and object project\addons\terrain\bin\libterrain.windows.debug.x86_64.exp
scons: done building targets.

```


## 6. Set up the extension in Godot

* Close Godot. (Not required the first time, but required when updating the files.)
* Copy the files in `project/addons/terrain` to your own project folder under `/addons/terrain`. 
* Open Godot and create or open a scene. Add a new node and search for Terrain3D.
* Make a new Terrain3DStorage resource.
* Save the storage resource to a binary .res file (optional but recommended).

## Other Build Options

The `scons` build system has additional useful options. These come from the GDExtension template we are using, so some options may not be supported or work properly for this particular plugin. e.g. The platform.

#### Clean up build files
```
scons --clean
```

#### Manually specify the target platform
Note this plugin is being build and tested on Windows and Linux. It will eventually be tested on MacOS, but mobile/web platforms are currently unknown.
```
# platform: Target platform (linux|macos|windows|android|ios|javascript)

scons platform=linux
```

#### Build release or debug_release templates
```
# target: Compilation target (editor|template_release|template_debug)

scons target=template_release
```

#### See all options
```
scons --help

and

scons -H
```

## Troubleshooting

### When running scons, I get these errors:

```
GDExtensionTerrain$ scons
scons: Reading SConscript files ...

scons: warning: Calling missing SConscript without error is deprecated.
Transition by adding must_exist=False to SConscript calls.
Missing SConscript 'godot-cpp\SConstruct'
File "C:\gd\_plugins\GDExtensionTerrain\SConstruct", line 6, in <module>
AttributeError: 'NoneType' object has no attribute 'Append':
  File "C:\gd\_plugins\GDExtensionTerrain\SConstruct", line 9:
    env.Append(CPPPATH=["src/"])

```

Your godot-cpp directory is probably empty. Review the instructions above for updating the submodule.

### I can build the plugin, however Godot instantly crashes. 
Your godot-cpp version probably does not match your engine version. At this time, they must match. Review the instructions above for switching versions. Test the example project in the next question.

### How can I make sure godot-cpp is the right version and working?
You'll find a test project in `godot-cpp/test/`. Make sure this test project works with your Godot version first, then come back and try GDExtensionTerrain again.
  * Build the example plugin in `godot-cpp/test/`.
  * Copy `example.gdextension` and `bin` into the root folder of your project.
  * Run Godot. If it crashes, you're on the wrong version, or Godot-cpp has a problem that the maintainers will need to resolve.
  * Create a new scene.
  * Add a new `Example` node. When clicking the node, you should see an `Example` section and `Property From List` and various `Dproperty#` variables in the inspector.
