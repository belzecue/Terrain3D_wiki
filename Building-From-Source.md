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
Submodule path 'godot-cpp': checked out '19091138895d35e1ce69742889b8bfd82be57f17'

```
Note the version it checked out: **1909113**...

This number is important for the next section.

## 3. Identify the appropriate godot-cpp version

At this time your version of godot-cpp must match the version of your Godot engine exactly. Use one of these steps to get the correct commit hash.

### Using the website
Look at the [godot-cpp commit history](https://github.com/godotengine/godot-cpp/commits/master
) for your version. Search for entries named `Sync with upstream commit...`.


Currently, the most recent one is Godot 4 Beta 14.
![image](https://user-images.githubusercontent.com/632766/214382959-c6143e07-eb11-43ff-b654-75ed99fd033f.png)

Clicking the `...` on the right expands the description which shows `beta14`. This is the correct commit. You can click the two overlapping squares on the right to copy the commit hash (`1909113`).


### Using the command line
Alternatively you can use git to search on the command line. This will search the server (origin) for all commit messages with `beta` allowing you to find the commits. Make sure to grab the top commit message (a8d8..), not the upstream commit, which is from the Godot repository.
```
GDExtensionTerrain/godot-cpp$ git log origin -Gbeta
commit a8d848506074b1bb7eae319b06e9de60395eee1f
Author: Rémi Verschelde <rverschelde@gmail.com>
Date:   Fri Jan 27 17:02:22 2023 +0100

    gdextension: Sync with upstream commit 518b9e5801a19229805fe837d7d0cf92920ad413 (4.0-beta16)
[truncated]
```

### Recent commits
Here are some godot-cpp commits for recent releases of Godot 4:
* Beta 16 `a8d848506074b1bb7eae319b06e9de60395eee1f`
* Beta 15 `ae1afba8d126e2b577ba358f17b2843787f71ef1`
* Beta 14 `19091138895d35e1ce69742889b8bfd82be57f17`
* Beta 13 `cb15429e4a2cf0682acd626e7ecf703c2a159460`
* Beta 12 `151ea35c5fbb0254e0d3d29e230270b60852915f`

### Check out the right version
Once you have the commit string, you just need to check it out. Git is smart enough to determine the hash you want with only the first 6-8 or so characters in a hash string. So `1909113` matches beta 14 above.

If the version checked out in step 3 above is not the version you want, then go back to the command line and change it. This will change it to Beta 12 for instance:

```
GDExtensionTerrain$ cd godot-cpp

GDExtensionTerrain/godot-cpp$ git checkout 151ea35c5fbb0254e0d3d29e230270b60852915f
Previous HEAD position was 1909113 gdextension: Sync with upstream commit 28a24639c3c6a95b5b9828f5f02bf0dc2f5ce54b (4.0-beta14)
HEAD is now at 151ea35 gdextension: Sync with upstream commit 3c9bf4bc210a8e6a208f30ca59de4d4d7e18c04d (4.0-beta12)

```

## 4. Build the extension

```
GDExtensionTerrain/godot-cpp$ cd ..

GDExtensionTerrain$ scons
```

Upon success you should see something like this at the end:

```
Creating library project\addons\terrain\bin\libterrain.windows.debug.x86_64.lib and object project\addons\terrain\bin\libterrain.windows.debug.x86_64.exp
scons: done building targets.

```


## 5. Set up the extension in Godot

* Close Godot. (Not required the first time, but required when updating the files.)
* Copy the files in `project/addons/terrain` to your own project folder under `/addons/terrain`. 
* Open Godot and create or open a scene. Add a new node and search for Terrain3D.


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
