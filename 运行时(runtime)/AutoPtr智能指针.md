# AutoPtr智能指针



## AutoPtr智能指针定义

#### 头（.h）文件

/como/como/runtime/util/comosp.h

```
namespace como {

template<typename T>
class AutoPtr
{
public:
    inline AutoPtr()
        : mPtr(nullptr)
    {}

    AutoPtr(
        /* [in] */ T* other);
......
```

#### 说明

- 所有被定义为AutoPtr的指针的原型必须是继承自Object的类

```
class CA
    : public Object
{
public:
    CA()
    {
        CA_CREATED = true;
    }
```





## AutoPtr编程应用示例

参见测试用例：

/como/test/runtime/autoptr/main.cpp

#### 作为函数返回值

```
AutoPtr<CA> CreateCA()
{
    return new CA();
}

AutoPtr<CA> GetCA()
{
    static const AutoPtr<CA> gCA = new CA();
    return gCA;
}

void CreateCA2(
    /* [out] */ CA** o)
{
    *o = new CA();
    REFCOUNT_ADD(*o);
}
```

#### 作为函数参数

```
void CallCA(CA* obj)
{
    EXPECT_TRUE(CA_CREATED);
    EXPECT_EQ(1, obj->GetStrongCount());
    EXPECT_FALSE(CA_DESTROYED);
}

void CallCA2(AutoPtr<CA> obj)
{
    EXPECT_TRUE(CA_CREATED);
    EXPECT_EQ(1, obj->GetStrongCount());
    EXPECT_FALSE(CA_DESTROYED);
}

void CallCA3(AutoPtr<CA>&& obj)
{
    EXPECT_TRUE(CA_CREATED);
    EXPECT_EQ(1, obj->GetStrongCount());
    EXPECT_FALSE(CA_DESTROYED);
}
```





## 其它说明

### 引用计数操作

```cpp
#define REFCOUNT_ADD(i)     if (i) { (i)->AddRef(); }
#define REFCOUNT_RELEASE(i) if (i) { (i)->Release(); }
```



##### 如果你想把一个智能指针在别的地方用

```cpp
case TypeKind::Interface: {
    AutoPtr<IInterface> value;
    argParcel->ReadInterface(value);
    argList->SetInputArgumentOfInterface(i, value);
    REFCOUNT_ADD(value);
    break;
}
```



如果想写好这个文档，可以参考C++智能指针方面的文档

https://www.toutiao.com/article/6839870422234694156