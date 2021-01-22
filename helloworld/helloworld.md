# hello world



## make工具

COMO使用CMake作为make工具。

长远上看，COMO将提供更多手段供其它make工具使用COMO构件，但是build COMO构件本身还是得用CMake工具。

CMake是一个跨平台的安装（编译）工具，可以用简单的语句来描述所有平台的安装（编译过程）。它能够输出各种各样的makefile或者project文件，能测试编译器所支持的C++特性，类似于UNIX下的automake。只是 CMake 的组态文件取名为 CMakeLists.txt。Cmake 并不直接建构出最终的软件，而是产生标准的建构文件（如 Unix 的 Makefile 或 Windows Visual C++ 的projects/workspaces），然后再依一般的构建方式使用。这使得熟悉某个集成开发环境（IDE）的开发者可以用标准的方式建构它的软件，这种可以使用各平台的原生建构系统的能力是 CMake 和 SCons 等其他类似系统的区别之处。



##### 源码目录结构
```
├── CMakeLists.txt
└── democomponent
    ├── CMakeLists.txt
    ├── client
    │   ├── CMakeLists.txt
    │   └── main.cpp
    └── component
        ├── CFoo.cpp
        ├── CFoo.h
        ├── CFooBar.cpp
        ├── CFooBar.h
        ├── CMakeLists.txt
        ├── FooBarDemo.cdl
        ├── IBar.cdl
        └── IFoo.cdl
```



##### 最外层CMakeLists.txt

```makefile
set(SAMPLES_SRC_DIR ${PROJECT_DIR}/samples)
set(SAMPLES_OBJ_DIR ${OBJ_DIR}/samples)

add_subdirectory(democomponent)
```






## client主程序

##### CMakeLists.txt

```makefile
project(demo-client CXX)

set(CLIENT_SRC_DIR ${DEMOCOMPONENT_SRC_DIR}/client)
set(CLIENT_OBJ_DIR ${DEMOCOMPONENT_OBJ_DIR}/client)

include_directories(
    ./
    ${INC_DIR}
    ${CLIENT_OBJ_DIR})

set(SOURCES
    main.cpp)

set(GENERATED_SOURCES
    ${CLIENT_OBJ_DIR}/FooBarDemo.cpp)

IMPORT_LIBRARY(comort.so)

add_executable(demo
    ${SOURCES}
    ${GENERATED_SOURCES})
target_link_libraries(demo comort.so)
add_dependencies(demo FooBarDemo)

add_custom_command(
    OUTPUT
        ${GENERATED_SOURCES}
    COMMAND
        "$ENV{CDLC}"
        -gen
        -mode-client
        -split
        -d ${CLIENT_OBJ_DIR}
        -metadata-so "${BIN_DIR}/FooBarDemo.so")

COPY(demo ${CLIENT_OBJ_DIR}/demo ${BIN_DIR})

install(FILES
    ${CLIENT_OBJ_DIR}/demo
    DESTINATION ${BIN_DIR}
    PERMISSIONS
        OWNER_READ
        OWNER_WRITE
        OWNER_EXECUTE
        GROUP_READ
        GROUP_WRITE
        GROUP_EXECUTE
        WORLD_READ
        WORLD_EXECUTE)

```



##### main.cpp

