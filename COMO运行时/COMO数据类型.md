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



## 方法签名

方法签名是在元数据中，对方法除方法名（函数名）之外，参数及返回值数据类型的标识字符串。

Method::BuildSignature

(参数签名)返回值类型签名

#### Type Signature

|    Type     | Signature |
| :---------: | :-------: |
|    Byte     |     B     |
|    Short    |     S     |
|   Integer   |     I     |
|    Long     |     L     |
|    Float    |     F     |
|   Double    |     D     |
|    Char     |     C     |
|   Boolean   |     Z     |
|   String    |     T     |
|   HANDLE    |     H     |
|    ECode    |     E     |
|  CoclassID  |     K     |
| ComponentID |     M     |
| InterfaceID |     U     |
|   Triple    |     R     |
|    Array    |     [     |
|   Pointer   |     *     |
|  Reference  |     &     |
|    Enum     |  Lxx/xx;  |
|  Interface  |  Lxx/xx;  |



#### 局部数据类型Local Types
局部数据类型是经过RPC时必须对其进行编码的数据类型，包括：
- HANDLE,
- CoclassID,
- ComponentID,
- InterfaceID
- Array<*local types*>.

#### 参考
[Java Syntax Specification](https://users-cs.au.dk/amoeller/RegAut/JavaBNF.html)