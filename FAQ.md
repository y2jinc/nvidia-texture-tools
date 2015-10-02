### Where can I ask questions? How can I get support? ###
You can ask questions about the usage of the Texture Tools at the [NVIDIA developer forums](http://developer.nvidia.com/forums/index.php?showforum=9). You can report bugs and request new features in our [issue database](http://code.google.com/p/nvidia-texture-tools/issues/list). If you are a developer and have questions about the API or the source code, feel free to drop by the [developer's mailing list](http://groups.google.com/group/nvidia-texture-tools). If you would like to contact us privately, please send an email to [TextureTools@nvidia.com](mailto:TextureTools@nvidia.com).

### Why is feature XYZ not supported? ###
In order to keep the code small and reduce maintenance costs we have limited the
features available in our new texture tools. For this reason, we have also open sourced the code, so that people can modify it and add their own custom features.

### What platforms do the NVIDIA Texture Tools support? ###
The tools are compiled and tested regularly on Linux, OSX, and Windows.
> Some platforms are tested more frequently than others and there may be bugs on some uncommon configurations.

### Is CUDA required? ###
No. The Visual Studio solution file contains a configuration that allows you
to compile the texture tools without CUDA support. The cmake scripts automatically
detect the CUDA installation and use it only when available.

Even if the texture tools are compiled with CUDA support it's possible to use

them on systems that do not support CUDA or that do not have a valid CUDA driver

installed.


### Where can I get CUDA? ###
At http://developer.nvidia.com/object/cuda.html

### Can I use the NVIDIA Texture Tools in my commercial application? ###
Yes, the NVIDIA Texture Tools are licensed under the MIT license.

### Can I use the NVIDIA Texture Tools in my GPL application? ###
Yes, the MIT license is compatible with the GPL and LGPL licenses.

### Can I use the NVIDIA Texture Tools in the US? Do I have to obtain a license of the S3TC patent (US patent 5,956,431)? ###
NVIDIA has a license of the S3TC patent that covers all our products, including our Texture Tools. You don't have to obtain a license of the S3TC patent to use any of NVIDIA's products, but certain uses of NVIDIA Texture Tools source code cannot be considered NVIDIA products anymore. Keep in mind that the NVIDIA Texture Tools are licensed under the MIT license and thus are provided without warranty of any kind.