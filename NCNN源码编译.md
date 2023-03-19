# NCNN源码编译

 `ndk 21.4.7075529` 环境 https://dl.google.com/android/repository/android-ndk-r21e-darwin-x86_64.zip

官方编译文档 https://github.com/Tencent/ncnn/wiki/how-to-build#build-for-android

编译脚本：

```shell
#!/bin/bash

export ANDROID_NDK=/Users/destiny/Library/Android/sdk/ndk/21.4.7075529

cmake -DCMAKE_TOOLCHAIN_FILE="$ANDROID_NDK/build/cmake/android.toolchain.cmake"\
    -DANDROID_ABI="arm64-v8a" \
    -D NCNN_SHARED_LIB=ON \
    -D NCNN_OPENMP=OFF \
	-DANDROID_PLATFORM=android-28 -DNCNN_VULKAN=ON ..

make -j$(nproc)
make install
```

Openmp 和 paddle的库有冲突，需要去掉。

