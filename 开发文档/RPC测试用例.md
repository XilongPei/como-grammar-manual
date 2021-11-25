# COMO RPC测试用例

### 进入工作环境

```
$ source build/envsetup.sh

$ como_linux_x64

```

### 1、起动服务管理器

```
$ ./servicemanager/exe/linux/servicemanager
```

### 2、起动服务

```
$ ./test/runtime/rpc/service/testRPCSrv
```

### 3、启动客户端

```
$ ./test/runtime/rpc/client/testRPCCli

```



## 程序调试知识

C++源码中设置断点 https://github.com/scottt/debugbreak

```
__builtin_trap();
```
