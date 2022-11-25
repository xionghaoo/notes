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

### 预置App

系统签名预置路径 `vendor/rockchip/common/apps/`

### 系统Webview替换

[更新Android系统自带的Webview](http://www.vaststargames.com/read.php?tid=26&fid=13&page=1#172)



### 系统不锁屏不休眠

[AOSP-不锁屏不休眠](https://blog.csdn.net/qq23001186/article/details/122416169)





