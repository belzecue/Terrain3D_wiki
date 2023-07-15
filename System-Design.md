Some terrain systems generate grid of mesh chunks, centered around the camera. As the camera moves forward, old meshes far behind are destroyed and new meshes in front are created.

Like The Witcher 3, this system uses a geometry clipmap, where the mesh components are generated once, and at periodic intervals is recentered on the camera location. Terrain3D updates if the camera moves 50% the width of the size of LOD0 (`clipmap_size`, which defaults to 48 units). On each update, the vertex heights are adjusted by the GPU reading from the terrain heightmap. LODs are built into the clipmap mesh so don't require any additional consideration once put in place and heights are adjusted.

### Reference Material
* [GPU Gems 2: Terrain Rendering Using GPU-Based Geometry Clipmaps](https://developer.nvidia.com/gpugems/gpugems2/part-i-geometric-complexity/chapter-2-terrain-rendering-using-gpu-based-geometry)

* [Geometry clipmaps: simple terrain rendering with level of detail](https://mikejsavage.co.uk/blog/geometry-clipmaps.html)

* GDC 2014: [The Witcher 3 Clipmap Terrain and Texturing](https://archive.org/details/GDC2014Gollent) - [Slides](https://ubm-twvideo01.s3.amazonaws.com/o1/vault/GDC2014/Presentations/Gollent_Marcin_Landscape_Creation_and.pdf)
