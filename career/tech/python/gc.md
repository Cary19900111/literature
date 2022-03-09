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
引用计数无法处理循环引用（只有容器对象才会产生循环引用的情况，比如列表、字典、用户自定义对象等）的情况。所以需要标记清除来管理回收，循环调用如同以下例子。
```python
a = [1,2]
b = [3,4]
b.append(a)
a.append(b)
del a
del b
```
用于循环引用垃圾回收的头
```c
typedef union _gc_head {
    struct {
        union _gc_head *gc_next; /* 用于双向链表 */
        union _gc_head *gc_prev; /* 用于双向链表 */
        Py_ssize_t gc_refs;      /* 用于复制 */
    } gc;
    long double dummy;
} PyGC_Head;
```
dummy作用：将整个结构体 PyGC_Head 的大小对齐为 long double 型。
a的计数为2，b的计数为2。删除a，删除b，计数都-1，余1，引用不能回收。    
该算法在进行垃圾回收时拢共分两步。
步骤1：标记阶段。该阶段会遍历所有的对象，如果是可达的，即还有对象引用它，那么就标记该对象为可达。
步骤2：清除阶段。再次遍历对象，如果发现某个对象没有标记可达，则将其回收。
这里解释一下root object，它指的其实就是一些全局变量、调用栈、寄存器，这些对象时不可删除的。

# 隔代回收
解决标记清除应该在什么时间进行扫描的问题。减少标记-清除机制所带来的额外操作。  
当引用计数变为0会被清除，那什么时候会触发清除，这引入了隔代回收的概念
```python
gc.get_threshold()
# 700,10,10
```
700 生成的容器对象的数量-删除的容器对象的数量
当0代回收进行10次的时候就进行2代回收
当2代回收进行10次的时候就进行2代回收


