# COMO运行时(runtime)基础(base)



COMO构件由其所定义的接口、构件类及相关元数据组成。

COMO以构件模块为封装和存储单位，并通过动态链接库实现(每个COMO构件模块都是一个动态链接库)构件模块由uunm（Universal Unique Name）唯一标识，在本机则以存储的文件名为标识。



COMO中的Class的Load机制和Java虚拟机JVM ClassLoad类似，也有CLASSPATH（环境变量：PATH），所有PATH中的构件将被索引，从而可以通过名字找到它。通过ClassLoader装载元数据，需要得到一个对象实例（instance）时，找到类厂（class factory），再由类厂去创建ClassObject对象，通过`IClassObject`接口操作ClassObject对象。`como::IClassObject->CreateObject()`
```c++
ECode CoGetComponentMetadata(
    /* [in] */ const ComponentID& cid,		//也可以是：const String& path, 
    /* [in] */ IClassLoader* loader,
    /* [out] */ AutoPtr<IMetaComponent>& mc)
{
    if (loader == nullptr) {
        loader = CBootClassLoader::GetSystemClassLoader();
    }

    return loader->LoadComponent(cid, mc);

}

AutoPtr<IMetaComponent> mc;
CoGetComponentMetadata(CID_FooBarDemo, nullptr, mc);
CoGetComponentMetadata("FooBarDemo.so", nullptr, mc);
```



### 名词解释：

- Component， 构件，Component比class更大一级
- Coclass， 类
- class factory，类厂
- Interface（接口），接口是一组逻辑上相关的函数集合，是构件特征的抽象定义，是最基本的构件使用单位。接口概念类似于C++的抽象类（abstract class）、Java中的接口（interface），声明了“契约”。

- 构件类是最基本的构件运行实体（指构件对象），一个构件模块可以封装一到多个构件类的实现。构件类的实例是构件对象， 构件对象是接口的实现，一个构件对象可以实现多个接口，一个接口可以被多个构件对象实现。



## COMO运行时(runtime)基础(base)API一共由下列接口组成：

interface como::IMetaCoclass;
interface como::IMetaComponent;
interface como::IMetaInterface;




## 接口介绍

### IClassObject

类元数据操作

##### 常量(Consts)

（无）

##### 方法总汇(Method Summary)
```idl
AttachMetadata([in] IMetaComponent* component);
GetMetadata([out] IMetaComponent&& component);
CreateObject([in] InterfaceID iid, [out] IInterface** object);
```

##### 方法详解(Method Detail)

- AttachMetadata([in] IMetaComponent* component);
  把构件元数据附着给类。

  **参数(Parameters):**

  1. [in] IMetaComponent* component

- GetMetadata([out] IMetaComponent&& component);
  取构件元数据。

  **参数(Parameters):**

  1. [out] IMetaComponent&& component

- CreateObject([in] InterfaceID iid, [out] IInterface** object);
  创建对象。

  **参数(Parameters):**

  1. [in] InterfaceID iid
  2. [out] IInterface** object



### IObject

构件元数据操作

##### 常量(Consts)

（无）

##### 方法总汇(Method Summary)
```idl
AttachMetadata([in] IMetaComponent* component, [in] String coclassName);
GetCoclassID([out] CoclassID& cid);
GetCoclass([out] IMetaCoclass&& klass);
GetHashCode([out] Integer& hash);
Equals([in] IInterface* obj, [out] Boolean& same);
SetObjectObserver([in] IObjectObserver* observer);
ToString([out] String& desc);
```

##### 方法详解(Method Detail)

- AttachMetadata([in] IMetaComponent* component, [in] String coclassName);
  把构件元数据附着给类。

  **参数(Parameters):**

  1. [in] IMetaComponent* component
  2. [in] String coclassName

- GetCoclassID([out] CoclassID& cid);
  获取类标识ID。

  **参数(Parameters):**

  1. [out] CoclassID& cid

- GetCoclass([out] IMetaCoclass&& klass);
  获取类元数据。

  **参数(Parameters):**

  1. [out] IMetaCoclass&& klass

- GetHashCode([out] Integer& hash);
  获取对象Hash值。

  **参数(Parameters):**

  1. [out] Integer& hash

