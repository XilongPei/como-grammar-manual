# 元数据级语言互操作支持（Opaque）机制



## 反射机制中，支持opaque用户自定义内容字段

反射机制中，得到module、coclass、method，有时用户需要往反射机制中“记录”一些临时数据，opaque，这个概念来自 quickjs，这样方便 python、javascript等引擎在python、javascript与COMO间做“粘合”。

## 反射机制中有关Opaque的接口

##### 哪些接口有这两个函数

```
    SetOpaque(
        [in] HANDLE value);

    GetOpaque(
        [out] HANDLE& value);

```



接口

- interface IMetaComponent
- interface IMetaCoclass
- interface IMetaMethod



## Opaque使用示例

quickjs与COMO互操作 https://gitee.com/tjopenlab/como_quickjs/blob/master/quickjs.c

```c++
JSValue JS_NewCFunctionData(JSContext *ctx, JSCFunctionData *func,
                            int length, int magic, int data_len,
                            JSValueConst *data)
{
......
    s->func = func;
    s->length = length;
    s->data_len = data_len;
    s->magic = magic;
    for(i = 0; i < data_len; i++)
        s->data[i] = JS_DupValue(ctx, data[i]);
    JS_SetOpaque(func_obj, s);
    js_function_set_properties(ctx, func_obj,
                               JS_ATOM_empty_string, length);
    return func_obj;
}
```





## 顺带说说BTW

Opaque这个概念 quickjs中有，以微内核著称的seL4中也有类似做法。

seL4白皮书（https://sel4.systems/About/seL4-whitepaper.pdf）中有这样的说法：

> Capabilities have further nice properties. One is the ability to interpose access, which is a consequence of the fact that they are **opaque** object references.