```c++
#include "como.demo.CFoo.h"
#include "como.demo.IFoo.h"
#include "FooBarDemo.h"
#include <comoapi.h>
#include <comosp.h>
#include <cstdio>
#include <cstdlib>

using namespace como;

using como::demo::CFoo;
using como::demo::IFoo;
using como::demo::IID_IFoo;

int main(int argc, char** argv)
{
    AutoPtr<IFoo> foo;
    CFoo::New(IID_IFoo, (IInterface**)&foo);
    foo->Foo(9);

    AutoPtr<IMetaComponent> mc;
    CoGetComponentMetadata(CID_FooBarDemo, nullptr, mc);
    String name;
    mc->GetName(name);
    printf("==== component name: %s ====\n\n", name.string());

    Integer clsNumber;
    mc->GetCoclassNumber(clsNumber);
    printf("==== component class number: %d ====\n", clsNumber);
    Array<IMetaCoclass*> klasses(clsNumber);
    mc->GetAllCoclasses(klasses);
    for (Integer i = 0; i < klasses.GetLength(); i++) {
        String clsName, clsNs;
        klasses[i]->GetName(clsName);
        klasses[i]->GetNamespace(clsNs);
        printf("==== [%d] class name: %s, namespace: %s ====\n",
                i, clsName.string(), clsNs.string());
    }
    printf("\n");

    Integer intfNumber;
    mc->GetInterfaceNumber(intfNumber);
    printf("==== component interface number: %d ====\n", intfNumber);
    Array<IMetaInterface*> intfs(intfNumber);
    mc->GetAllInterfaces(intfs);
    for (Integer i = 0; i < intfs.GetLength(); i++) {
        String intfName, intfNs;
        intfs[i]->GetName(intfName);
        intfs[i]->GetNamespace(intfNs);
        printf("==== [%d] interface name: %s, namespace: %s ====\n",
                i, intfName.string(), intfNs.string());
    }
    printf("\n");

    AutoPtr<IInterface> obj;
    klasses[0]->CreateObject(IID_IInterface, &obj);

    return 0;
}
```



## component构件程序

##### CMakeLists.txt

```makefile
project(demo-component CXX)

set(COMPONENT_SRC_DIR ${DEMOCOMPONENT_SRC_DIR}/component)
set(COMPONENT_OBJ_DIR ${DEMOCOMPONENT_OBJ_DIR}/component)

include_directories(
    ./
    ${INC_DIR}
    ${COMPONENT_OBJ_DIR})

set(SOURCES
    CFoo.cpp
    CFooBar.cpp)

set(GENERATED_SOURCES
    ${COMPONENT_OBJ_DIR}/_como_demo_CFoo.cpp
    ${COMPONENT_OBJ_DIR}/_como_demo_CFooBar.cpp
    ${COMPONENT_OBJ_DIR}/FooBarDemoPub.cpp
    ${COMPONENT_OBJ_DIR}/MetadataWrapper.cpp)

IMPORT_LIBRARY(comort.so)

add_library(FooBarDemo
    SHARED
    ${SOURCES}
    ${GENERATED_SOURCES})
target_link_libraries(FooBarDemo comort.so)
add_dependencies(FooBarDemo comort)

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

COPY(FooBarDemo ${COMPONENT_OBJ_DIR}/FooBarDemo.so ${BIN_DIR})

install(FILES
    ${COMPONENT_OBJ_DIR}/FooBarDemo.so
    DESTINATION ${BIN_DIR}
    PERMISSIONS
        OWNER_READ
        OWNER_WRITE
        OWNER_EXECUTE
        GROUP_READ
        GROUP_WRITE
        GROUP_EXECUTE
        WORLD_READ
        WORLD_EXECUTE)

```

##### IDL文件

######  FooBarDemo.cdl
```idl
[
    uuid(6a07eff9-9090-41a9-bfc3-67eb3ed80f49),
    uri("http://como.org/component/sample/FooBarDemo.so")
]
module FooBarDemo
{

namespace como {
namespace demo {

const Long SERIALIZE_NUMBER = 0x123456789ABCDEFll;
const String MODULE_NAME = "FooBarDemo";

}
}

include "IFoo.cdl"
include "IBar.cdl"

namespace como {
namespace demo {

[
    uuid(b150ddc0-530b-411f-8462-b04252fd012b),
    version(0.1.0)
]
coclass CFoo
{
    interface IFoo;
}

[
    uuid(855db520-7f7a-4ad1-a60a-7d8d3461be7b),
    version(0.1.0)
]
coclass CFooBar
{
    Constructor();

    Constructor(
        [in] Long data);

    interface IFoo;
    interface IBar;
}

}
}

}

```

###### IFoo.cdl
```idl
namespace como {
namespace demo {

[
    uuid(6e7582c4-e0ec-4474-9046-702732e1e96d),
    version(0.1.0)
]
interface IFoo
{
    Foo(
        [in] Integer data);
}

}
}

```

###### IBar.idl

```idl
namespace como {
namespace demo {

[
    uuid(ef1b8367-e32d-4dc0-b320-688cf4d9bdec),
    version(0.1.0)
]
interface IBar
{
    Bar(
        [in] String data);
}

}
}

```



##### c++源文件

```

```

