# opencv_catkin

Catkin wrapper for opencv (originally by [ASL at ETH](https://github.com/ethz-asl/opencv3_catkin))

## Configuration of the package
- All the desired configuration must be manually set in the [CMakeLists.txt](CMakeLists.txt) file. Please read carefully the comments depending on what you need.
- `opencv` version is implicitley chosen via the zip url in line 14. It must be obtained from the [release page](https://github.com/opencv/opencv/releases) in the official GitHub repo.
- The version of (`opencv_contrib`) must match `opencv`'s and, similarly, can be obtained from [here](https://github.com/opencv/opencv_contrib/releases).
- However, if you plan to use it with ROS Melodic, **you must use OpenCV 3.2**; for Noetic you need **OpenCV 4.2**. Otherwise you'll have linking errors because of the mismatch between the system libraries and this package (and that will require that you also build your own versions of other ROS packages, e.g. `cv_bridge`, which could also fail depending on the [version of OpenCV you are trying to use](https://github.com/ros-perception/vision_opencv/issues/291)).
- Please note that **not** all the options that OpenCV has are available in the CmakeLists.txt file in this package. Confirm with the [original OpenCV's CMakeLists](https://github.com/opencv/opencv/blob/master/CMakeLists.txt).

## Configuration of other packages that rely on this one
- Catkin will automatically find OpenCV (as usual, with CMake), in the devel folder. You only have to compile this package first, and then the package that depends on OpenCV. If you have already built your package, you must clean it up and build again, otherwise your package will stay linked to the system libraries.
- If you want to confirm that you are linking your package to the right libraries:
  -  Go to `~/catkin_ws/devel/lib`. 
  -  You should find the OpenCV binary libraries `libopencv_*.so`
  -  You should also find yor package. For example, `libmypackage.so`
  -  To check which libraries `mypackage` is linked to, use `ldd libmypackage.so`

## Changing OpenCV's source
- If you want to change the source (to include some messages, changing some implementations, etc), the source code is available in `~/catkin_ws/build/opencv_catkin/opencv_src`
- Similarly, `opencv_contrib` modules can be found in `~/catkin_ws/build/opencv_catkin/opencv_contrib_src`

## Building on Jetson platforms
In order to make the process faster and avoidcompiling code for all the possible CUDA architectures, select 7.2 (for Xavier board). For other architectures and corresponding number, check [here](https://developer.nvidia.com/cuda-gpus).

```sh
catkin build opencv_catkin --cmake-args -DCUDA_ARCH_BIN=7.2
```

If you get errors due to the GCC version and CUDA architecture, add the compiler manually:

```sh
catkin build opencv_catkin --cmake-args -DCUDA_ARCH_BIN=7.2 -DCMAKE_C_COMPILER=/usr/bin/gcc-7
```