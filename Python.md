#### 什么是Python？

 Python是一种编程语言，它有对象、模块、线程、异常处理和自动内存管理，可以加入其他语言的对比。
 Python是一种==解释型==语言，Python在代码运行之前不需要解释。

 Python适合==面向对象==的编程，因为它支持通过组合与继承的方式定义类。

 Python是动态类型语言，在声明变量时，不需要说明变量的类型。

 在Python语言中，函数是第一类对象。
 Python用途广泛，常被用走"胶水语言"，可帮助其他语言和组件改善运行状况。
 使用Python，程序员可以专注于算法和数据结构的设计，而不用处理底层的细节。

#### Python 对象三要素

==在 Python 中，对象包含的三个基本要素，分别是：id（标识）、type（数据类型）和 value（值）==



#### ==os==

**os.listdir()**	用于返回指定的文件夹包含的文件或文件夹的名字的列表。（*只支持在 Unix, Windows 下使用。*）

**os.path.isdir(path)** 	判断路径是否为文件

###### os.path 和sys.path 分别==代表==什么

os.path主要是用于对系统路径文件的操作。
sys.path主要是对Python解释器的系统环境参数的操作（动态的改变Python解释器搜索路径）。



```python
现有字典 d={‘a’:24，‘g’:52，‘i’:12，‘k’:33}请按字典中的 value值进行排序？ (2018-3-30-lxy)

d={'a':24,'b':52,'i':32,'k':22}
sorted(d.items(),key = lambda x:x[1])
```

#### ==__slots__==

我们想要限制实例的属性

**Python允许在定义class的时候，定义一个特殊的`__slots__`变量，来限制该class实例能添加的属性：**

```python
class Student(object):
    __slots__ = ('name', 'age') # 用tuple定义允许绑定的属性名称
```



## Python 常用内建模块

### collections

##### **nametuple** 

`namedtuple`是一个函数，它用来创建一个自定义的`tuple`对象，并且规定了`tuple`元素的个数，并可以用属性而不是索引来引用`tuple`的某个元素。 

```python 
>>> from collections import namedtuple
>>> Point = namedtuple('Point', ['x', 'y'])
>>> p = Point(1, 2)
>>> p.x
1
>>> p.y
2
```

类似的，如果要用坐标和半径表示一个圆，也可以用`namedtuple`定义：

```python 
# namedtuple('名称', [属性list]):
Circle = namedtuple('Circle', ['x', 'y', 'r'])
```



##### **deque**

deque是为了高效实现插入和删除操作的双向列表，适合用于队列和栈：

```python
>>> from collections import deque
>>> q = deque(['a', 'b', 'c'])
>>> q.append('x')
>>> q.appendleft('y')
>>> q
deque(['y', 'a', 'b', 'c', 'x'])
```



##### **defaultdict**

使用`dict`时，如果引用的Key不存在，就会抛出`KeyError`。如果希望key不存在时，返回一个默认值，就可以用`defaultdict`：

```python 
>>> from collections import defaultdict
>>> dd = defaultdict(lambda: 'N/A')
>>> dd['key1'] = 'abc'
>>> dd['key1'] # key1存在
'abc'
>>> dd['key2'] # key2不存在，返回默认值
'N/A'
```

注意默认值是调用函数返回的，而函数在创建`defaultdict`对象时传入。



##### **OrderdeDict**

使用`dict`时，Key是无序的。在对`dict`做迭代时，我们无法确定Key的顺序。

如果要保持Key的顺序，可以用`OrderedDict`：

```python
>>> from collections import OrderedDict
>>> d = dict([('a', 1), ('b', 2), ('c', 3)])
>>> d # dict的Key是无序的
{'a': 1, 'c': 3, 'b': 2}
>>> od = OrderedDict([('a', 1), ('b', 2), ('c', 3)])
>>> od # OrderedDict的Key是有序的
OrderedDict([('a', 1), ('b', 2), ('c', 3)])
```

 `OrderedDict`可以实现一个FIFO（先进先出）的dict，当容量超出限制时，先删除最早添加的Key： 

