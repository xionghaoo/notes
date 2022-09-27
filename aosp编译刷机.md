## 源码准备

[代号、标记和 build 号](https://source.android.com/docs/setup/start/build-numbers?hl=zh-cn)

[Factory Images](https://developers.google.com/android/images#angler)

源码编译之后可以再模拟器上刷机，要想刷实体机，还缺少vendor.img文件，可以去google官方的factory images里面去找。

刷入厂商驱动镜像

```shell
$ fastboot flash vendor vendor.img
```

刷入我们编译的镜像

```shell
$ fastboot flash boot boot.img
$ fastboot flash recovery recovery.img
$ fastboot flash system system.img
$ fastboot flash userdata userdata.img
$ fastboot flash cache cache.img
```

刷完重启

```shell
$ fastboot reboot
```



## 参考文章

[android源码编译并刷入nexus 6p手机](https://blog.csdn.net/chen1234219/article/details/90342071)