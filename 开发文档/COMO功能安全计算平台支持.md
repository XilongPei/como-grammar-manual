# COMO功能安全计算平台支持



## 把COMO类定义为功能安全类

要想在COMO编译环境中支持功能安全

```shell
$ export COMO_FUNCTION_SAFETY=enable
```



功能安全coclass具有功能安全的一些计算属性。coclass的属性定义中，FuncSafetySetting是一个字符串类型的属性，它里面是关于功能安全的一些描述信息。安全描述信息包括：

- 过期时间expire

```idl
[
    uuid(bdf665c0-8a7a-4621-ba55-e6a097dfc85d),
    version(0.1.0)
    FuncSafetySetting("1")
]
coclass CByte
{
    Constructor(
        [in] Byte value);

    interface IByte;
    interface INumber;
    interface ISerializable;
    interface IComparable;
}

```

有FuncSafetySetting定义的COMO对象，会继承ComoFunctionSafetyObject类，该类实现了接口IComoFunctionSafetyObject

```idl
namespace como {

[
    uuid(00000000-0000-0000-0000-000000000008),
    version(0.1.0)
]
interface IComoFunctionSafetyObject
{
    const Integer CFSO_ExpireVALID = 0;
    const Integer CFSO_ExpireTime = 1;

    IsValid(
        [out] Integer& isValid);

    InvalidObject(
        [in] Integer isValid);
}

}
```

### FuncSafetySetting书写规范

```
FuncSafetySetting = "timeout = 10000 ; delay = 0"
```





# 分区内存申请与创建COMO对象

下面程序示例的两种方法将使COMO对象New在分区内存中，前提是ComoContext::gComoContext要设置与分区内存相关的信息。

- InterfaceIDfromNameWithMemArea("como::demo::IBar", 分区内存序号（0开始，最大值4096）);

- InterfaceIDWithMemArea(IID_IBar, 分区内存序号（0开始，最大值4096）);

  在分区内存中New对象时，InterfaceID携带了第几个内存分区信息。

```cpp
TEST(TestIDfromName, testNewInMemArea)
{
    InterfaceID iid = InterfaceIDfromNameWithMemArea("como::demo::IBar", 1);
    AutoPtr<IBar> bar;
    ECode ec = CFooBar::New(iid, (IInterface**)&bar);
    EXPECT_EQ(ec, NOERROR);
    bar->Bar(String("testNewInMemArea: Tongji University"));
}

TEST(TestIDfromName, testNewInMemAreaWithIID)
{
    InterfaceID iid = InterfaceIDWithMemArea(IID_IBar, 1);
    AutoPtr<IBar> bar;
    ECode ec = CFooBar::New(iid, (IInterface**)&bar);
    EXPECT_EQ(ec, NOERROR);
    bar->Bar(String("testNewInMemAreaWithIID: Tongji University"));
}
```

如果有ComoContext::gComoContext->funComoMalloc设置，则由该函数负责分配COMO对象内存。

```cpp
ECode CFooBarClassObject::CreateObject(
    /* [in] */ const InterfaceID& iid,
    /* [out] */ como::IInterface** object)
{
    VALIDATE_NOT_NULL(object);

    void* addr;
//#ifdef COMO_FUNCTION_SAFETY
    if ((iid.mCid != nullptr) && ((HANDLE)iid.mCid < 4096) && (ComoContext::gComoContext != nullptr)) {
        if (ComoContext::gComoContext->funComoMalloc != nullptr) {
            addr = ComoContext::gComoContext->funComoMalloc((Short)(HANDLE)iid.mCid - 1, sizeof(CFooBar));
        }
        else {
            addr = calloc(sizeof(CFooBar), 1);
        }
    }
    else {
        addr = calloc(sizeof(CFooBar), 1);
    }
/*#else
    addr = calloc(sizeof(CFooBar), 1);
#endif
*/    if (addr == nullptr) {
        return E_OUT_OF_MEMORY_ERROR;
    }
    CFooBar* _obj = new(addr) CFooBar();
    ECode ec = _obj->Constructor();
    if (FAILED(ec)) {
        delete _obj;
        return ec;
    }
    _obj->AttachMetadata(mComponent, "como::demo::CFooBar");
    *object = _obj->Probe(iid);
    if (*object) {
        _obj->SetObjSize(sizeof(CFooBar));
#ifdef COMO_FUNCTION_SAFETY
        if ((iid.mCid != nullptr) && ((HANDLE)iid.mCid < 4096) && (ComoContext::gComoContext != nullptr)) {
            if (ComoContext::gComoContext->freeMemInArea != nullptr) {
                _obj->SetFunFreeMem(ComoContext::gComoContext->freeMemInArea, (Short)(HANDLE)iid.mCid - 1);
            }
        }
#endif
    }
#ifdef COMO_FUNCTION_SAFETY
    else {
        if ((iid.mCid != nullptr) && ((HANDLE)iid.mCid < 4096) && (ComoContext::gComoContext != nullptr)) {
            if (ComoContext::gComoContext->freeMemInArea != nullptr) {
                ComoContext::gComoContext->freeMemInArea((Short)(HANDLE)iid.mCid - 1, addr);
                return E_INTERFACE_NOT_FOUND_EXCEPTION;
            }
        }
        delete _obj;
        return E_INTERFACE_NOT_FOUND_EXCEPTION;
    }
#else
    else {
        delete _obj;
        return E_INTERFACE_NOT_FOUND_EXCEPTION; 
    }
#endif
    REFCOUNT_ADD(*object);
    return NOERROR;
}
```



