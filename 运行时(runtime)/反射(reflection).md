# Como运行时(runtime)反射机制



### 名词解释：

- Component， 构件
- Coclass， 类



## como反射机制API一共由下列接口组成：

interface como::IArgumentList;
interface como::IClassLoader;
interface como::IMetaCoclass;
interface como::IMetaConstant;
interface como::IMetaConstructor;
interface como::IMetaEnumeration;
interface como::IMetaEnumerator;
interface como::IMetaInterface;
interface como::IMetaMethod;
interface como::IMetaParameter;
interface como::IMetaType;
interface como::IMetaValue;
interface como::IMetaComponent



## 常量

- enum IOAttribute  UNKNOWN,  IN,  OUT,  IN_OUT,  OUT_CALLEE

- enum TypeModification  NAKED,  POINTER,  POINTER_POINTER,  POINTER_REFERENCE,  REFERENCE,  REFERENCE_POINTER,  REFERENCE_REFERENCE



## 接口介绍

### IMetaComponent
##### 常量(Consts)
##### 方法总汇(Method Summary)
```idl
GetName([out] String& name);
GetComponentID([out] ComponentID& cid);
GetConstantNumber([out] Integer& number);
GetAllConstants([out] Array<IMetaConstant*>& consts);
GetConstant([in] String name, [out] IMetaConstant&& constt);
GetEnumerationNumber([out] Integer& number);
GetAllEnumerations([out] Array<IMetaEnumeration*>& enumns);
GetEnumeration([in] String fullName, [out] IMetaEnumeration&& enumn);
GetInterfaceNumber([out] Integer& number);
GetAllInterfaces([out] Array<IMetaInterface*>& intfs);
GetInterface([in] String fullName, [out] IMetaInterface&& intf);
GetInterface([in] InterfaceID iid, [out] IMetaInterface&& intf);
GetCoclassNumber([out] Integer& number);
GetAllCoclasses([out] Array<IMetaCoclass*>& klasses);
GetCoclass([in] String fullName, [out] IMetaCoclass&& klass);
GetCoclass([in] CoclassID cid, [out] IMetaCoclass&& klass);
GetSerializedMetadata([out, callee] Array<Byte>& metadata);
IsOnlyMetadata([out] Boolean& onlyMetadata);
LoadComponent([in] IMetaComponent* comp);
Preload();
CanUnload([out] Boolean& unload);
Unload();
```

##### 方法详解(Method Detail)

- GetName([out] String& name);
  获取构件名称。

  **参数(Parameters):**
  
  1. [out] String& name
  
- GetComponentID([out] ComponentID& cid);
  **参数(Parameters):**
  
    1. [out] ComponentID& cid



### IMetaCoclass
##### 常量(Consts)
##### 方法总汇(Method Summary)
```idl
GetComponent([out] IMetaComponent&& comp);
GetName([out] String& name);
GetNamespace([out] String& ns);
GetCoclassID([out] CoclassID& cid);
GetClassLoader([out] IClassLoader&& loader);
GetConstructorNumber([out] Integer& number);
GetAllConstructors([out] Array<IMetaConstructor*>& constrs);
GetConstructor([in] String signature, [out] IMetaConstructor&& constr);
GetInterfaceNumber([out] Integer& number);
GetAllInterfaces([out] Array<IMetaInterface*>& intfs);
GetInterface([in] String fullName, [out] IMetaInterface&& intf);
ContainsInterface([in] String fullName, [out] Boolean& result);
GetMethodNumber([out] Integer& number);
GetAllMethods([out] Array<IMetaMethod*>& methods);
GetMethod([in] String name, [in] String signature, [out] IMetaMethod&& method);
CreateObject([in] InterfaceID iid, [out] IInterface** object);
```

##### 方法详解(Method Detail)

- GetComponent([out] IMetaComponent&& comp);
  **参数(Parameters):**
    1. [out] IMetaComponent&& comp



### IMetaCoclass

##### 常量(Consts)
##### 方法总汇(Method Summary)
```idl
GetComponent([out] IMetaComponent&& comp);
GetName([out] String& name);
GetNamespace([out] String& ns);
GetEnumeratorNumber([out] Integer& number);
GetAllEnumerators([out] Array<IMetaEnumerator*>& enumrs);
GetEnumerator([in] String name, [out] IMetaEnumerator&& enumr);
```

##### 方法详解(Method Detail)

- GetComponent([out] IMetaComponent&& comp);
  **参数(Parameters):**
    1. [out] IMetaComponent&& comp




### IMetaEnumerator
##### 常量(Consts)
##### 方法总汇(Method Summary)
```idl
GetEnumeration([out] IMetaEnumeration&& enumn);
GetName([out] String& name);
GetValue([out] Integer& value);
```
##### 方法详解(Method Detail)

