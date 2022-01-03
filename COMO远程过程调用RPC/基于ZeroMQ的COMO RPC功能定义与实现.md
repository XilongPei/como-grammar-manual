# 基于ZeroMQ的COMO RPC功能定义与实现



ZeroMQ是一个轻量级的消息库。它扩展了标准的套接字接口，其特性与传统的消息中间件不同，ZeroMQ提供异步消息队列、多消息传递模式、消息过滤（订阅）、无缝访问多个传输协议等的抽象。

##### 线程安全

ZeroMQ Context（上下文）是线程安全的，可以根据需要在尽可能多的应用程序线程之间共享，而不需要调用者需要额外的锁定。多个上下文可能共存于一个应用程序中。 

　　除了在将套接字从一个线程迁移到另一个线程时发出完全内存障碍的情况之外，单个ZeroMQ 套接字不是线程安全的。

##### 消息

ZeroMQ 消息是在同一应用程序的应用程序或组件之间传递的一个独立的数据单元。ØMQ消息没有内部结构，从ØMQ本身来看，它们被认为是不透明的二进制数据。



## 基于ZeroMQ的COMO RPC 

如果是本地注册服务，调用的是AddService，如果是远地注册服务，调用的是AddRemoteService。

AddRemoteService时，增加了一个参数，IP地址:端口，把本地的一个服务注册到其它机器的COMO运行时（COMORuntime）上，然后再它调用方式都不变，就象是本机RPC一样。

```cpp
ECode ServiceManager::AddRemoteService(
    /* [in] */ const String& thisServerName,
    /* [in] */ const String& thatServerName,
    /* [in] */ const String& name,
    /* [in] */ IInterface* object);
```

ServiceManager::GetService取得服务时，有这样的代码：

```cpp
            String str = nullptr;
            ec = ipack->GetServerName(str);
            if ((nullptr == str) || str.IsEmpty()) {
                ec = CoUnmarshalInterface(ipack, RPCType::Local, object);
            }
            else {
                ec = CoUnmarshalInterface(ipack, RPCType::Remote, object);
            }
```

根据服务提供者的ServerName是否设，不为空，则是远程服务，此时client得到的接口指向的是远程。



### 辅助函数

```cpp
#include "ComoConfig.h"
std::string ComoConfig::AddZeroMQEndpoint(std::string serverName, std::string endpoint)

// call it like this     
ComoConfig::AddZeroMQEndpoint(std::string("comotest"), std::string("tcp://192.168.1.18:1239"));
```



### 概念

- ZMQ端点（endpoint）是一个由**transport://**后跟一个地址组成的字符串。 transport指定要使用的基础协议。该地址指定要连接的传输特定地址。
