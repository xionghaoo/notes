## 编译运行

编译并链接生成可执行文件

```shell
g++ ./hello.c
```

可编译多个文件，上述命令生成`a.out`文件，可直接运行。

第一个文件需要有main方法，不然会报错。

## ARM64架构编译

```shell
sudo apt-get install gcc-9-aarch64-linux-gnu
aarch64-linux-gnu-gcc-9 -shared -o libapple.so -fPIC apple.c
```

用指定平台的编译工具编译

## 正常编译so

```shell
gcc -shared -o libapple.so -fPIC apple.c
```

根据执行平台的不同，会编译出不同cpu架构的so产物

## 其他命令

+ 查看so文件格式，可显示abi信息

  ```shell
  file libapple.so
  ```

  