- GetEnumeration([out] IMetaEnumeration&& enumn);
  获取构件名称。

  **参数(Parameters):**
  
    1. [



### IMetaInterface

##### 常量(Consts)
##### 方法总汇(Method Summary)
```idl
GetComponent([out] IMetaComponent&& comp);
GetName([out] String& name);
GetNamespace([out] String& ns);
GetInterfaceID([out] InterfaceID& iid);
GetBaseInterface([out] IMetaInterface&& baseIntf);
GetConstantNumber([out] Integer& number);
GetAllConstants([out] Array<IMetaConstant*>& consts);
GetConstant([in] String name, [out] IMetaConstant&& constt);
GetConstant([in] Integer index, [out] IMetaConstant&& constt);
GetMethodNumber([out] Integer& number);
GetAllMethods([out] Array<IMetaMethod*>& methods);
GetDeclaredMethodNumber([out] Integer& number);
GetDeclaredMethods([out] Array<IMetaMethod*>& methods);
GetMethod([in] String name, [in] String signature, [out] IMetaMethod&& method);
GetMethod([in] Integer index, [out] IMetaMethod&& method);
```
##### 方法详解(Method Detail)

- GetComponent([out] IMetaComponent&& comp);
  获取。

  **参数(Parameters):**
  
    1. [



### IMetaConstant

##### 常量(Consts)
##### 方法总汇(Method Summary)
```idl
GetName([out] String& name);
GetNamespace([out] String& ns);
GetType([out] IMetaType&& type);
GetValue([out] IMetaValue&& value);
```
##### 方法详解(Method Detail)

- GetName([out] String& name);
  获取名称。

  **参数(Parameters):**
  
    1. [



### IMetaMethod

##### 常量(Consts)
##### 方法总汇(Method Summary)
```idl
GetInterface([out] IMetaInterface&& intf);
GetName([out] String& name);
GetSignature([out] String& signature);
GetParameterNumber([out] Integer& number);
GetAllParameters([out] Array<IMetaParameter*>& params);
GetParameter([in] String name,[out] IMetaParameter&& param);
GetParameter([in] Integer index,[out] IMetaParameter&& param);
HasOutArguments([out] Boolean& outArgs);
CreateArgumentList([out] IArgumentList&& argList);
Invoke([in] IInterface* thisObject,[in] IArgumentList* argList);
```
##### 方法详解(Method Detail)

- GetInterface([out] IMetaInterface&& intf);
  获取。

  **参数(Parameters):**
  
    1. [



### IMetaConstructor : IMetaMethod

##### 常量(Consts)
##### 方法总汇(Method Summary)
```idl
GetCoclass([out] IMetaCoclass&& klass);
IsDefault([out] Boolean& isDefault);
CreateObject([in] IArgumentList* argList, [out] IInterface&& object);
```
##### 方法详解(Method Detail)

- GetCoclass([out] IMetaCoclass&& klass);
  获取名称。

  **参数(Parameters):**
  
    1. [



### IMetaParameter

##### 常量(Consts)
##### 方法总汇(Method Summary)
```idl
GetMethod([out] IMetaMethod&& method);
GetName([out] String& name);
GetIndex([out] Integer& index);
GetIOAttribute([out] IOAttribute& attr);
GetType([out] IMetaType&& type);
```
##### 方法详解(Method Detail)

- GetName([out] String& name);
  获取构件名称。

  **参数(Parameters):**
  
    1. [



### IMetaType

##### 常量(Consts)
##### 方法总汇(Method Summary)
```idl
GetName([out] String& name);
GetTypeKind([out] TypeKind& kind);
GetElementType([out] IMetaType&& elemType);
GetTypeModification([out] TypeModification& typeMod);
```
##### 方法详解(Method Detail)

- GetName([out] String& name);
  获取名称。

  **参数(Parameters):**
  
    1. [
  
       


### IMetaValue

##### 常量(Consts)
##### 方法总汇(Method Summary)
```idl
GetType([out] IMetaType&& type);
GetBooleanValue([out] Boolean& value);
GetCharValue([out] Char& value);
GetByteValue([out] Byte& value);
GetShortValue([out] Short& value);
GetIntegerValue([out] Integer& value);
GetLongValue([out] Long& value);
GetFloatValue([out] Float& value);
GetDoubleValue([out] Double& value);
GetStringValue([out] String& value);
GetRadix([out] Integer& radix);
```
##### 方法详解(Method Detail)

- GetType([out] IMetaType&& type);
  获取。

  **参数(Parameters):**
  
    1. [out] IMetaType&& type



### IArgumentList

##### 常量(Consts)
##### 方法总汇(Method Summary)
```idl
GetInputArgumentOfByte([in] Integer index,[out] Byte& value);
SetInputArgumentOfByte([in] Integer index,[in] Byte value);
GetInputArgumentOfShort([in] Integer index,[out] Short& value);
SetInputArgumentOfShort([in] Integer index,[in] Short value);
GetInputArgumentOfInteger([in] Integer index,[out] Integer& value);
SetInputArgumentOfInteger([in] Integer index,[in] Integer value);
GetInputArgumentOfLong([in] Integer index,[out] Long& value);
SetInputArgumentOfLong([in] Integer index,[in] Long value);
GetInputArgumentOfFloat([in] Integer index,[out] Float& value);
SetInputArgumentOfFloat([in] Integer index,[in] Float value);
GetInputArgumentOfDouble([in] Integer index,[out] Double& value);
SetInputArgumentOfDouble([in] Integer index,[in] Double value);
GetInputArgumentOfChar([in] Integer index,[out] Char& value);
SetInputArgumentOfChar([in] Integer index,[in] Char value);
GetInputArgumentOfBoolean([in] Integer index,[out] Boolean& value);
SetInputArgumentOfBoolean([in] Integer index,[in] Boolean value);
GetInputArgumentOfString([in] Integer index,[out] String& value);
SetInputArgumentOfString([in] Integer index,[in] String value);
GetInputArgumentOfHANDLE([in] Integer index,[out] HANDLE& value);
SetInputArgumentOfHANDLE([in] Integer index,[in] HANDLE value);
GetInputArgumentOfECode([in] Integer index,[out] ECode& value);
SetInputArgumentOfECode([in] Integer index,[in] ECode value);
GetInputArgumentOfCoclassID([in] Integer index,[out] CoclassID& value);
SetInputArgumentOfCoclassID([in] Integer index,[in] CoclassID value);
GetInputArgumentOfComponentID([in] Integer index,[out] ComponentID& value);
SetInputArgumentOfComponentID([in] Integer index,[in] ComponentID value);
GetInputArgumentOfInterfaceID([in] Integer index,[out] InterfaceID& value);
SetInputArgumentOfInterfaceID([in] Integer index,[in] InterfaceID value);
GetInputArgumentOfArray([in] Integer index,[out] Triple& value);
SetInputArgumentOfArray([in] Integer index,[in] Triple value);
GetInputArgumentOfEnumeration([in] Integer index,[out] Integer& value);
SetInputArgumentOfEnumeration([in] Integer index,[in] Integer value);
GetInputArgumentOfInterface([in] Integer index,[out] IInterface&& value);
SetInputArgumentOfInterface([in] Integer index,[in] IInterface* value);
AssignOutputArgumentOfByte([in] Integer index,[in] Byte value);
SetOutputArgumentOfByte([in] Integer index,[in] HANDLE addr);
AssignOutputArgumentOfShort([in] Integer index,[in] Short value);
SetOutputArgumentOfShort([in] Integer index,[in] HANDLE addr);
AssignOutputArgumentOfInteger([in] Integer index,[in] Integer value);
SetOutputArgumentOfInteger([in] Integer index,[in] HANDLE addr);
AssignOutputArgumentOfLong([in] Integer index,[in] Long value);
SetOutputArgumentOfLong([in] Integer index,[in] HANDLE addr);
AssignOutputArgumentOfFloat([in] Integer index,[in] Float value);
SetOutputArgumentOfFloat([in] Integer index,[in] HANDLE addr);
AssignOutputArgumentOfDouble([in] Integer index,[in] Double value);
SetOutputArgumentOfDouble([in] Integer index,[in] HANDLE addr);
AssignOutputArgumentOfChar([in] Integer index,[in] Char value);
SetOutputArgumentOfChar([in] Integer index,[in] HANDLE addr);
AssignOutputArgumentOfBoolean([in] Integer index,[in] Boolean value);
SetOutputArgumentOfBoolean([in] Integer index,[in] HANDLE addr);
AssignOutputArgumentOfString([in] Integer index,[in] String value);
SetOutputArgumentOfString([in] Integer index,[in] HANDLE addr);
AssignOutputArgumentOfHANDLE([in] Integer index,[in] HANDLE value);
SetOutputArgumentOfHANDLE([in] Integer index,[in] HANDLE addr);
AssignOutputArgumentOfECode([in] Integer index,[in] ECode value);
SetOutputArgumentOfECode([in] Integer index,[in] HANDLE addr);
AssignOutputArgumentOfCoclassID([in] Integer index,[in] CoclassID value);
SetOutputArgumentOfCoclassID([in] Integer index,[in] HANDLE addr);
AssignOutputArgumentOfComponentID([in] Integer index,[in] ComponentID value);
SetOutputArgumentOfComponentID([in] Integer index,[in] HANDLE addr);
AssignOutputArgumentOfInterfaceID([in] Integer index,[in] InterfaceID value);
SetOutputArgumentOfInterfaceID([in] Integer index,[in] HANDLE addr);
AssignOutputArgumentOfArray([in] Integer index,[in] Triple value);
SetOutputArgumentOfArray([in] Integer index,[in] HANDLE addr);
AssignOutputArgumentOfEnumeration([in] Integer index,[in] Integer value);
SetOutputArgumentOfEnumeration([in] Integer index,[in] HANDLE addr);
AssignOutputArgumentOfInterface([in] Integer index,[in] IInterface* value);
SetOutputArgumentOfInterface([in] Integer index,[in] HANDLE addr);
GetArgumentAddress([in] Integer index,[out] HANDLE& addr);
```
##### 方法详解(Method Detail)

- GetInputArgumentOfByte([in] Integer index,[out] Byte& value);
  获取。

  **参数(Parameters):**
    1. [in] Integer index
    2. [out] Byte& value