```python
from collections import OrderedDict

class LastUpdatedOrderedDict(OrderedDict):

    def __init__(self, capacity):
        super(LastUpdatedOrderedDict, self).__init__()
        self._capacity = capacity

    def __setitem__(self, key, value):
        containsKey = 1 if key in self else 0
        if len(self) - containsKey >= self._capacity:
            last = self.popitem(last=False)
            print 'remove:', last
        if containsKey:
            del self[key]
            print 'set:', (key, value)
        else:
            print 'add:', (key, value)
        OrderedDict.__setitem__(self, key, value)
```

##### **Counter**

Counter

`Counter`是一个简单的计数器，例如，统计字符出现的个数：

```python
>>> from collections import Counter
>>> c = Counter()
>>> for ch in 'programming':
...     c[ch] = c[ch] + 1
...
>>> c
Counter({'g': 2, 'm': 2, 'r': 2, 'a': 1, 'i': 1, 'o': 1, 'n': 1, 'p': 1})
```

`Counter`实际上也是`dict`的一个子类，上面的结果可以看出，字符`'g'`、`'m'`、`'r'`各出现了两次，其他字符各出现了一次。

## Python内存管理

##### 引用计数

>  python内部使用引用计数，来保持追踪内存中的对象，Python内部记录了对象有多少个引用，即引用计数，当对象被创建时就创建了一个引用计数，当对象不再需要时，这个对象的引用计数为0时，它被垃圾回收。 

```tex
总结一下对象会在一下情况下引用计数'加1'：
1.对象被创建：x=4
2.另外的别人被创建：y=x
3.被作为参数传递给函数：foo(x)
4.作为容器对象的一个元素：a=[1,x,'33']

引用计数减少情况
1.一个本地引用离开了它的作用域。比如上面的foo(x)函数结束时，x指向的对象引用减1。
2.对象的别名被显式的销毁：del x ；或者del y
3.对象的一个别名被赋值给其他对象：x=789
4.对象从一个窗口对象中移除：myList.remove(x)
5.窗口对象本身被销毁：del myList，或者窗口对象本身离开了作用域。
```

##### 垃圾回收

 1、当内存中有不再使用的部分时，垃圾收集器就会把他们清理掉。**它会去检查那些引用计数为0的对象**，然后清除其在内存的空间。 

 2、垃圾回收机制还有一个**循环垃圾回收器**, 确保释放循环引用对象(a引用b, b引用a, 导致其引用计数永远不为0)。 

##### 内存池机制

>  在Python中，许多时候申请的内存都是小块的内存，这些小块内存在申请后，很快又会被释放，由于这些内存的申请并不是为了创建对象，所以并没有对象一级的内存池机制。这就意味着Python在运行期间会大量地执行**malloc**和**free**的操作，频繁地在用户态和核心态之间进行切换，这将严重影响Python的执行效率。为了加速Python的执行效率，Python引入了一个内存池机制，用于管理对小块内存的申请和释放。 

- Python提供了对内存的垃圾收集机制，但是**它将不用的内存放到内存池**而不是返回给**操作系统**。

- Python中所有小于256个字节的对象都使用pymalloc实现的分配器，而大的对象则使用系统的 malloc。

  >  Pymalloc机制：为了加速Python的执行效率，Python引入了一个内存池机制，用于管理对小块内存的申请和释放。 

- 另外Python对象，如整数，浮点数和List，都有其独立的私有内存池，对象间不共享他们的内存池。也就是说如果你分配又释放了大量的整数，用于缓存这些整数的内存就不能再分配给浮点数。

[pymalloc](可在印象笔记查看此资料)

##### 代码--引用计数

```python
#引用计数器用法：
import sys
class Person(object):
    pass
p = Person()
p1 = p
print(sys.getrefcount(p))
p2 = p1
print(sys.getrefcount(p))
p3 = p2
print(sys.getrefcount(p))
del p1
print(sys.getrefcount(p))
```

