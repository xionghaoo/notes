## OpenCV Android SDK集成

添加libopencv_java4.so时需要加上

```groovy
 externalNativeBuild {
     cmake {
         arguments "-DANDROID_STL=c++_shared"
     }
 }
```

cmake添加项

```cmake
# 设置opencv根目录
set(OPENCV_DIR ${CMAKE_SOURCE_DIR}/../../../opencv)
# 指定opencv头文件路径
include_directories(${OPENCV_DIR}/include)
# 添加opencv库
add_library(opencv_java4 SHARED IMPORTED)
set_target_properties(
        opencv_java4
        PROPERTIES
        IMPORTED_LOCATION
        "${OPENCV_DIR}/libs/${ANDROID_ABI}/libopencv_java4.so"
)
# 链接库，注意库名称必须是opencv_java4
target_link_libraries(
        # Specifies the target library.
        ${libname}
       	...
        opencv_java4
)
```

## OpenCV_Contrib编译Android产物

### Linux下编译

```shell
# Install minimal prerequisites (Ubuntu 18.04 as reference)
sudo apt update && sudo apt install -y cmake g++ wget unzip
# Download and unpack sources
wget -O opencv.zip https://github.com/opencv/opencv/archive/4.x.zip
wget -O opencv_contrib.zip https://github.com/opencv/opencv_contrib/archive/4.x.zip
unzip opencv.zip
unzip opencv_contrib.zip
# Create build directory and switch into it
mkdir -p build && cd build
# Configure
cmake -DOPENCV_EXTRA_MODULES_PATH=../opencv_contrib-4.x/modules ../opencv-4.x
# Build
cmake --build .
```

### Android平台编译脚本

NDK是必要依赖

```shell
#!bin/bash

rm -rf build
mkdir build && cd build

export ANDROID_NDK=/home/rabbit/opencv/android-ndk-r21e
export ANDROID_SDK=/home/rabbit/opencv/sdk/platform-tools
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64

cmake -DCMAKE_TOOLCHAIN_FILE=$ANDROID_NDK/build/cmake/android.toolchain.cmake \
-DCMAKE_ANDROID_NDK=$ANDROID_NDK \
-DANDROID_SDK=$ANDROID_SDK \
-DANDROID_NATIVE_API_LEVEL=28 \
-DBUILD_ANDROID_PROJECTS=OFF \
-DBUILD_ANDROID_EXAMPLES=OFF \
-DCMAKE_BUILD_TYPE=Release  \
-DBUILD_JAVA=ON  \
-DBUILD_ZLIB=ON \
-DANDROID_ABI=arm64-v8a \
-DBUILD_FAT_JAVA_LIB=ON \
-DBUILD_opencv_java=ON \
-DBUILD_opencv_videoio=OFF \
-DBUILD_opencv_video=OFF \
-DBUILD_opencv_gapi=OFF \
-DCMAKE_BUILD_WITH_INSTALL_RPATH=ON \
-DANDROID_STL=c++_shared \
-DBUILD_SHARED_LIBS=OFF \
-DCMAKE_INSTALL_PREFIX=/home/rabbit/opencv/ \
-DANT_EXECUTABLE="/usr/bin" \
-DOPENCV_EXTRA_MODULES_PATH=../opencv_contrib-4.x/modules ../opencv-4.x

make -j$(nproc)

make install
```

`libopencv_java4.so`生成的必要条件：`BUILD_FAT_JAVA_LIB=ON`，`ANT_EXECUTABLE="/usr/bin"`，`BUILD_JAVA=ON`

输出路径：`<当前脚本路径>/sdk/native/libs/arm64-v8a/libopencv_java4.so`

### 编译错误

1. `./gradlew`不存在，注释掉`opencv/opencv-4.x/modules/java/android_sdk/CMakeLists.txt`里面的这行代码，不影响编译结果

   ```cmake
   add_custom_command(
       OUTPUT "${AAR_FILE}" "${OPENCV_DEPHELPER}/${the_module}_android"
       #COMMAND ./gradlew ${OPENCV_GRADLE_VERBOSE_OPTIONS} "opencv:assemble"
       COMMAND ${CMAKE_COMMAND} -E touch "${OPENCV_DEPHELPER}/${the_module}_android"
       WORKING_DIRECTORY "${ANDROID_BUILD_BASE_DIR}"
       DEPENDS ${depends} ${the_module}
       COMMENT "Building OpenCV Android library project"
   )
   ```

   

## 常见异常

#### 1. 报错`fread_unlocked android`

minSDK版本低于28，调到28以上就行了，或者在28以上的设备上运行

#### 2. OpenCV3 某些方法链接报错

OpenCV3使用*gnustl*版本的C++标准库，和现在新版的c++_shared库不兼容，升级到opencv4就行了









