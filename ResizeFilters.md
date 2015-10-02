## Introduction ##

This page demonstrates the results of resampling different images using various resize filters.

Images used for the test:

| ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test1.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test1.png) | ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test3.jpg](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test3.jpg) | ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test5.jpg](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test5.jpg) |
|:------------------------------------------------------------------------------------------------------------------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------|

The width of the filters are:

| **Box** | **Triangle** | **Quadratic** | **Lanczos** | **Kaiser** |
|:--------|:-------------|:--------------|:------------|:-----------|
| 0.5     | 1.0          | 1.5           | 3.0         | 5.0        |

The parameters used for the Kaiser filter are:

```
alpha = 4
stretch = 1
```

## Test 1 ##

Downsampling without gamma correction.

### 0.7 scale factor ###

| **Box** | **Triangle** | **Quadratic** | **Lanczos** | **Kaiser** |
|:--------|:-------------|:--------------|:------------|:-----------|
| ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test1_07_box.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test1_07_box.png) | ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test1_07_triangle.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test1_07_triangle.png) | ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test1_07_quadratic.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test1_07_quadratic.png) | ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test1_07_lanczos.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test1_07_lanczos.png) | ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test1_07_kaiser.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test1_07_kaiser.png) |


### 0.5 scale factor ###

| **Box** | **Triangle** | **Quadratic** | **Lanczos** | **Kaiser** |
|:--------|:-------------|:--------------|:------------|:-----------|
| ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test1_05_box.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test1_05_box.png) | ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test1_05_triangle.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test1_05_triangle.png) | ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test1_05_quadratic.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test1_05_quadratic.png) | ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test1_05_lanczos.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test1_05_lanczos.png) | ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test1_05_kaiser.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test1_05_kaiser.png) |


### 0.3 scale factor ###

| **Box** | **Triangle** | **Quadratic** | **Lanczos** | **Kaiser** |
|:--------|:-------------|:--------------|:------------|:-----------|
| ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test1_03_box.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test1_03_box.png) | ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test1_03_triangle.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test1_03_triangle.png) | ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test1_03_quadratic.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test1_03_quadratic.png) | ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test1_03_lanczos.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test1_03_lanczos.png) | ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test1_03_kaiser.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test1_03_kaiser.png) |


## Test 2 ##

Downsampling with gamma correction. Gamma is set to 2.2.

### 0.7 scale factor ###

| **Box** | **Triangle** | **Quadratic** | **Lanczos** | **Kaiser** |
|:--------|:-------------|:--------------|:------------|:-----------|
| ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test2_07_box.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test2_07_box.png) | ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test2_07_triangle.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test2_07_triangle.png) | ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test2_07_quadratic.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test2_07_quadratic.png) | ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test2_07_lanczos.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test2_07_lanczos.png) | ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test2_07_kaiser.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test2_07_kaiser.png) |


### 0.5 scale factor ###

| **Box** | **Triangle** | **Quadratic** | **Lanczos** | **Kaiser** |
|:--------|:-------------|:--------------|:------------|:-----------|
| ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test2_05_box.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test2_05_box.png) | ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test2_05_triangle.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test2_05_triangle.png) | ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test2_05_quadratic.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test2_05_quadratic.png) | ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test2_05_lanczos.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test2_05_lanczos.png) | ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test2_05_kaiser.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test2_05_kaiser.png) |


### 0.3 scale factor ###

| **Box** | **Triangle** | **Quadratic** | **Lanczos** | **Kaiser** |
|:--------|:-------------|:--------------|:------------|:-----------|
| ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test2_03_box.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test2_03_box.png) | ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test2_03_triangle.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test2_03_triangle.png) | ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test2_03_quadratic.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test2_03_quadratic.png) | ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test2_03_lanczos.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test2_03_lanczos.png) | ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test2_03_kaiser.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test2_03_kaiser.png) |


## Test 3 ##

Downsampling without gamma correction.

### 0.7 scale factor ###

| **Box** | **Triangle** | **Quadratic** | **Lanczos** | **Kaiser** |
|:--------|:-------------|:--------------|:------------|:-----------|
| ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test3_07_box.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test3_07_box.png) | ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test3_07_triangle.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test3_07_triangle.png) | ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test3_07_quadratic.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test3_07_quadratic.png) | ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test3_07_lanczos.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test3_07_lanczos.png) | ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test3_07_kaiser.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test3_07_kaiser.png) |


