# 引用计数
Python中每个东西都是对象，核心就是PyObject。其中ob_refcnt就是作为引用计数。当引用计数为0时，该对象生命就结束了。如下面两段代码.
```c
typedef struct_object {
 int ob_refcnt;
 struct_typeobject *ob_type;
} PyObject;
```
```c
#define Py_INCREF(op)   ((op)->ob_refcnt++) //增加计数
#define Py_DECREF(op) \ //减少计数
    if (--(op)->ob_refcnt != 0) \
        ; \
    else \
        __Py_Dealloc((PyObject *)(op))
```
优点：简单，实时，处理回收内存的时间分摊到了平时。
缺点：维护引用计数消耗资源。循环引用。
