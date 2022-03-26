# COMO开发测试环境



### build环境

如README文档所示 https://gitee.com/tjopenlab/como

### 测试案例管理与程序调试

#### 测试用例管理工具gtest-runner

工具仓库  https://gitee.com/tjopenlab/gtest-runner

我们在原gtest-runner的基础上，做了一些工作，主要是建立测试工程时不要进图形界面。我们没找到gtest-runner是怎么管理它里面的测试案例的。

###### 提示：

从源码级安装gtest-runner，见其README文档的 Other Linux Distributions 部分，Please see the instructions on how to build gtest-runner from source.

<img src="E:\UbuntuHome\como-grammar-manual\开发文档\gtest-runner管理COMO测试案例屏幕.png" alt="gtest-runner管理COMO测试案例屏幕" style="zoom:50%;" />



#### 把测试用例加入管理工具gtest-runner

源码树中的角本add_tests_to_gtest-runner.sh是把测试案例放入gtest-runner的工具：

/como/test/scripts/add_tests_to_gtest-runner.sh



## COMO构件元数据dump工具

```shell
$ cdlc -comoMetadataReader -metadata-so sampleComoComponent.so -dump-metadata
```

- sampleComoComponent.so 为构件库（动态链接库）名字。



## comouuid工具

```shell
(base) xilong@XilongPei:~/home/como/tools$ ./comouuid --help
Ggenerate a uuid or a hash uuid-style string of a URI (example: a::b::c).
Usage: comouuid [options]
Options:
--help                  Display command line options
--cpp                   Display UUID for C++ program
-genuuid                Generate a random hash uuid-style string
-cityhash URL           Generate a hash uuid-style string of a URI (example: a::b::c)
```

