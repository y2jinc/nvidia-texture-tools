## References ##

  * Jon Blow articles in Game Developer Magazine:
    * [Mipmapping, Part 1](http://number-none.com/product/Mipmapping,%20Part%201/index.html)
    * [Mipmapping, Part 2](http://number-none.com/product/Mipmapping,%20Part%202/index.html)

  * [Non-Power-of-Two Mipmapping](http://developer.nvidia.com/object/np2_mipmapping.html)

## Conclusions ##

Here are some of my findings:

  * Windowed sinc filters with radius greater than 3 produce too many ringing artifacts.
  * For good results with non power of two textures or arbitrary scales, you must use polyphase filters.
  * Sampling the polyphase filter with a box filter greatly improves the quality.
  * Using a triangle filter looks even better, but the difference is small.
  * For most cases box does a great job.
  * The Lanczos and Kaiser windows produce indistinguishable results.
  * Gamma correction produces more correct results, but sometimes enhances the ringing artifacts.