J.M.P. van Waveren's paper has a much more detailed analysis of the different ways a normal map can be compressed:

[Real-Time Normal Map DXT Compression](http://developer.nvidia.com/object/real-time-normal-map-dxt-compression.html)

Another interesting (but not so detailed) article on the topic is:

http://www.ozone3d.net/tutorials/bump_map_compression.php


### DXT5nm ###

This format was introduced in the original nvdxt library and since then it has become quite popular. It uses a DXT5 block to represent normals, storing the X component in the Alpha and the Y component in the Green channel.

### RXGB ###

The DOOM 3 engine and its derivates use the so called RXGB format, that is just like DXT5, but with the Red and Alpha components swapped, which means that the normal is given by the (a, g, b) components.

### BC5 ###

BC5 is a two component block compression format, where each component is represented with a DXT5 alpha block. It was originally called 3Dc by ATI and its main purpose was to encode normals storing only the X and Y components.

In DirectX 10 the 3Dc format was standarized and renamed to BC5.

BC5 can be loaded in OpenGL as LATC or RGTC. The LATC format is particularly convenient, because you can use the same swizzle that is used for DXT5nm, that's because the luminance is replicated in the RGB components.

### Capcon's DXT trick ###

As described by [Christer Ericson](http://realtimecollisiondetection.net/blog/?page_id=2) in [his blog](http://realtimecollisiondetection.net/blog/?p=28):

> One clever thing they do (as mentioned on these two slides) is to encode their normal maps so that you can feed either a DXT1 or DXT5 encoded normal map to a shader, and the shader doesn’t have to know. This is neat because it cuts down on shader permutations for very little shader cost. Their trick is for DXT1 to encode X in R and Y in G, with alpha set to 1. For DXT5 they encode X in alpha, Y in G, and set R to 1. Then in the shader, regardless of texture encoding format, they reconstruct the normal as `X = R * alpha`, `Y = G`, `Z = Sqrt(1 - X^2 - Y^2)`.

> A DXT5-encoded normal map has much better quality than a DXT1-encoded one, because the alpha component of DXT5 is 8 bits whereas the red component of DXT1 is just 5 bits, but more so because the alpha component of a DXT5 texture is compressed independently from the RGB components (the three of which are compressed dependently for both DXT1 and DXT5) so with DXT5 we avoid co-compression artifacts. Of course, the cost is that the DXT5 texture takes twice the memory of a DXT1 texture (plus, on the PS3, DXT1 has some other benefits over DXT5 that I don’t think I can talk about).


### Alternative Projections ###

Usually 3D normals are stored using only two components and the normal is reconstructed using an orthogonal projection:

`z = sqrt(1 - x*x - y*y)`

This is easy to compute on your shaders and has the advantage of providing more detail near the top of the hemisphere, that is flat normals are represented more accurately.

The main disadvantage is that the reconstruction is done after interpolation and unfortunately lines in 2D do not map to great circles when using this projection, and that can potentially cause interpolation artifacts.

Another approach is to use a stereographic projection. This projection has a slightly better behavior under interpolation and is only slightly more expensive. The distribution of normals is also more uniform, although it's not clear whether that's good or not.