# 使用taskset命令让进程运行在指定CPU上

为了让多版本应用防止单点故障，必须仔细考虑COMO Runtime在CPU不同核上的分布情况，这也就是常说的**多核编**程。多核编程和多线程有很大的不同：多线程是指每个CPU上可以运行多个线程，涉及到线程调度、锁机制以及上下文的切换；而多核则是每个CPU核一个进程（其中包含N个线程），核心之间访问数据无需上锁。为了最大限度减少线程调度的资源消耗，需要将Linux绑定在特定的核上，释放其余核心来专供应用程序使用。同时还需要考虑CPU特性和系统是否支持NUMA架构，如果支持的话，不同插槽上CPU的进程要避免访问远端内存，尽量访问本端内存。

#### 操作场景

taskset命令，可用于进程的CPU调优，可以把COMO Runtime进程，指定在某个CPU上工作。

#### 操作步骤

1、执行如下命令，查看云服务器CPU核数。

````shell
$ cat /proc/cpuinfo
````

关于CPU的核心参数说明：

- processor：指明第几个CPU处理器
- cpu cores：指明每个处理器的核心数

2、执行以下命令，获取进程状态（以下操作以进程test.sh为例，对应的pid为23989）

```shell
$ ps aux | grep test.sh
```



3、执行以下命令，查看进程当前运行在哪个CPU上。

```shell
$ taskset -p *进程号*
```

**例如：taskset -p 23989**

显示的是十进制数字1，转换为2进制为1。

4、执行以下命令，指定进程运行在第二个CPU（CPU1）上。

```shell
$ taskset -pc 1 *进程号*
```

例如：**taskset -pc 1 23989**

**说明：**

CPU的标号是从0开始的，所以CPU1表示第二个CPU（第一个CPU的标号是0），这样就把应用程序test.sh绑定到了CPU1上运行

也可以使用如下命令在启动程序时绑定CPU（启动时绑定到第二个CPU）上。

```shell
$ taskset -c 1 ./test.sh&
```





# 接口中超时设置

通过const定义常量的方法，定义超时值。接口中timeout = 整型值

timeout值的单位为纳秒ns。

```
[
    uuid(6fa00f5d-080c-422d-a1ac-4f051be6e1f7),
    version(0.1.0)
]
interface IChar
{
    const timeout = 1000;
    CompareTo(
        [in] IInterface* other,
        [out] Integer& result);

    GetValue(
        [out] Char& value);
}
```

一个方法一旦超时，它的结果将被抛弃，调用者得到的ECode值是：FUNCTION_SAFETY_CALL_TIMEOUT



# ComoConfig中与功通安全计算相关的



分区内存配置

```c++
// FSCP: Function Safety Computing Platform
typedef struct tagFSCP_MEM_AREA_INFO {
    size_t mem_size;        // Size of space to be managed
    void  *base;            // Base address of space to be managed
    size_t allocated;       // Allocated size
} FSCP_MEM_AREA_INFO;

class COM_PUBLIC ComoConfig
{
    static FSCP_MEM_AREA_INFO FscpMemAreaInfo[];
    static int sizeofFscpMemAreaInfo;
};
```



# 异构计算支持

每种体系结构的计算机，自己定义自己的Invoke函数。

```cpp
// CPU, DSA(Domain Specific Architecture)
using CpuInvokeDsa = ECode(*)(
    /* [in] */ HANDLE func,
    /* [in] */ Byte* params,
    /* [in] */ Integer paramNum,
    /* [in] */ Integer stackParamNum,
    /* [in] */ struct ParameterInfo* paramInfos);
    
static CpuInvokeDsa cpuInvokeDsa[MAX_DSA_IN_ONE_SYSTEM];
```



# 带安全检查的Setter/Getter函数

FSOGetterSetter.h

```cpp
// 函数定义
FSOSetter(fieldName,args...)
FSOGetter(fieldName,args...) 
// FSOSetter或FSOGetter实际上是定义了两个函数：get##n 和 _get_##n （set##n 和 _set_##n）， 函数_get_##n（_set_##n）是外壳，它具有检查数据合法性的功能（调用函数FSOSetterChecker或FSOGetterChecker）。

// 调用函数需要通过下面两个宏定义
CallFSOSetter(fieldName,args...)
CallFSOGetter(fieldName,args...)
```

##### FSOSetterChecker、FSOGetterChecker的程序可能会根据应用场景的不同而有差异，COMO自有的实现只是一种原理性的功能安全策略。



#### 程序原理

```cpp
void FSOGetter(PropertyName, const char *s)
{
    puts(s);
}

上面的函数定义，实际上是被定义了两个函数，等价于

void getPropertyName(const char *s)
{
	// _get_PropertyName function begin
    puts(s);
	// _get_PropertyName function end
    
    /* If we do a check before CALL_FUNCTION, we have to keep all the registers \
       and restore after the check */                                           \
    FSOGetterChecker();    
}
```

- ##### 因为`FSOGetter`是个宏，不是真正的函数名，所以调用这个函数时，需要 `CallFSOGetter(fieldName,args...)` 这样的写法。

- ##### C++宏构造起来的程序逻辑，`_get_##n这种方式构造出的函数名，无法有类名，所现在版本的Setter/Getter函数是C的，如果需要调用类的方法，需要传类对象指针的方式实现对C++程序资源的调用。



## 具有校验能力的智能指针

AutoPtr中的mPtr，在Linux 64位系统（x64、aarch64）上，高2个字节总是0，利用这2个字节，VerifiedU64Pointer算法。实现对指针的1 bit出错修复，多bit出错检测的功能。
