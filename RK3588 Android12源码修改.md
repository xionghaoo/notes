###  禁用USB授权弹窗

所在文件`frameworks/base/core/res/res/values/config.xml`

修改内容

```xml
<bool name="config_disableUsbPermissionDialogs">true</bool>
```

### 修改USB摄像头角度

所在文件`device/rockchip/common/external_camera_config.xml`

修改内容：

```xml
<Orientation  degree="0"/>
```

### 修改系统显示dpi

所在文件`device/rockchip/rk3588/rk3588_s/rk3588_s.mk`

修改内容

```mk
PRODUCT_PROPERTY_OVERRIDES += ro.sf.lcd_density=240
```

### 修改设备文件权限

所在文件`system/core/rootdir/ueventd.rc`

修改内容

```shell
/dev/uinput    0777   uhid   uhid
```

### 系统预装App

系统签名预置路径 `vendor/rockchip/common/apps/`

最后需要将app添加到PRODUCT_PACKAGES里面：

修改文件: `device/rockchip/common/device.mk`

```mk
PRODUCT_PACKAGES += \
    TouchService
```

编译后的app会添加到`out/target/product/rk3588_s/system/app/`这个路径下

#### 1. TouchService App

+ `Android.mk`: 路径`vendor/rockchip/common/apps/TouchService/Android.mk`

  ```mk
  ###############################################################################
  # TouchService
  LOCAL_PATH := $(call my-dir)
  include $(CLEAR_VARS)
  LOCAL_MODULE := TouchService
  LOCAL_MODULE_CLASS := APPS
  LOCAL_MODULE_TAGS := optional
  # LOCAL_BUILT_MODULE_STEM := package.apk
  LOCAL_MODULE_SUFFIX := $(COMMON_ANDROID_PACKAGE_SUFFIX)
  #LOCAL_PRIVILEGED_MODULE :=
  LOCAL_CERTIFICATE := PRESIGNED
  #LOCAL_OVERRIDES_PACKAGES := 
  LOCAL_SRC_FILES := $(LOCAL_MODULE).apk
  LOCAL_ENFORCE_USES_LIBRARIES := false
  LOCAL_PREBUILT_JNI_LIBS := \
  	@lib/arm64-v8a/libc++_shared.so \
  	@lib/arm64-v8a/libimage_processing_util_jni.so \
  	@lib/arm64-v8a/libjpeg-turbo1500.so \
  	@lib/arm64-v8a/libopencv_java3.so \
  	@lib/arm64-v8a/librobocontroller.so \
  	@lib/arm64-v8a/libTouchScreenCore.so \
  	@lib/arm64-v8a/libusb100.so \
  	@lib/arm64-v8a/libuvc.so \
  	@lib/arm64-v8a/libUVCCamera.so 
  include $(BUILD_PREBUILT)
  ```

+ `TouchService.apk`: 路径`vendor/rockchip/common/apps/TouchService/TouchService.apk`

+ so文件: 路径`vendor/rockchip/common/apps/TouchService/lib/arm64-v8a/*.so`

### RK预装APP

apk放置路径: 

+ `device/rockchip/rk3588/rk3588_s/preinstall`: 应用不能删除
+ `device/rockchip/rk3588/rk3588_s/preinstall_del`: 应用能删除，但是系统重启后会恢复
+ `device/rockchip/rk3588/rk3588_s/preinstall_del_forever`: 应用能永久删除

直接将Apk放到对应文件夹，如果没有就自己创建，编译时会自动生成所需文件，生成的app放置在:

+ `out/target/product/rk3588_s/odm/bundled_persist-app`
+ `out/target/product/rk3588_s/odm/bundled_uninstall_back-app`

### 系统Webview替换

[更新Android系统自带的Webview](http://www.vaststargames.com/read.php?tid=26&fid=13&page=1#172)

1. 下载最新的webview apk包，替换`external/chromium-webview/prebuilt`下各个架构的`webview.apk`

2.  修改文件`frameworks/base/core/res/res/values/config.xml`，增加下面的内容

   ```xml
   <resources>
    ...
       <string name="config_webViewPackageName" translatable="false">com.google.android.webview</string>
   </resources>
   ```

3. 修改文件`frameworks/base/core/res/res/xml/config_webview_packages.xml`

   ```xml
   <webviewproviders>
       <!-- The default WebView implementation -->
       <webviewprovider description="Android WebView" packageName="com.google.android.webview" availableByDefault="true">
       </webviewprovider>
   </webviewproviders>
   ```

### 系统不锁屏不休眠

[AOSP-不锁屏不休眠](https://blog.csdn.net/qq23001186/article/details/122416169)

#### 不锁屏

修改文件：`frameworks/base/packages/SettingsProvider/res/values/defaults.xml`

```xml
<bool name="def_lockscreen_disabled">true</bool>
```

#### 不休眠

(无效)修改文件：`frameworks/base/packages/SettingsProvider/res/values/defaults.xml`

```xml
<integer name="def_screen_off_timeout">0</integer>
```

60000改为0，0默认是永久不锁屏，这个配置在`packages/apps/Settings/src/com/android/settings/display/ScreenTimeoutPreferenceController.java`中有用到。

重新编译：

```shell
mmma ./framework/base/packages/SettingsProvider/
```

输出位置：`out/target/product/rk3588_s/system/priv-app/SettingsProvider/SettingsProvider.apk`

修改文件：`device/rockchip/rk3588/overlay/framworks/base/packages/SettingsProvider/res/values/default.xml`

```xml
 <integer name="def_screen_off_timeout">0</integer>
```

6000改为0

### 移除系统导航栏

(无效)修改文件：`frameworks/base/packages/SystemUI/src/com/android/systemui/navigationbar/NavigationBarController.java`

```java
void createNavigationBar(Display display, Bundle savedState, RegisterStatusBarResult result) {
    // 注释掉这句话
  	// mNavigationBars.put(displayId, navBar);
}
```

重新编译：

```shell
mmma ./frameworks/base/packages/SystemUI/
```

输出位置：`out/target/product/rk3588_s/system_ext/priv-app/SystemUI/SystemUI.apk`

true改为false

修改文件：`device/rockchip/common/overlay/framworks/base/core/res/res/values/config.xml`

```xml
<bool name="config_showNavigationBar">false</bool>

<bool name="config_swipe_up_gesture_setting_available">false</bool>
```

true改为false

修改文件：`frameworks/base/packages/SystemUI/src/com/android/statusbar/phone/StatusBar.java`

```java
protected void makeStatusBarView(@Nullable RegisterStatusBarResult result) { 
    Log.i(TAG, "xionghaoo remove navigationbar")
    // createNavigationBar(result);
}
```



### 修改导航栏的位置



com.android.internal.systemui.navbar.threebutton

