Charles Bloom compares [some of these techniques](http://cbloomrants.blogspot.com/2009/06/06-17-09-dxtc-more-followup.html) in more detail.


### YCoCg-DXT5 ###

A simple way of obtaining higher quality compression is also using the YCoCg color space, but encoding the Y component in the in the alpha channel of a DXT5 block, and the CoCg components in the RGB channels. That was originally suggested by Waveren in [Real-Time DXT Compression](http://www.intel.com/cd/ids/developer/asmo-na/eng/dc/index.htm). This approach  was described in more detail by Waveren and Castano in [Real-Time YCoCg-DXT Compression](http://developer.nvidia.com/object/real-time-ycocg-dxt-compression.html).

### Humus' Compression ###

[Emil Persson (aka Humus)](http://www.humus.name) also has [a demo](http://www.humus.name/index.php?page=3D&ID=68) that implements a similar idea, but  using YCbCr color space instead. He is actually using two textures, either one ATI1N (BC4/DXT5A) texture to store the luma (Y) and one ATI2N (BC5/3Dc) texture to store the chroma (CbCr), or one DXT1 texture to store the luma and one DXT5 (really DXT5nm) texture to store the chroma. The second texture that stores the chroma is a quarter the size to achieve a compression to 6 bits per pixel, as opposed to 8 bits per pixel for YCoCg DXT5. As shown in the figure below, the compression quality is not as good as YCoCg-DXT5. It also has lower performance, since it requires two texture lookups.

![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/humus.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/humus.png)

_Graph courtesy of J.M.P. van Waveren._

### Tom Forsyth's Compression ###

Another alternative is one suggested by [Tom Forsyth](http://www.eelpi.gotdns.org/):

One alternative I want to try is using two DXT1 textures with the same mapping and with one of them using the 1-bit alpha that you get for free. Then simply lerp between them. This allows each pixel to be one of three colours from tex0, or if tex0 is the alpha=0 colour, it can be one of 4 colours from tex1. Pixel shader is pretty simple:

```
tex t0
tex t1
lerp r0, t0.a, t0, t1
```

This approach is interesting, but according to Tom, it doesn't work that well in practice.

Instead he suggests doing something similar to Humus' compression, but using an uncompressed RGB-565 texture, and a separate luminance texture. The original texture is downsampled to 1/4th its size, and the luminance texture encodes the luminance difference between the original and the downsampled texture. The luminance deltas usually have a small range, so you could use a per texture scale factor to scale the deltas to the representable range, and obtain higher precision. Tom [explains it with more details on his blog](http://home.comcast.net/~tom_forsyth/blog.wiki.html#%5B%5BTexture%20formats%20for%20faster%20compression%5D%5D).

The advantage of this method is that compression is very fast, the downsampled texture is only quantized, and the luminance texture is 1D, so it has a small search space. He has not done any PSNR analysis, thought.


### Bungie Compression ###

The basic idea is to compress a texture once, compute the difference between the compressed texture and the original, and compress that difference again.

http://research.microsoft.com/research/pubs/view.aspx?tr_id=1198

https://www.cmpevents.com/GD08/a.asp?option=C&V=11&SessID=6221


### RGBV ###

This is a variant of RGBM using two DXT1 blocks in order to get free alpha mask.

http://files.wayforward.com/shane/rgbv/