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

# 标记-清除
引用计数无法处理循环引用的情况，所以有标记清除
```python
a = [1, 3, 3]
b = [2, 3, 4]
b.append(a)
a.append(b)
```
# 隔代回收
当引用计数变为0会被清除，那什么时候会触发清除，这引入了隔代回收的概念
```python
gc.get_threshold()
# 700,10,10
```
700 新增的对象数量减去删除的对象数量>700时，会执行一次垃圾回收
当0代回收进行10次的时候就进行2代回收
当2代回收进行10次的时候就进行2代回收
