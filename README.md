# opencv_catkin

Catkin wrapper for opencv (originally by [ASL at ETH](https://github.com/ethz-asl/opencv3_catkin))

## Getting started
### Configuration
- All the desired configurations must be manually set in the [CMakeLists.txt](CMakeLists.txt) file. Please read the comments carefully depending on what you need.
- OpenCV's version is implicitly chosen via the zip url in lines 14 and 30. It must be obtained from the [release page](https://github.com/opencv/opencv/releases) in the official GitHub repo.
- The version of (`opencv_contrib`) must match `opencv`'s and, similarly, can be obtained from [here](https://github.com/opencv/opencv_contrib/releases).
- **ROS version considerations**: if you plan to use it with ROS Melodic, **you must use OpenCV 3.2**; for Noetic you need **OpenCV 4.2**. Otherwise, you'll have linking errors because of the mismatch between the system libraries and this package (and that will require that you also build your own versions of other ROS packages, e.g. `cv_bridge`, which could also fail depending on the [version of OpenCV you are trying to use](https://github.com/ros-perception/vision_opencv/issues/291)).
- Please note that **not** all the options that OpenCV offers are available in the CmakeLists.txt file in this package. Confirm with the [original OpenCV's CMakeLists](https://github.com/opencv/opencv/blob/master/CMakeLists.txt).
- If you want to use `cmake-gui` or `ccmake` to change the setup, you can open them in `~/catkin_ws/build/opencv_catkin/opencv_opencv_build`

### Compilation
#### Normal compilation
This should be straightforward on your desktop machine:
```sh
catkin build opencv_catkin
```

#### Compilation with CUDA support
In order to make the process faster and avoid compiling code for all the possible CUDA architectures, you can manually select the compute capability (`CUDA_ARCH_BIN`) of your board. Check [here](https://developer.nvidia.com/cuda-gpus) for the full list.

For example, to compile on a Xavier board (`CUDA_ARCH_BIN=7.2`) you will use:

```sh
catkin build opencv_catkin --cmake-args -DCUDA_ARCH_BIN=7.2
```

Other video cards, such as the Nvidia Quadro P200 (`CUDA_ARCH_BIN=6.1`), additionally require to use an older version of GCC. You can set it manually:

```sh
catkin build opencv_catkin --cmake-args -DCUDA_ARCH_BIN=6.1 -DCMAKE_C_COMPILER=/usr/bin/gcc-7
```

## Other notes
# Changing OpenCV's source
- If you want to change the source (to include some messages, chang some implementations, etc), the source code is available in `~/catkin_ws/build/opencv_catkin/opencv_src`
- Similarly, `opencv_contrib` modules can be found in `~/catkin_ws/build/opencv_catkin/opencv_contrib_src`

## Check that the libraries are correctly linked
Catkin will automatically find OpenCV (as usual, with CMake), in the devel folder. You only have to compile this package first, and then the package that depends on OpenCV. If you have already built your package, you must clean it up and build it again, otherwise your package will stay linked to the system libraries.

If you want to confirm that you are linking your package to the right libraries:
-  Go to `~/catkin_ws/devel/lib`. 
-  You should find the OpenCV binary libraries `libopencv_*.so`
-  You should also find your package. For example, `libmypackage.so`
-  To check which libraries `mypackage` is linked to, use `ldd libmypackage.so`. They should point to the OpenCV's binaries found before.
