# COMO数据类型



## 基本数据类型

**源文件：**comotypes.h
namespace como

- typedef signed char     Byte;
- typedef short           Short;
- typedef int             Integer;
- typedef long long int   Long;
- typedef float           Float;
- typedef double          Double;
- typedef char32_t        Char;
- typedef bool            Boolean;
- typedef uintptr_t       HANDLE;
- typedef int             ECode;



## 复合数据类型

**源文件：**comouuid.h

namespace como

```c++
struct UUID
{
    unsigned int        mData1;
    unsigned short      mData2;
    unsigned short      mData3;
    unsigned short      mData4;
    unsigned char       mData5[6];
};

struct InterfaceID
{
    UUID                mUuid;
    const ComponentID*  mCid;
    static const InterfaceID Null;
};
```