## Contents ##

  * [Prerequisites](CompilationInstructions#Prerequisites.md)
  * [Windows](CompilationInstructions#Windows.md)
    * [Visual Studio 8](CompilationInstructions#Visual_Studio_8.md)
    * [CMake and Visual Studio 7](CompilationInstructions#CMake_and_Visual_Studio_7.md)
    * [CMake and Visual Studio 8](CompilationInstructions#CMake_and_Visual_Studio_8.md)
    * [CMake and Visual Studio 10](CompilationInstructions#CMake_and_Visual_Studio_10.md)
  * [Linux](CompilationInstructions#Linux.md)
  * [Mac OSX](CompilationInstructions#Mac_OSX.md)


## Prerequisites ##

Project files are provided for Visual Studio 8. For all other platforms and targets you will need to install CMake. You can download it from:

http://www.cmake.org/cmake/resources/software.html

In order to compile the CUDA accelerated compressors you need to install CUDA:

http://www.nvidia.com/object/cuda_get.html

Note that CUDA is not free software and is not supported on all platforms.

In addition to that, the command line tools also need the following libraries:

  * libpng
  * libjpeg
  * ...

Note that the sources already include these libraries precompiled for 32 bit windows.


## Windows ##

### Visual Studio 8 ###

Simply open the solution file:

```
projects/vc8/nvtt.sln
```

and build all (Ctrl + Shift + B).


### CMake and Visual Studio 7 ###

Run the following commands:

```
$ mkdir vc7
$ cd vc7
$ cmake .. -DNVTT_SHARED=1 -G "Visual Studio 7 2003 .NET"
```

open the generated solution file:

```
vc7/NV.sln
```

and build all (Ctrl + Shift + B).


### CMake and Visual Studio 8 ###

Run the following commands:

```
$ mkdir vc8
$ cd vc8
$ cmake .. -DNVTT_SHARED=1 -G "Visual Studio 8 2005"
```

open the generated solution file:

```
vc8/NV.sln
```

and build all (Ctrl + Shift + B).


### CMake and Visual Studio 10 ###

Run the following commands:

```
$ mkdir vc10
$ cd vc10
$ cmake .. -DNVTT_SHARED=1 -G "Visual Studio 10"
```

open the generated solution file:

```
vc10/NV.sln
```

and build all (Ctrl + Shift + B).


### CMake + MinGW ###

Note that CUDA does not currently support the MinGW compiler. This target is not supported yet, but may work if CUDA is not enabled.


## Linux ##

If you are running Debian or Ubuntu you can install the required libraries as follows:

```
sudo apt-get install ...
```

On other distributions you will have to use the corresponding package manager.

Then compile and install the sources as follows:

```
$ ./configure
$ make
$ sudo make install
```


## Mac OSX ##

Install the required libraries as follows:

```
$ sudo port install ...
```

Then compile and install the sources as follows:

```
$ ./configure
$ make
$ sudo make install
```