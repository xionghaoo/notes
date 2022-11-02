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

所在文件`device/rockchip/common/display_settings.xml`

修改内容

```xml
<display
  name="local:1"
  shouldShowSystemDecors="false"
  shouldShowIme="true"
  forcedDensity="160"	/>
```







