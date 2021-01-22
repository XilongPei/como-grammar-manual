IRuntime接口介绍



Java技术体系中，Runtime类代表着Java程序的运行时环境，每个Java程序都有一个Runtime实例，该类会被自动创建，我们可以通过`Runtime.getRuntime()` 方法来获取当前程序的Runtime实例。

##### IRuntime.cdl

```
namespace como {
namespace core {

interface IRuntime
{
    AddShutdownHook(
        [in] IThread* hook);

    AvailableProcessors(
        [out] Integer& ncpu);

    Exit(
        [in] Integer status);
}

}
}
```



$como/system/entrypoint/entry.cpp