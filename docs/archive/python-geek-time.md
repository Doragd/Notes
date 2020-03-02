# Python核心技术与实战
!!! tip
    这里记录一下在极客时间买的Python课程的笔记

![python-graph](../images/python-graph.png)


## 匿名函数

### 匿名函数基础

```python
lambda argument1, argument2,... argumentN : expression
```
匿名函数 lambda 和常规函数一样，返回的都是一个函数对象（function object）
区别：

!!! notes
    lambda 是一个表达式（expression），并不是一个语句（statement）。所谓的表达式，就是用一系列“公式”去表达一个东西。而所谓的语句，则一定是完成了某些功能。lambda 的主体是只有一行的简单表达式，并不能扩展成一个多行的代码块。之所以发明 lambda，就是为了让它和常规函数各司其职：lambda 专注于简单的任务，而常规函数则负责更复杂的多行逻辑。使用匿名函数 lambda，可以帮助我们大大简化代码的复杂度，提高代码的可读性。

  * lambda 可以用在一些常规函数 def 不能用的地方，比如，lambda 可以用在列表内部，而常规函数却不能
    ```python
    [(lambda x: x*x)(x) for x in range(10)]
    ```
  * lambda 可以被用作某些函数的参数，而常规函数 def 也不能。
    ```python
    l.sort(key=lambda x: x[1]) # 按列表中元祖的第二个元素排序
    ```
  * 常规函数 def 必须通过其函数名被调用，因此必须首先被定义。但是作为一个表达式的 lambda，返回的函数对象就不需要名字了。 

### 函数式编程

!!! notes "函数式编程"
    函数式编程，是指代码中每一块都是不可变的（immutable），都由纯函数（pure function）的形式组成。这里的纯函数，是指函数本身相互独立、互不影响，对于相同的输入，总会有相同的输出，没有任何副作用。(比如有些就地操作改变了原列表，产生了副作用)

```python

def multiply_2(l): # l会被改变，多次运行函数返回值都不同
    for index in range(0, len(l)):
        l[index] *= 2
    return l


def multiply_2_pure(l): # 纯函数式
    new_list = []
    for item in l:
        new_list.append(item * 2)
    return new_list
```

函数式编程的优点，主要在于其纯函数和不可变的特性使程序更加健壮，易于调试（debug）和测试；缺点主要在于限制多，难写。

!!! warning "必须掌握的"
    Python 主要提供了这么几个函数：`map()`、`filter()` 和 `reduce()`，通常结合匿名函数 lambda 一起使用。

`map(function, iterable)` : 对 iterable 中的每个元素，都运用 function 这个函数，最后返回一个新的可遍历的集合.

```python
l = [1, 2, 3, 4, 5]
new_list = map(lambda x: x * 2, l) # [2， 4， 6， 8， 10]
```
 `map()` 函数直接由 C 语言写的，运行时不需要通过 Python 解释器间接调用，并且内部做了诸多优化，所以运行速度快（相比for循环和列表生成式）

 `filter(function, iterable)` : 对 iterable 中的每个元素，都使用 function 判断，并返回 True 或者 False，最后将返回 True 的元素组成一个新的可遍历的集合

 ```python
l = [1, 2, 3, 4, 5]
new_list = filter(lambda x: x % 2 == 0, l) # [2, 4]
 ```

`reduce(function, iterable)` : 对 iterable 中的每个元素以及上一次调用后的结果，运用 function 进行计算，所以最后返回的是一个单独的数值

```python
l = [1, 2, 3, 4, 5]
product = reduce(lambda x, y: x * y, l) # 1*2*3*4*5 = 120
```

  

## 自定义函数

### 函数基础
* Python函数不用考虑输入的数据类型，这种特性是多态
* Python还支持函数的嵌套

!!! notes "函数嵌套作用"
    函数的嵌套能够保证内部函数的隐私。内部函数(有一些隐私数据（比如数据库的用户、密码等）)只能被外部函数所调用和访问，不会暴露在全局作用域, 不能直接在外面被调用。
    ```python
    def connect_DB():
        def get_DB_configuration():
            ...
            return host, username, password
        conn = connector.connect(get_DB_configuration())
        return conn
    ```
    合理的使用函数嵌套，能够提高程序的运行效率



* 在函数内部调用其他函数，函数间哪个声明在前、哪个在后就无所谓，因为 def 是可执行语句，函数在调用之前都不存在，我们只需保证调用时，所需的函数都已经声明定义:
```python

def my_func(message):
    my_sub_func(message) # 调用my_sub_func()在其声明之前不影响程序执行
    
def my_sub_func(message):
    print('Got a message: {}'.format(message))

my_func('hello world')
```