### 0.5 scale factor ###

| **Box** | **Triangle** | **Quadratic** | **Lanczos** | **Kaiser** |
|:--------|:-------------|:--------------|:------------|:-----------|
| ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test3_05_box.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test3_05_box.png) | ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test3_05_triangle.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test3_05_triangle.png) | ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test3_05_quadratic.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test3_05_quadratic.png) | ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test3_05_lanczos.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test3_05_lanczos.png) | ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test3_05_kaiser.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test3_05_kaiser.png) |


### 0.3 scale factor ###

| **Box** | **Triangle** | **Quadratic** | **Lanczos** | **Kaiser** |
|:--------|:-------------|:--------------|:------------|:-----------|
| ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test3_03_box.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test3_03_box.png) | ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test3_03_triangle.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test3_03_triangle.png) | ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test3_03_quadratic.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test3_03_quadratic.png) | ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test3_03_lanczos.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test3_03_lanczos.png) | ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test3_03_kaiser.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test3_03_kaiser.png) |


## Test 4 ##

Downsampling with gamma correction. Gamma is set to 2.2.

### 0.7 scale factor ###

| **Box** | **Triangle** | **Quadratic** | **Lanczos** | **Kaiser** |
|:--------|:-------------|:--------------|:------------|:-----------|
| ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test4_07_box.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test4_07_box.png) | ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test4_07_triangle.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test4_07_triangle.png) | ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test4_07_quadratic.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test4_07_quadratic.png) | ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test4_07_lanczos.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test4_07_lanczos.png) | ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test4_07_kaiser.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test4_07_kaiser.png) |


### 0.5 scale factor ###

| **Box** | **Triangle** | **Quadratic** | **Lanczos** | **Kaiser** |
|:--------|:-------------|:--------------|:------------|:-----------|
| ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test4_05_box.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test4_05_box.png) | ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test4_05_triangle.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test4_05_triangle.png) | ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test4_05_quadratic.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test4_05_quadratic.png) | ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test4_05_lanczos.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test4_05_lanczos.png) | ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test4_05_kaiser.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test4_05_kaiser.png) |


### 0.3 scale factor ###

| **Box** | **Triangle** | **Quadratic** | **Lanczos** | **Kaiser** |
|:--------|:-------------|:--------------|:------------|:-----------|
| ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test4_03_box.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test4_03_box.png) | ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test4_03_triangle.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test4_03_triangle.png) | ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test4_03_quadratic.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test4_03_quadratic.png) | ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test4_03_lanczos.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test4_03_lanczos.png) | ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test4_03_kaiser.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test4_03_kaiser.png) |



## Test 5 ##

Downsampling with gamma correction. Gamma is set to 2.2.

### 0.7 scale factor ###

| **Box** | **Triangle** | **Quadratic** | **Lanczos** | **Kaiser** |
|:--------|:-------------|:--------------|:------------|:-----------|
| ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test5_07_box.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test5_07_box.png) | ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test5_07_triangle.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test5_07_triangle.png) | ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test5_07_quadratic.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test5_07_quadratic.png) | ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test5_07_lanczos.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test5_07_lanczos.png) | ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test5_07_kaiser.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test5_07_kaiser.png) |


### 0.5 scale factor ###

| **Box** | **Triangle** | **Quadratic** | **Lanczos** | **Kaiser** |
|:--------|:-------------|:--------------|:------------|:-----------|
| ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test5_05_box.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test5_05_box.png) | ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test5_05_triangle.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test5_05_triangle.png) | ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test5_05_quadratic.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test5_05_quadratic.png) | ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test5_05_lanczos.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test5_05_lanczos.png) | ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test5_05_kaiser.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test5_05_kaiser.png) |


### 0.3 scale factor ###

| **Box** | **Triangle** | **Quadratic** | **Lanczos** | **Kaiser** |
|:--------|:-------------|:--------------|:------------|:-----------|
| ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test5_03_box.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test5_03_box.png) | ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test5_03_triangle.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test5_03_triangle.png) | ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test5_03_quadratic.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test5_03_quadratic.png) | ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test5_03_lanczos.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test5_03_lanczos.png) | ![http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test5_03_kaiser.png](http://nvidia-texture-tools.googlecode.com/svn/wiki/imgs/test5_03_kaiser.png) |