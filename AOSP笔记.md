#### 编译单独模块

```shell
xhmbp: ~/aosp$ mmm ./packages/experimental/HelloAndroid/
```

输出目录：`out/target/product/angler/system/app`

`framework/base`编译

```shell
mma ./framework/base
```

`framework/base/packages/SystemUI/`编译

```shell
mmma ./framework/base/packages/SystemUI/
```





#### 重新打包Android系统镜像

```shell
xhmbp: ~/aosp$ make snod
```

输出目录：`out/target/product/angler`