#### 函数变量作用域

变量是在函数内部定义的，就称为局部变量，只在函数内部有效。一旦函数执行完毕，局部变量就会被回收，无法访问。全局变量则是定义在整个文件层次上的。

!!! warning "global/nonlocal" 
    全局变量，可以在文件内的任何地方被访问，当然在函数内部也是可以的。不过，我们不能在函数内部随意改变全局变量的值。如果我们一定要在函数内部改变全局变量的值，就必须加上 `global` 这个声明。对于嵌套函数来说，内部函数可以访问外部函数定义的变量，但是无法修改，若要修改，必须加上 `nonlocal` 这个关键字

!!! warning "同名情况"
    遇到函数内部局部变量和全局变量同名的情况，那么在函数内部，局部变量会覆盖全局变量。如果不加上 `nonlocal` 这个关键字，而内部函数的变量又和外部函数变量同名，那么同样的，内部函数变量会覆盖外部函数的变量。

#### 闭包

闭包（closure）: 闭包其实和刚刚讲的嵌套函数类似，不同的是，**这里外部函数返回的是一个函数，而不是一个具体的值。**返回的函数通常赋于一个变量，这个变量可以在后面被继续执行调用。

```python
# 闭包
def nth_power(exponent):
    def exponent_of(base):
        return base ** exponent
    return exponent_of # 返回值是exponent_of函数

# 嵌套函数
def nth_power(exponent, base):
    def exponent_of(exponent, base):
        return base ** exponent
    return exponent_of(exponent, base) # 返回值是exponent_of计算结果

# 闭包
square = nth_power(2)
res = square(4) # 4^2
res2 = square(6) # 6^2

# 嵌套函数
res = nth_power(2, 4)
res2 = nth_power(2, 6)

```

!!! warning "为什么使用闭包"
    函数开头需要做一些额外工作，而你又需要多次调用这个函数时，将那些额外工作的代码放在外部函数，就可以减少多次调用导致的不必要的开销，提高程序的运行效率。同时闭包常和装饰器一起使用

### 总结
* 嵌套函数的使用，能保证数据的隐私性，提高程序运行效率
* 合理地使用闭包，则可以简化程序的复杂度，提高可读性


## 异常处理

### 捕捉指定类型异常和所有异常

```python
try:
    s = input('please enter two numbers separated by comma: ')
    num1 = int(s.split(',')[0].strip())
    num2 = int(s.split(',')[1].strip())
except ValueError as err:
    print('Value Error: {}'.format(err))
except IndexError as err:
    print('Index Error: {}'.format(err))
except (ValueError, IndexError) as err: 
    print('Error: {}'.format(err))
except Exception as err:
    pass
except:
    pass

print('continue')
```
!!! warning
    这里是必须异常类型匹配才能进行异常处理程序，更通常的做法，是在最后一个 except block，声明其处理的异常类型是 Exception。Exception 是其他所有非系统异常的基类，能够匹配任意非系统异常。也可以在 except 后面省略异常类型，这表示与任意异常相匹配（包括系统异常等）

### finally 使用

异常处理中，还有一个很常见的用法是 finally，经常和 try、except 放在一起来用。无论发生什么情况，finally block 中的语句都会被执行，哪怕前面的 try 和 excep block 中使用了 return 语句。

典型应用场景是读取文件，对于文件的读取，我们也常常使用 with open，你也许在前面的例子中已经看到过，with open 会在最后自动关闭文件，让语句更加简洁

```python

import sys
try:
    f = open('file.txt', 'r')
    .... # some data processing
except OSError as err:
    print('OS error: {}'.format(err))
except:
    print('Unexpected error:', sys.exc_info()[0])
finally:
    f.close()
```

### 用户自定义异常

```python

class MyInputError(Exception):
    """Exception raised when there're errors in input"""
    def __init__(self, value): # 自定义异常类型的初始化
        self.value = value
    def __str__(self): # 自定义异常类型的string表达形式
        return ("{} is invalid input".format(repr(self.value)))
    
try:
    raise MyInputError(1) # 抛出MyInputError这个异常
except MyInputError as err:
    print('error: {}'.format(err))
# error: 1 is invalid input
```

### 总结

* 异常，通常是指程序运行的过程中遇到了错误，终止并退出。我们通常使用 try except 语句去处理异常，这样程序就不会被终止，仍能继续执行。
* 处理异常时，如果有必须执行的语句，比如文件打开后必须关闭等等，则可以放在 finally block 中。

