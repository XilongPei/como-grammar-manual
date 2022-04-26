# COMO RPC测试用例

### 进入工作环境

```
$ source build/envsetup.sh

$ como_linux_x64

```

### 1、起动服务管理器

```
$ ./servicemanager/exe/linux/servicemanager


xilongpei@xilongpei-OptiPlex-7010:~/como/como/out/target/como.linux.x64.rls$ ./servicemanager/exe/linux/servicemanager
D/[ 2021-11-26 10:23:03.029 +0800 DEBUG ServiceManager]: starting ...
D/[ 2021-11-26 10:23:03.031 +0800 DEBUG ServiceManager]: dbus_connection_read_write_dispatch
D/[ 2021-11-26 10:23:03.031 +0800 DEBUG ServiceManager]: dbus_connection_read_write_dispatch
```

### 2、起动服务

```
$ ./test/runtime/rpc/service/testRPCSrv


xilongpei@xilongpei-OptiPlex-7010:~/como/como/out/target/como.linux.x64.rls$ ./test/runtime/rpc/service/testRPCSrv
==== rpc service wait for calling ====

```

### 3、启动客户端

```
$ ./test/runtime/rpc/client/testRPCCli


xilongpei@xilongpei-OptiPlex-7010:~/como/como/out/target/como.linux.x64.rls$ ./test/runtime/rpc/client/testRPCCli
[==========] Running 7 tests from 1 test case.
[----------] Global test environment set-up.
[----------] 7 tests from RPCTest
[ RUN      ] RPCTest.TestGetRPCService
[       OK ] RPCTest.TestGetRPCService (3 ms)
[ RUN      ] RPCTest.TestCallTestMethod1
[       OK ] RPCTest.TestCallTestMethod1 (1 ms)
[ RUN      ] RPCTest.TestCallTestMethod2
[       OK ] RPCTest.TestCallTestMethod2 (1 ms)
[ RUN      ] RPCTest.TestCallTestMethod3
[       OK ] RPCTest.TestCallTestMethod3 (1 ms)
[ RUN      ] RPCTest.TestCallTestMethod4
[       OK ] RPCTest.TestCallTestMethod4 (4 ms)
[ RUN      ] RPCTest.TestCallTestMethod5
[       OK ] RPCTest.TestCallTestMethod5 (1 ms)
[ RUN      ] RPCTest.TestIsStubAlive
[       OK ] RPCTest.TestIsStubAlive (0 ms)
[----------] 7 tests from RPCTest (11 ms total)

[----------] Global test environment tear-down
[==========] 7 tests from 1 test case ran. (11 ms total)
[  PASSED  ] 7 tests.
```



## 程序调试知识

#### C++源码中设置断点 https://github.com/scottt/debugbreak

```
__builtin_trap();
```

#### 反汇编

有些时候，你不得不读汇编代码。示例如下：

```
g++ -g cpp_test.cpp
objdump -d -S -C a.out
```

#### ldd命令

Linux 下可以使用ldd查看可执行文件所需要的动态链接库（*.so）。

```shell
[xilong-tongji@tongji-openeuler como.linux.aarch64.rls]$ ldd ./como/runtime/comort.so
        linux-vdso.so.1 (0x0000fffef3eb0000)
        libdl.so.2 => /lib64/libdl.so.2 (0x0000fffef3de0000)
        libpthread.so.0 => /lib64/libpthread.so.0 (0x0000fffef3da0000)
        libdbus-1.so.3 => /lib64/libdbus-1.so.3 (0x0000fffef3d20000)
        libstdc++.so.6 => /lib64/libstdc++.so.6 (0x0000fffef3b70000)
        libm.so.6 => /lib64/libm.so.6 (0x0000fffef3aa0000)
        libgcc_s.so.1 => /lib64/libgcc_s.so.1 (0x0000fffef3a60000)
        libc.so.6 => /lib64/libc.so.6 (0x0000fffef38e0000)
        /lib/ld-linux-aarch64.so.1 (0x0000fffef3ec0000)
        libsystemd.so.0 => /lib64/libsystemd.so.0 (0x0000fffef3810000)
        librt.so.1 => /lib64/librt.so.1 (0x0000fffef37e0000)
        liblzma.so.5 => /lib64/liblzma.so.5 (0x0000fffef3790000)
        liblz4.so.1 => /lib64/liblz4.so.1 (0x0000fffef3730000)
        libgcrypt.so.20 => /lib64/libgcrypt.so.20 (0x0000fffef3650000)
        libgpg-error.so.0 => /lib64/libgpg-error.so.0 (0x0000fffef3600000)
```



# COMO RPC Over ZeroMQ测试用例

### 编译环境

```
export RPC_OVER_ZeroMQ_SUPPORT_Enable=enable
```



### 1、起动服务管理器

```
./servicemanager/exe/linux/servicemanager
```

### 2、起动基于ZeroMQ的服务

```
./test/runtime/rpc/serviceZMQ/testRPCZmqSrv
```



### 3、启动基于ZeroMQ的客户端

```
./test/runtime/rpc/clientZMQ/testRPCZmqCli
```



