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

## 常见异常

#### 1. 报错`fread_unlocked android`

minSDK版本低于28，调到28以上就行了，或者在28以上的设备上运行

#### 2. OpenCV3 某些方法链接报错

OpenCV3使用*gnustl*版本的C++标准库，和现在新版的c++_shared库不兼容，升级到opencv4就行了