!!! warning
    异常处理，通常用在你不确定某段代码能否成功执行，也无法轻易判断的情况下，比如数据库的连接、读取等等。正常的 flow-control 逻辑，不要使用异常处理，直接用条件语句解决就可以了。

## 字典、集合，你真的了解吗？

### 基础知识
* 字典是一系列由键（key）和值（value）配对组成的元素的集合
* 字典属于集合，但是集合没有键和值的配对，是一系列无序的、唯一的元素组合
* 字典访问: 可以使用`get(key, default)`函数来进行索引。如果键不存在，调用 `get()`函数可以返回一个默认值
* 集合并不支持索引操作，因为集合本质上是一个哈希表
* 集合/字典是高度优化的哈希表，里面元素不能重复，并且其添加和查找操作只需 $O(1)$的复杂度, 而列表达不到。
* `dis`模块可以分析python字节码
  
!!! warning
    * 在 Python3.7+，字典被确定为有序
    * 在 3.6 中，字典有序是一个`implementation detail`，在 3.7 才正式成为语言特性，无法 100% 确保其有序性
    * 3.6 之前是无序的，其长度大小可变，元素可以任意地删减和改变。

## 列表和元组，到底用哪一个？

### 基础知识

* 相同点: 列表和元组，都是一个可以放置任意数据类型的有序集合，都支持负数索引，都支持切片操作，可以随意嵌套，可以通过 `list()` 和 `tuple()` 函数相互转换。
* 不同点: 列表是动态的，长度大小不固定，可以随意地增加、删减或者改变元素（mutable）。而元组是静态的，长度大小固定，无法增加删减或者改变（immutable）。

### 列表和元组存储方式的差异

```python
l = [1, 2, 3]
l.__sizeof__() # 64
tup = (1, 2, 3)
tup.__sizeof__() # 48


l = []
l.__sizeof__() # 空列表的存储空间为40字节
40
l.append(1)
l.__sizeof__() 
72 # 加入了元素1之后，列表为其分配了可以存储4个元素的空间 (72 - 40)/8 = 4
l.append(2) 
l.__sizeof__()
72 # 由于之前分配了空间，所以加入元素2，列表空间不变
l.append(3)
l.__sizeof__() 
72 # 同上
l.append(4)
l.__sizeof__() 
72 # 同上
l.append(5)
l.__sizeof__() 
104 # 加入元素5之后，列表的空间不足，所以又额外分配了可以存储4个元素的空间
```
* 用`__sizeof__()`可以看对象占有的空间(字节)。
* 列表不同于元组，列表是动态的，所以它需要存储指针，来指向对应的元素（上述例子中，对于 int 型，8 字节）。另外，由于列表可变，所以需要额外存储已经分配的长度大小（8 字节）。
* 在列表增删元素时，为了减小每次增加 / 删减操作时空间分配的开销，Python 每次分配空间时都会额外多分配一些，这样的机制（over-allocating）保证了其操作的高效性：增加 / 删除的时间复杂度均为 $O(1)$
* 元组长度大小固定，元素不可变，所以存储空间固定

### 列表和元组的性能
* 元组要比列表更加轻量级一些，所以总体上来说，元组的性能速度要略优于列表。你想要增加、删减或者改变元素，那么列表显然更优，因为元组要新建一个来完成。
* Python 会在后台，对静态数据做一些资源缓存（resource caching）。通常来说，因为垃圾回收机制的存在，如果一些变量不被使用了，Python 就会回收它们所占用的内存，返还给操作系统，以便其他变量或其他应用使用
* 对于一些静态变量，比如元组，如果它不被使用并且占用空间不大时，Python 会暂时缓存这部分内存。这样，下次我们再创建同样大小的元组时，Python 就可以不用再向操作系统发出请求，去寻找内存，而是可以直接分配之前缓存的内存空间，这样就能大大加快程序的运行速度。

### 总结
* 列表和元组都是有序的，可以存储任意数据类型的集合。
* 列表是动态的，长度可变，可以随意的增加、删减或改变元素。列表的存储空间略大于元组，性能略逊于元组。
* 元组是静态的，长度大小固定，不可以对元素进行增加、删减或者改变操作。元组相对于列表更加轻量级，性能稍优。
* 使用场景主要看存储的数据是否可变动态
* Python 源码学习
    * list: https://github.com/python/cpython/blob/master/Objects/listobject.c. 
    * tuple: https://github.com/python/cpython/blob/master/Objects/tupleobject.c


!!! warning "用`list()`还是 `[]`"

    主要在于`list()`是一个`function call`，Python的`function call`会创建stack，并且进行一系列参数检查的操作，比较expensive，反观`[]`是一个内置的C函数，可以直接被调用，因此效率高。



