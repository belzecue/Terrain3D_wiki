# Importing & Exporting Data

Currently importing and exporting is possible via code or our demo. We will [make a UI](https://github.com/outobugi/GDExtensionTerrain/issues/81)  eventually. In the meantime, we have written a script that uses the Godot Inspector as a makeshift UI. You can use it to make a data file for your other scenes.

## How To Import Data

1) Open up the Demo project and load `demo/Import.tscn`

2) Click Terrain3D and create a new Terrain3DStorage. Save the file wherever you wish. Make sure to save it as `.res`.

![image](https://github.com/outobugi/GDExtensionTerrain/assets/632766/bf5cd9ed-0285-425f-a011-7e6cd562015f)

3) Click on the Import node. 

4) In the inspector, `Import File` section, use the folder icons to specify a height map, and optional color or control maps. 

![image](https://github.com/outobugi/GDExtensionTerrain/assets/632766/8cdd2c75-6a24-4c2e-8223-2fd0fee97299)

5) Specify the location where in the world you want it. Y is ignored. X/Z are rounded to the nearest `region_size` (defaults to 1024). The map is placed centered on that point. So a location of (-2000, 100, 1000) will be imported centered around (-2048, 0, 1024).

6) Add an offset and/or scale to adjust every pixel on the heightmap. Offset is added first, then scale. `clr.r = (clr.r + p_offset) * p_scale;`

7) If you are importing an R16 heightmap, specify the min/max height range (eg. -328, 543), and image dimensions (eg 2048x2048). See [import formats](#supported-image-formats) below.

8) Click `Run import` and wait. If you have the console open, and the Terrain `debug_level` set to `debug`, you'll see progress or errors.

## How To Export Data

Start with steps 1-3 above.

3) Select the type of map you wish to extract: Height (standard), Color (rgba), Control (proprietary).

4) Specify the name, which will determine the file type based upon extension. See [export formats](#supported-export-formats).

5) Click `Run Export` and wait. 10-30s is normal. Look at your file system or the console for status.

## Supported Import Formats

We can import any supported image format Godot can read into any of the map types. These types include:
* [Image formats for external files](https://docs.godotengine.org/en/4.0/tutorials/assets_pipeline/importing_images.html#supported-image-formats): `bmp`, `dds`, `exr`, `hdr`, `jpg`, `jpeg`, `png`, `tga`, `svg`, `webp`.
* [Image formats stored in a Godot resource file](https://docs.godotengine.org/en/4.0/classes/class_image.html#enum-image-format): `.tres`, `.res`.
* R16 Height map aka RAW: For 16-bit heightmaps read/writable by World Machine, Unity, Krita, Photoshop, etc. Rename the extension to `.r16`. Min/Max heights and image size are not stored in the file, so you must keep track of them elsewhere (such as in the name).

Only EXR or R16 are recommended for heightmaps. Godot PNG only supports 8-bit per channel, so don't use it for heightmaps. It is fine for external editing of control and color maps which are RGBA. See [Terrain3DStorage](https://github.com/outobugi/GDExtensionTerrain/wiki/Terrain3DStorage#internal-data-storage).

Upon import, you can specify the location of import on the greater 16k^2 world. e.g. You could import multiple maps as separate or combined islands.

It will slice or pad odd sized images into region sized chunks (default is 1024x1024). e.g. You could import a 4k x 2k EXR into the height map, several 1k x 1k PNGs into the color map, and a 5123 x 3769 jpg into the control map and position them so they are adjacent.


## Supported Export Formats

We can export any map as `r16`, `exr`, `png`, `jpg`, `webp`, or native Godot resource formats `tres` for text and `res` for binary (with `ResourceSaver::FLAG_COMPRESS` enabled).

Only EXR or R16 are recommended for heightmaps. Godot PNG only supports 8-bit per channel, so don't use it for heightmaps. It is fine for external editing of control and color maps which are RGBA. See [Terrain3DStorage](https://github.com/outobugi/GDExtensionTerrain/wiki/Terrain3DStorage#internal-data-storage).

The exporter takes the smallest rectangle that will fit around all active regions in the 16k^2 world and export that as an image. So, if you have a 1k x 1k island in the NW corner, and a 2k x 3k island in the center, with a 1k strait between them, the resulting export image will be something like 4k x 5k. You'll need to specify the location (rounded to `region_size`) when reimporting to have a perfect round trip.

The demo tool does not offer region by region export, but there is an API where you can retrieve any given region, then you can use `Image` to save it externally yourself.
