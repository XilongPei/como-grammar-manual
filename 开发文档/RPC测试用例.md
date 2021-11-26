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

C++源码中设置断点 https://github.com/scottt/debugbreak

```
__builtin_trap();
```
