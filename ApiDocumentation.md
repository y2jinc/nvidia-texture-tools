## Contents ##

  * [Introduction](ApiDocumentation#Introduction.md)
  * [Usage](ApiDocumentation#Usage.md)
  * [Input Options](ApiDocumentation#Input_Options.md)
    * [Specifying Input Images](ApiDocumentation#Specifying_Input_Images.md)
    * [Mipmap Generation](ApiDocumentation#Mipmap_Generation.md)
    * [Alpha Mode](ApiDocumentation#Alpha_Mode.md)
    * [Gamma Correction](ApiDocumentation#Gamma_Correction.md)
    * [Processing Normal Maps](ApiDocumentation#Processing_Normal_Maps.md)
    * [Generating Normal Maps](ApiDocumentation#Generating_Normal_Maps.md)
    * [Image Resizing](ApiDocumentation#Image_Resizing.md)
    * [Color Transforms](ApiDocumentation#Color_Transforms.md)
  * [Compression Options](ApiDocumentation#Compression_Options.md)
    * [Compression Formats](ApiDocumentation#Compression_Formats.md)
    * [Compression Quality](ApiDocumentation#Compression_Quality.md)
    * [GPU Acceleration](ApiDocumentation#GPU_Acceleration.md)
    * [Color Weighting](ApiDocumentation#Color_Weighting.md)
    * [Pixel Format Conversion](ApiDocumentation#Pixel_Format_Conversion.md)
    * [Quantization](ApiDocumentation#Quantization.md)
  * [Output Options](ApiDocumentation#Output_Options.md)
    * [Output Handler](ApiDocumentation#Output_Handler.md)
    * [Producing DDS Files](ApiDocumentation#Producing_DDS_Files.md)
    * [Reporting Progress](ApiDocumentation#Reporting_Progress.md)
    * [Handling Errors](ApiDocumentation#Handling_Errors.md)

## Introduction ##

The NVIDIA Texture Tools library (NVTT for short) is a C++ library that allows you to create textures compressed in any of the DX10 texture formats, and apply certain transformations to them such as mipmap generation, and normal map conversion.

There are also bindings for other languages, see [LanguageBindings](LanguageBindings.md).

## Usage ##

By default NVTT is compiled as a shared library. To use it in your application you only have to link it implicitly. With gcc that's achieved using `-lnvtt`, and with Visual Studio you have to add `nvtt.lib` to the Linker -> Input -> Additional Dependencies field of the project settings. For compilation instructions see [CompilationInstructions](CompilationInstructions.md).

In order to access the API you only have to include the [nvtt.h](http://code.google.com/p/nvidia-texture-tools/source/browse/trunk/src/nvtt/nvtt.h) header file:

```
#include <nvtt/nvtt.h>
```

All the members of the API are in the `nvtt` namespace. Members outside of the `nvtt` namespace should not be considered public and are subject to change. If you want your code to always stay compatible with the latest release of NVTT, you should not include other headers, nor use members outside of the `nvtt` namespace.

As new features are added to NVTT, binary compatibility will always be preserved. That should allow upgrading to new versions without even having to recompile the application code.

The most important function of the NVTT API is the following:

```
bool Compressor::process(const InputOptions &, const CompressionOptions &, const OutputOptions &);
```

It takes the texture defined by `InputOptions`, compresses it according to the `CompressionOptions` and outputs it according to the `OutputOptions`. These classes are documented in detail in the following sections.

Here's a very simple usage example:

```
InputOptions inputOptions;
inputOptions.setTextureLayout(TextureType_2D, w, h);
inputOptions.setMipmapData(data, w, h);

OutputOptions outputOptions;
outputOptions.setFileName("output.dds");

CompressionOptions compressionOptions;
compressionOptions.setFormat(Format_DXT1);

Compressor compressor;
compressor.process(inputOptions, compressionOptions, outputOptions);
```


## Input Options ##

The `InputOptions` class describes the images that compose the compressed texture, and several image processing operations that can be applied to them.


### Specifying Input Images ###

Before specifying the image data, the layout of the texture has to be defined. This is done with the following method:

```
void InputOptions::setTextureLayout(TextureType type, int width, int height, int depth=1);
```

where texture `type` is one of the following:

  * TextureType\_2D
  * TextureType\_Cube

Note that 3D textures are not supported yet. The `depth` argument should always be equal to 1.

Once the layout of the texture has been defined, the data for each of the images can be provided with the following method:

```
bool InputOptions::setMipmapData(const void * data, int w, int h, int d, int face, int mipmap);
```

NVTT internally allocates a copy of the provided data, that allows you to delete or reuse the memory.

If the width, height or depth of the provided mipmap does not match the one expected by the layout, then the function returns false, otherwise it returns true.

Again, since 3D textures are not supported, the depth argument should always be set to 1.

**TODO**: explain how the size of the mipmaps is computed, specially for non powers of two.


### Mipmap Generation ###

Each of the faces of the surface is composed of a mipmap chain. You can provide each of these images explicitely with the `setMipmapData` method, but you can also disable mipmap generation or let the library compute the mipmaps procedurally. The following method provides some control over this process:

```
void InputOptions::setMipmapGeneration(bool enabled, int maxLevel);
```

In order to output mipmaps for each of the faces of the surface, the `enabled` argument should be set to true.

If the mipmap image is not provided explicitly by the application, and generate mipmaps is enabled, then the mipmaps will be generated automatically from the previous level using a downsampling filter. It's also possible to provide incomplete mipmap chains, and the missing levels will be generated using the same process.

By default the entire mipmap chain is generated, but it's possible to limit the number of mimap levels generated with the `maxLevel` argument. When `maxLevel` is set to -1, this argument has no effect.

It's possible to specify the mipmap generation filter using the following method:

```
void InputOptions::setMipmapFilter(MipmapFilter filter);
```

Where `filter` is one of the following options:

  * MipmapFilter\_Box
  * MipmapFilter\_Triangle
  * MipmapFilter\_Kaiser

`MipmapFilter_Box` is a [polyphase box filter](http://developer.nvidia.com/object/np2_mipmapping.html). It's the default option and good choice for most cases. It's also much faster than the other filters.

`MipmapFilter_Triangle` uses a triangle filter. The kernel has a larger width and thus produces blurrier results than the box filter.

`MipmapFilter_Kaiser` is a Kaiser-windowed sinc filter. That's generally considered the best choice for downsampling filters, but in order to obtain best results it may be required to tweak some of the parameters of the filter, otherwise the resulting images could suffer from ringing artifacts or the result may not be as sharp as desired. The default values of the parameters are generally safe, but it's possible to tweak them using the following method:

```
void InputOptions::setKaiserParameters(float width, float alpha, float stretch);
```

The default values are:

```
inputOptions.setKaiserParameters(3.0f, 4.0f, 1.0f);
```

Larger kernel widths are supposed to approximate better a perfect low pass filter, but sometimes result in ringing artifacts. Values above 5 are not recommended. You should not change the alpha and stretch values unless you know what you are doing.

You can compare the result of each of these filters at [ResizeFilters](ResizeFilters.md). For more information about mipmap generation algorithms see [MipmapGeneration](MipmapGeneration.md).

When evaluating the color of texels that are near the border, most the filters usually sample outside of the texture. By default, NVTT assumes the texture wrapping mode is to mirror, because that generally looks good, but better results can be achieved by explicitly specifying the desired wrapping mode. That can be done with the following method:

```
void InputOptions::setWrapMode(WrapMode wrapMode);
```

Where the `wrapMode` argument must have one the following values:

  * WrapMode\_Mirror
  * WrapMode\_Repeat
  * WrapMode\_Clamp

Note that the `wrapMode` is also used for other image processing operations, like normal map generation. So, it's a good idea to set it even if you are not generating mipmaps. The default `wrapMode` is `WrapMode_Mirror`.


### Alpha Mode ###

The contents of the alpha channel influences the mipmap generation and compression of the images. If the alpha channel contains transparency information that can be taken into account to produce more accurate results.

In order to achieve that, the alpha mode can be specified with the following method:

```
void InputOptions::setAlphaMode(AlphaMode alphaMode);
```

Where the `alphaMode` argument has one of the following values:

  * AlphaMode\_None
  * AlphaMode\_Transparency
  * AlphaMode\_Premultiplied

When the `alphaMode` is `AlphaMode_None` the alpha and color channels are processed independently. However, if the alpha is used for transparency it should be set to `AlphaMode_Transparency`, except when the alpha is premultiplied, in which case `AlphaMode_Premultiplied` should be used. The default `alphaMode` is `AlphaMode_None`.


### Gamma Correction ###

For a good discussion about why gamma correction is important and why you should filter your images in linear space, read <a href='http://number-none.com/product/Mipmapping,%20Part%202/index.html'>Jon Blow's Mipmapping article at the Inner Product</a>.

In order to process images that are not in linear space, and output images in the desired gamma space, NVTT allows you to specify the input and output gamma values with the following method:

```
void InputOptions::setGamma(float inputGamma, float outputGamma);
```

By default, both the `inputGamma` and the `outputGamma` are set to 2.2. Gamma correction is only applied to the RGB channels. It's not applied to the alpha channel and it's never applied to normal maps. You can disable gamma correction by setting these values to 1.0 as follows:

```
inputOptions.setGamma(1.0f, 1.0f);
```


### Processing Normal Maps ###

If the input image is a normal map, it may require some special treatment. For example, gamma correction won't be applied, and some operations will only be active for normal maps. To indicate that the input image is a normal map the following method is used:

```
void InputOptions::setNormalMap(bool isNormalMap);
```

Normal map mipmaps are generated by down filtering the previous normal map level, just like with regular images. However, after down filtering the resulting normals are generally not unit length. By default, these normals are re-normalized, but you can chose to left them unnormalized with the following method:

```
void InputOptions::setNormalizeMipmaps(bool normalizeMipmaps);
```


### Generating Normal Maps ###

**TODO**: Explain how normal maps are generated from color images or height maps.

```
inputOptions.convertToNormalMap(true);
```


```
bool InputOptions::setHeightEvaluation(float r, float g, float b, float a);
```

For example, to use the alpha channel of the input texture as the height you would use the following weights:

```
inputOptions.setHeightEvaluation(0, 0, 0, 1);
```

The height factors do not necessarily sum 1. So, you can also use them to change the steepness of the height map, and the sharpness of the resulting normal map.

**TODO**: Explain normal filter, add wiki page that compares different values.

```
void InputOptions::setNormalFilter(float small, float medium, float big, float large);
```

Mipmap generation behaves as with regular normal maps. The input images are first converted to normal map. Then the mipmaps are generated from the normal maps as usual, unless the user provides the mipmaps explicitly, in which case normal map conversion is applied again.

**TODO**: Normal map conversion is only done to the input images. Mipmaps are generated from the normal map as usual.


### Image Resizing ###

It's possible to constrain the size of the textures to a certain limit. This is accomplished with the following method:

```
void InputOptions::setMaxExtents(int d);
```

NVTT is able to handle non power of two textures correctly, but not all hardware supports them. It's possible to round the size of the input images to the next, nearest, or previous power of two. The desired rounding mode is specified with the following method.

```
void InputOptions::setRoundMode(RoundMode mode);
```

where `RoundMode` is one of the following:

  * RoundMode\_None
  * RoundMode\_ToNextPowerOfTwo
  * RoundMode\_ToNearestPowerOfTwo
  * RoundMode\_ToPreviousPowerOfTwo

The default round mode is none.


### Color Transforms ###

The colors of the input image can also be transformed to a different color space. This transform is specified with the following method:

```
void InputOptions::setColorTransform(ColorTransform t);
```

Where `ColorTransform` is one of the following:

```
enum ColorTransform
{
    ColorTransform_None,
    ColorTransform_Linear,
    ColorTransform_Swizzle,
};
```

Currently only one transform can be specified at a time. By default no transformation is performed, which is equivalent to `ColorTransform_None`.

Linear color transforms are performed in linear space and before mipmap generation. The transformation of every channel is specified with the following function:

```
void InputOptions::setLinearTransform(int channel, float w0, float w1, float w2, float w3, float offset);
```

For example, to transform the input image to YCbCr color space:

```
Y  =       + 0.299    * R + 0.587    * G + 0.114    * B
Cb = 128   - 0.168736 * R - 0.331264 * G + 0.5      * B
Cr = 128   + 0.5      * R - 0.418688 * G - 0.081312 * B
```

You would use the following code:

```
inputOptions.setLinearTransform(0,  0.299,     0.587f,    0.114f,   0);
inputOptions.setLinearTransform(1, -0.168736, -0.331264,  0.5,      0, 128);
inputOptions.setLinearTransform(2,  0.5,      -0.418688, -0.081312, 0, 128);
inputOptions.setLinearTransform(3,  0,         0,         0,        1);
```

If you simply want to swizzle the channels, you can also achieve that with `setLinearTransform`, but it's much more convenient to use the following function:

```
void InputOptions::setSwizzleTransform(int x, int y, int z, int w);
```

Where the indices represent the following channels or values:

| **index** | **channel/value** |
|:----------|:------------------|
| 0         | R                 |
| 1         | G                 |
| 2         | B                 |
| 3         | A                 |
| 4         | 1.0               |
| 5         | 0.0               |
| 6         | -1.0              |



## Compression Options ##

**TODO**

### Compression Formats ###

NVTT supports all the Direct3D10 block compression formats and some variations that use these formats to represent normals or to represent colors with higher quality. The compression format is specified as follows:

```
compressionOptions.setFormat(format);
```

The supported block compression formats are the following:

  * Format\_BC1 (DXT1)
> This is the default block compression format. By default, the implicit 1 bit alpha channel is not used and is always opaque.

  * Format\_BC1a (DXT1a)
> This is a variation of BC1 that supports a 1 bit alpha channel.

  * Format\_BC2 (DXT3)
> This is a block compression format similar to BC1, but with an explicit 3 bit alpha channel.

  * Format\_BC3 (DXT5)
> This is a block compression format similar to BC1, but with a compressed alpha block.

  * Format\_BC3n (DXT5nm)
> This is a variation of BC3 that is used to represent normal maps by encoding the X and Y components as follows: R=1, G=Y, B=0, A=X, this swizzle is used to facilitate decompression using [Capcon's DXT decompression trick](http://code.google.com/p/nvidia-texture-tools/wiki/NormalMapCompression#Capcon%27s_DXT_trick)

  * Format\_BC4 (ATI1)
> This is a block compression format that only contains a single alpha block.

  * Format\_BC5 (LATC, RGTC, 3Dc, ATI2)
> This is a block compression format that contains two alpha blocks. It's typically used to compress normal maps.

The following formats are only available in NVTT 2.0:

  * Format\_DXT1n
> Not yet supported.

  * Format\_CTX1
> Not yet supported.

  * Format\_YCoCg\_DXT5
> Not yet supported.


### Compression Quality ###

It's possible to control the quality of the compressor using the following method:

```
compressionOptions.setQuality(quality);
```

Where `quality` is one of the following:

  * Quality\_Fastest
  * Quality\_Normal
  * Quality\_Production
  * Quality\_Highest

`Quality_Fastest` will select a quick compressor that produces reasonable results in a very short amount of time. Note that this is not a real-time compressor. The code is not highly optimized, but still it's generally one order of magnitude faster than the normal CPU compression mode. If you need real-time compression you can find more resources at: [RealTimeDXTCompression](RealTimeDXTCompression.md).

`Quality_Normal` is the default mode and the one you should use in most cases. It provides a good trade off between compression quality and speed.

`Quality_Production` will generally produce similar results as `Quality_Normal`, but it may double or triple the compression time to obtain minor quality improvements.

`Quality_Highest` is a brute force compressor. In some cases, depending on the size of the search space, this compressor will be extremely slow. Use this only for testing purposes, to determine how much room is left for improvement in the regular compressors.

The following table indicates what compressor types are available for each of the compression formats.

|      | **Fastest** | **Normal** | **Production** | **Highest** |
|:-----|:------------|:-----------|:---------------|:------------|
| BC1  | Yes         | Yes        |                |             |
| BC1a | Yes         |            |                |             |
| BC2  | Yes         | Yes        |                |             |
| BC3  | Yes         | Yes        |                |             |
| BC3n | Yes         | Yes        |                |             |
| BC4  | Yes         | Yes        | Yes            |             |
| BC5  | Yes         | Yes        | Yes            |             |

Selecting a compression quality that's not available won't produce any error, but instead it will fall back to another compression mode. If `Quality_Production` is not available, `Quality_Normal` will be used instead, and if `Quality_Normal` is not available, `Quality_Fastest` will be used. `Quality_Highest` will never be used as a fallback.


### GPU Acceleration ###

Not all the compressors are GPU accelerated. Some formats can be compressed relatively fast, but others are much more expensive. Currently GPU acceleration is implemented only for the slowest compression modes, as shown in the following table:

|      | **GPU Accelerated** |
|:-----|:--------------------|
| BC1  | Yes                 |
| BC1a |                     |
| BC2  | Color only          |
| BC3  | Color only          |
| BC3n |                     |
| BC4  |                     |
| BC5  |                     |

High quality texture compression is a complex problem that would be very hard to solve using traditional GPGPU approaches (that is, using the graphics API, vertex, and fragment shaders). For this reason GPU compression is implemented in CUDA. Note that currently, CUDA is only available on NVIDIA GeForce 8 series.

When available, CUDA compression is enabled by default, but can be disabled as follows:

```
compressor.enableCudaAcceleration(false);
```

The GPU compressors do not provide exactly the same result as the CPU compressors. However, the difference between the two compressors is always very small and not noticeable to the eye.

### Color Weighting ###

By default, the compression error is measured for each channel uniformly. However, for some images it makes more sense to measure the error in a perceptual color space. This can be achieved with the following function:

```
compressionOptions.setColorWeights(float red, float green, float blue, float alpha=1.0f);
```

The choice for the weights of each color channel is subjective. In many cases uniform color weights (1.0, 1.0, 1.0) work very well. A popular choice is to use the NTSC luma encoding weights (0.2126, 0.7152, 0.0722), but I think that blue contributes to our perception more than a 7%. A better choice in my opinion is (3, 4, 2).


### Pixel Format Conversion ###

In addition to the block compression formats, NVTT can also convert the input image to an arbitrary pixel format. That's achieved selecting the `Format_RGBA` output format as follows:

```
compressionOptions.setFormat(Format_RGBA);
```

The output pixel format can be described with one of these two functions:

```
void CompressionOptions::setPixelFormat(uint bitcount, uint rmask, uint gmask, uint bmask, uint amask);
void CompressionOptions::setPixelFormat(unsigned char rsize, unsigned char gsize, unsigned char bsize, unsigned char asize);
```

The first one is more flexible, but it's limited to 32 bits per color. The color masks allow you to indicate the position and size of each of the color components.

For example, to select the RGB 5:6:5 output format you would use the following code:

```
compressionOptions.setPixelFormat(16, 0x001F, 0x07E0, 0xF800, 0);
```

The second function is only available in NVTT 2.0. It assumes that the colors are in RGBA order and that there are no padding bits between color components. However, colors don't have any size limitation. This function is specially useful to output floating point color formats, whose size is generally greater than 32 bits.

To indicate that the desired output is in floating point, you should use the following function:

```
void CompressionOptions:: setPixelType(PixelType pixelType);
```

Here are some usage examples to produce some of the DX10 floating point formats:

```
// DXGI_FORMAT_R16G16_FLOAT
compressionOptions.setPixelType(PixelType_Float);
compressionOptions.setPixelFormat(16, 16, 0, 0);

// DXGI_FORMAT_R32G32B32A32_FLOAT
compressionOptions.setPixelType(PixelType_Float);
compressionOptions.setPixelFormat(32, 32, 32, 32);
```

By default pixel rows of unaligned pixel formats are aligned to the next byte boundary. However, it's possible to change that behavior using the following function:

```
void CompressionOptions:: setPitchAlignment(int pitchAlignment);
```

Where `pitchAlignment` must be a positive number and a power of two, and is given in bytes. By default the pitch alignment is set to 1. Note, that previous versions of NVTT incorrectly aligned the pitch to 4 bytes by default. So, to obtain the same behavior you now have to explicitly set the alignment as follows:

```
compressionOptions.setPitchAlignment(4);
```


### Quantization ###

The compressor can dither the colors to the output bitcount before compression or during quantization. When using DXT compression this does not generally improve the quality of the output image, but in some cases it can produce smoother results. It's generally a good idea to enable dithering when the output format is `Format_RGBA`.

Color and alpha dither can be enabled independently with the following option:

```
void CompressionOptions::setQuantization(bool colorDithering, bool alphaDithering, bool binaryAlpha, int alphaThreshold = 127);
```

**TODO** Explain binary alpha, alpha threshold.

## Output Options ##

The most simple way of configuring the output options is by only providing the name of the file that will contain the resulting DDS texture:

```
OutputOptions::setFileName(const char *);
```



```
OutputOptions::setOutputHandler(OutputHandler *);
```


### Output Handler ###

The output handler is an interface that defines these three methods:

```
virtual void beginImage(int size, int width, int height, int depth, int face, int miplevel) = 0;
virtual bool writeData(const void * data, int size) = 0;
virtual void endImage() = 0;                                                                      // NVTT 2.1
```

Applications need to implement this interface in order to receive compressed textures from the compressor. The `writeData` method is first called with the contents of the texture header, and after that, the compressor calls `beginImage` and `endImage` to delimitate the data that corresponds to each image of the texture. You can use these two methods for example to allocate and deallocate temporary memory to hold the data
received through the `writeData` method.

Note that some compressors output one compressed block at a time, while others output one image at a time, so your implementation of the `OutputHanlder` needs to be designed to handle multiple consecutive calls to `writeData` for each compressed image.


### Producing Texture Files ###

By default the compressor calls the `writeData` method with the contents of a texture header. If you don't want to output the texture to a file, you can ignore these calls, or disable them with:

```
outputOptions.setOutputHeader(false);
```

If however, you want to generate a texture file, you can choose what container format as follows:

```
outputOptions.setContainer(container);
```

where container is one of:

  * `Container_DDS`
  * `Container_DDS10`

In the future it may be interesting to add support for other formats such as [VTF](http://developer.valvesoftware.com/wiki/Valve_Texture_Format) or [KTX](http://www.khronos.org/opengles/sdk/tools/KTX/file_format_spec/).


### Reporting Progress ###

**TODO**


### Handling Errors ###

NVTT will produce errors in some circumstances. In order to detect them it's possible to register an error handler with the following method:

```
OutputOptions::setErrorHandler(ErrorHandler *);
```

The `ErrorHandler` interface has a single method:

```
virtual void error(Error e) = 0;
```

That receives one of these error codes:

  * Error\_Unknown
  * Error\_InvalidInput
  * Error\_UnsupportedFeature
  * Error\_CudaError
  * Error\_FileOpen
  * Error\_FileWrite

It's possible to translate the error codes to strings using the following function:

```
const char * nvtt::errorString(Error e);
```