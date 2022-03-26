# 构件描述语言(cdl) 编译器cdlc生成夹壁墙代码



## 构件模式

以 /como/test/runtime/IDfromName/component/CMakeLists.txt 为例:

```
add_custom_command(
    OUTPUT
        ${GENERATED_SOURCES}
    COMMAND
        "$ENV{CDLC}"
        -gen
        -mode-component
        -split
        -d ${COMPONENT_OBJ_DIR}
        -i "${COMPONENT_SRC_DIR}"
        -c "${COMPONENT_SRC_DIR}/FooBarDemo.cdl")

```

- -mode-component          Set the "component" mode which is used to compile .cdl files of components

- -split    Generate interface or class declarations into seperate files when in the "component" mode or the "client" mode

#### 生成的文件：

```
set(GENERATED_SOURCES
    ${COMPONENT_OBJ_DIR}/_como_demo_CFoo.cpp
    ${COMPONENT_OBJ_DIR}/_como_demo_CFooBar.cpp
    ${COMPONENT_OBJ_DIR}/FooBarDemoPub.cpp
    ${COMPONENT_OBJ_DIR}/MetadataWrapper.cpp)
```



#### cdlc编译器中源码：

```cpp
void CodeGenerator::ComponentModeEmitter::Emit()
{
    EmitConstantsAndTypes();		// 生成：FooBarDemo.h FooBarDemo是module名字
    if (mOwner->mMode & Properties::CODEGEN_SPLIT) {
        EmitInterfaceDeclarationsSplitly();	
    }								// 生成：como.demo.IBar.h como.demo.ICFooBarClassObject.h
    								//		como.demo.IFoo.h
    
    EmitCoclasses();				// 生成：_como_demo_CFoo.cpp _como_demo_CFoo.h 
    								//		_como_demo_CFooBar.cpp _como_demo_CFooBar.h
    								// ^^^每个构件类一个.cpp文件、一个.h文件
    
    EmitComponentCpp();				// 生成：FooBarDemoPub.cpp
    
    EmitMetadataWrapper();			// 生成：MetadataWrapper.cpp
}
```



## module定义

一个COMO构件是一个动态链接库，它的文件名要唯一，如这里的：IDfromName_FooBar_demo.so。COMO的运行时将在环境变量`LIB_PATH`指示的位置找这个文件。

```
[
    //uuid(6a07eff9-9090-41a9-bfc3-67eb3ed80f49),
    uri("http://como.org/component/sample/IDfromName_FooBar_demo.so")
]
module FooBarDemo
```

- 如果uuid不写，CDL编译器cdlc根据名字，hash得到一个唯一名。

- COMO构件名不可以重复，否则会加载错误构件。



# 客户端模式

```
add_custom_command(
    OUTPUT
        ${GENERATED_SOURCES}
    COMMAND
        "$ENV{CDLC}"
        -gen
        -mode-client
        -split
        -d ${CLIENT_OBJ_DIR}
        -metadata-so "${BIN_DIR}/IDfromName_FooBar_demo.so")
```

#### 生成的文件：

```
set(GENERATED_SOURCES
    ${CLIENT_OBJ_DIR}/FooBarDemo.cpp)
```



#### cdlc编译器中源码：

```cpp

void CodeGenerator::ClientModeEmitter::Emit()
{
    EmitConstantsAndTypes();		// 生成：FooBarDemo.h FooBarDemo是module名字
    if (mOwner->mMode & Properties::CODEGEN_SPLIT) {
        EmitInterfaceDeclarationsSplitly();	// 生成：como.demo.IBar.h como.demo.ICFooBarClassObject.h
    										//		como.demo.IFoo.h

		EmitCoclassDeclarationsSplitly();	// 生成：como_demo_CFoo.h como_demo_CFooBar.h
    }

	EmitComponentCpp();				// 生成：FooBarDemoPub.cpp
}

```



#### CMakeLists.txt中要有的内容

```makefile
set(CLIENT_OBJ_DIR ${IDfromName_OBJ_DIR}/client)

IMPORT_GTEST()

include_directories(
    ./
    ${INC_DIR}
    ${CLIENT_OBJ_DIR})

set(SOURCES
    main.cpp)

set(GENERATED_SOURCES
    ${CLIENT_OBJ_DIR}/FooBarDemo.cpp)
```



#### 创建构件对象

```
TEST(TestIDfromName, testNewWithoutIID)
{
    InterfaceID iid = InterfaceIDfromName("como::demo::IBar", nullptr);
    AutoPtr<IBar> bar;
    ec = CFooBar::New(iid, (IInterface**)&bar);		// 这里New对象，用的是CFooBar的元数据，所以iid只要有接口
    												// 的uuid就够了，故第二个参数ComponentID可以是nullptr
    EXPECT_EQ(ec, NOERROR);
    bar->Bar(String("testNewWithoutIID: Tongji University"));
}

```

