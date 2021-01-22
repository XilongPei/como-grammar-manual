# COMO运行时(runtime)远程过程调用(rpc)



RPC



## 常量

enum RPCType   {      Local,      Remote  }

enum RPCPeer  {      Proxy,      Stub  }



## COMO运行时(runtime)远程过程调用(rpc)API一共由下列接口组成：

interface como::IParcel;

interface como::IParcelable;

interface como::IInterfacePack;





## 接口介绍

### IParcel

数据包裹操作

##### 常量(Consts)

（无）

##### 方法总汇(Method Summary)
```idl
ReadChar([out] Char& value);
WriteChar([in] Char value);
ReadByte([out] Byte& value);
WriteByte([in] Byte value);
ReadShort([out] Short& value);
WriteShort([in] Short value);
ReadInteger([out] Integer& value);
WriteInteger([in] Integer value);
ReadLong([out] Long& value);
WriteLong([in] Long value);
ReadFloat([out] Float& value);
WriteFloat([in] Float value);
ReadDouble([out] Double& value);
WriteDouble([in] Double value);
ReadBoolean([out] Boolean& value);
WriteBoolean([in] Boolean value);
ReadString([out] String& value);
WriteString([in] String value);
ReadCoclassID([out] CoclassID& value);
WriteCoclassID([in] CoclassID value);
ReadComponentID([out] ComponentID& value);
WriteComponentID([in] ComponentID value);
ReadInterfaceID([out] InterfaceID& value);
WriteInterfaceID([in] InterfaceID value);
ReadECode([out] ECode& value);
WriteECode([in] ECode value);
ReadEnumeration([out] Integer& value);
WriteEnumeration([in] Integer value);
ReadArray([out] Triple* array);
WriteArray([in] Triple array);
ReadInterface([out] IInterface&& value);
WriteInterface([in] IInterface* value);
GetData([out] HANDLE& data);
GetDataSize([out] Long& size);
SetData([in] HANDLE data, [in] Long size);
GetDataPosition([out] Long& pos);
SetDataPosition([in] Long pos);
GetPayload([out] HANDLE& payload);
SetPayload([in] HANDLE payload, [in] Boolean release);
```

##### 方法详解(Method Detail)

- ReadChar([out] Char& value);
  读一个字符。

  **参数(Parameters):**

  1. [out] Char& value



### IParcelable

数据操作

##### 常量(Consts)

（无）

##### 方法总汇(Method Summary)
```idl
ReadFromParcel([in] IParcel* source);
WriteToParcel([in] IParcel* dest);
```



### IInterfacePack

接口操作

##### 常量(Consts)

（无）

##### 方法总汇(Method Summary)
```idl
GetCoclassID([out] CoclassID& cid);
GetInterfaceID([out] InterfaceID& iid);
IsParcelable([out] Boolean& parcelable);
GetHashCode([out] Integer& hash);
```



### IRPCChannel

RPC通道操作

##### 常量(Consts)

（无）

##### 方法总汇(Method Summary)
```idl
Apply([in] IInterfacePack* ipack);
GetRPCType([out] RPCType& type);
IsPeerAlive([out] Boolean& alive);
LinkToDeath([in] IProxy* proxy, [in] IDeathRecipient* recipient, [in] HANDLE cookie = 0, [in] Integer flags = 0);
UnlinkToDeath([in] IProxy* proxy, [in] IDeathRecipient* recipient, [in] HANDLE cookie = 0, [in] Integer flags = 0, [out] IDeathRecipient&* outRecipient = nullptr);
GetComponentMetadata([in] CoclassID cid, [out, callee] Array<Byte>& metadata);
Invoke([in] IMetaMethod* method, [in] IParcel* argParcel, [out] IParcel&& resParcel);
StartListening([in] IStub* stub);
Match([in] IInterfacePack* ipack, [out] Boolean& matched);
```



### IRPCChannelFactory

RPC通道工厂

##### 常量(Consts)

（无）

##### 方法总汇(Method Summary)
```idl
CreateInterfacePack([out] IInterfacePack&& ipack);
CreateParcel([out] IParcel&& parcel);
CreateChannel([in] RPCPeer peer, [out] IRPCChannel&& channel);
MarshalInterface([in] IInterface* object, [out] IInterfacePack&& ipack);
UnmarshalInterface([in] IInterfacePack* data, [out] IInterface&& object);
```



### IProxy

代理接口

##### 常量(Consts)

（无）

##### 方法总汇(Method Summary)
```idl
GetTargetCoclass([out] IMetaCoclass&& target);
IsStubAlive([out] Boolean& alive);
LinkToDeath([in] IDeathRecipient* recipient, [in] HANDLE cookie = 0, [in] Integer flags = 0);
UnlinkToDeath([in] IDeathRecipient* recipient, [in] HANDLE cookie = 0, [in] Integer flags = 0, [out] IDeathRecipient&* outRecipient = nullptr);
```



### IDeathRecipient

析构接受器

##### 常量(Consts)

（无）

##### 方法总汇(Method Summary)
```idl
StubDied([in] IProxy* who);
```



### IStub

残桩

##### 常量(Consts)

（无）

##### 方法总汇(Method Summary)
```idl
Match([in] IInterfacePack* ipack, [out] Boolean& matched);
Invoke([in] IParcel* argParcel, [out] IParcel&& resParcel);
```