- Equals([in] IInterface* obj, [out] Boolean& same);
  判断是否同一对象。

  **参数(Parameters):**

  1. [in] IInterface* obj
  2. [out] Boolean& same

- SetObjectObserver([in] IObjectObserver* observer);
  设置对象观察器。

  **参数(Parameters):**

  1. [in] IObjectObserver* observer

- ToString([out] String& desc);
  转为字符串。

  **参数(Parameters):**

  1. [out] String& desc



### IClassLoader

类加载器

##### 常量(Consts)

（无）

##### 方法总汇(Method Summary)
```idl
LoadComponent([in] String path, [out] IMetaComponent&& component);
LoadComponent([in] ComponentID compId, [out] IMetaComponent&& component);
UnloadComponent([in] ComponentID compId);
LoadMetadata([in] Array<Byte> metadata, [out] IMetaComponent&& component);
LoadCoclass([in] String fullName, [out] IMetaCoclass&& klass);
LoadCoclass([in] CoclassID cid, [out] IMetaCoclass&& klass);
LoadInterface([in] String fullName, [out] IMetaInterface&& intf);
GetParent([out] IClassLoader&& parent);
```

##### 方法详解(Method Detail)

- LoadComponent([in] String path, [out] IMetaComponent&& component);
  获取构件元数据。

  **参数(Parameters):**

  1. [in] String path
  2. [out] IMetaComponent&& component

- LoadComponent([in] ComponentID compId, [out] IMetaComponent&& component);
  获取构件元数据。

  **参数(Parameters):**

  1. [in] ComponentID compId
  2. [out] IMetaComponent&& component

- UnloadComponent([in] ComponentID compId);
  卸载构件。

  **参数(Parameters):**

  1. [in] ComponentID compId

- LoadMetadata([in] Array<Byte> metadata, [out] IMetaComponent&& component);
  获取构件元数据。

  **参数(Parameters):**

  1. [in] Array<Byte> metadata
  2. [out] IMetaComponent&& component

- LoadCoclass([in] String fullName, [out] IMetaCoclass&& klass);
  装入类。

  **参数(Parameters):**

  1. [in] String fullName
  2. [out] IMetaCoclass&& klass

- LoadCoclass([in] CoclassID cid, [out] IMetaCoclass&& klass);
  装入构件。

  **参数(Parameters):**

  1. [in] CoclassID cid
  2. [out] IMetaCoclass&& klass

- LoadInterface([in] String fullName, [out] IMetaInterface&& intf);
  装入接口。

  **参数(Parameters):**

  1. [in] String fullName
  2. [out] IMetaInterface&& intf

- GetParent([out] IClassLoader&& parent);
  获取父类加载器。

  **参数(Parameters):**

  1. [out] IClassLoader&& parent



### IWeakReference

弱引用接口

##### 常量(Consts)

（无）

##### 方法总汇(Method Summary)
```idl
Resolve([in] InterfaceID iid, [out] IInterface** object);
```

##### 方法详解(Method Detail)

- Resolve([in] InterfaceID iid, [out] IInterface** object);
  解析接口。

  **参数(Parameters):**

  1. [in] InterfaceID iid,
  2. [out] IInterface** object



### IWeakReferenceSource

弱引用源接口

##### 常量(Consts)

（无）

##### 方法总汇(Method Summary)
```idl
GetWeakReference([out] IWeakReference&& wr);
```

##### 方法详解(Method Detail)

- GetWeakReference([out] IWeakReference&& wr);
  解析接口。

  **参数(Parameters):**

  1. [out] IWeakReference&& wr




### IObjectObserver

对象观察器

##### 常量(Consts)

（无）

##### 方法总汇(Method Summary)
```idl
OnLastStrongRef([in] IObject* obj);
OnLastWeakRef([in] IObject* obj);
```

##### 方法详解(Method Detail)

- OnLastStrongRef([in] IObject* obj);
  最后一次强连接回调函数。

  **参数(Parameters):**

  1. [in] IObject* obj

- OnLastWeakRef([in] IObject* obj);
  最后一次弱连接回调函数。

  **参数(Parameters):**

  1. [in] IObject* obj
