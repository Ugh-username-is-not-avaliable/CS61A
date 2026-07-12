# 部分课程笔记

> **记笔记的精髓在于——自己看得懂。**
> 
> <div align="right">——鲁迅（？）</div>
***


## 一些零碎
1. 调用时，普通函数会先计算所有参数：
> eg. if (True, 1, 1/0)这样的表述会报错
原理：参数求值（先算出参数，再调用函数）

2. 短路求值：
> eg. false and something()   
> python根本不会调用something()

3. 错有三种：
    1. 语法错误；
    2. 运行时的错误（由解释器检测到的错误）← Traceback会显示的错误；
    3. 逻辑或行为错误（程序运行，但做了错误的行为）。

4. python查找变量的规则：
    
    - Local → Enclsongsing → Global

5. 赋值语句总是改变current frame中的东西（源于``horse(mask)``和``mask(horse)``<sub>写出来真是太麻烦了！详见笔记本</sub>）
6. 元组：一个不可变的序列。
    - 如果不可变序列包含可变值作为元素，则仍可能会发生变化，例如：  
    `t = (1,2)`是元组，不可变；  
    但是``t = ([1], [2])``因为列表可变，所以``t[0].append(100)``可将该元组变为``t = ([1, 100], [2])``
7. 默认参数：调用时不给参数则默认使用的那个参数，例如：

        def greet(name="World"):
            print("Hello,", name)
    这段代码中，``name="World"``就是默认参数。
8. 要注意默认参数（结合第6条，列表是**可变的**，当**可变对象**（list, dict, set）为默认参数时需要注意）  
eg.:

        def f(s=[]):
            s.append(5)
            return len(s)

        >>> f()
        1
        >>> f()
        2
    每次调用列表s都会被append，这是因为默认参数保存的是这个列表本身，而不是在调用函数时重新创建列表。
9. 如果在函数外创建一个列表s = []，并不会影响默认参数的这个列表，因为内存地址不同：
    ```
    参数s       变量s
    ↓           ↓
    List A     List B
    []          []
    ```

## Python语法零碎
1. 运行Python文件的方法：
    - 在命令符中使用``python filename.py``代码执行完毕后会返回到命令行界面。如果文件只包含函数定义，没有语法错误就不会有任何输出。
    - 在命令符中使用``python -i filename.py``会打开一个交互式Python会话（显示``>>>``提示符），可以执行表达式，``Ctrl+z Enter``可以退出。
    - 在命令符中使用``python -m doctest filename.py``可以运行文件中的示例代码。
2. ``return 'x'``会返回``'x'``，而``print('x')``是打印``x``。


## Higher Oder Functions
- Functions are first class: 函数时第一类值，意味着它们可以作为参数传递，可以作为返回值返回。
- 高阶函数：可接受另一个函数作为参数，或返回一个函数。

- 每个用户定义的函数都有一个母框架
    1. 全局框架
    2. 外部函数

- 柯里化：将多参数函数转换为一个单参数高阶函数的行为，该函数返回一个接受其余参数的函数。
- lambda函数的母级是lambda函数被评估的当前环境（当前框架）→ lambda会记住*定义时*的环境，而不是*调用时*的环境。 <sub>**我觉得我可能还需要返回去看看这一段（看完删这句话）**</sub>

## 装饰器

在不修改函数内部代码的情况下，给函数增加额外功能。

    @ trace 1
    def triple (x):
        return 3 * x

和

    def triple (x):
        return 3 * x
    triple = trace 1 (trple)

一样  

即*函数包装函数*  
<sub>记的不是很熟，但也没用过，到时候可以回看这一部分复习复习。（看了删掉这句话）</sub>

## 递归、迭代
> 哦，这真是重中之重！
>
> <div align="right">——鲁迅</div>

- 递归函数：其主体要么直接调用自身，要么间接调用自身。 
- 递归函数将以一个检查基本情况的条件语句开始
    - 基本情况：尝试解决问题的非常简单的版本（问题的最简单形式！）

- 递归函数的组成：  
    1. 验证基本情况；
    2. 把函数当功能抽象来对待（不考虑它是如何实现的，只考虑它做什么）；
    3. 假设n-1的情况是正确的；
    4. 通过假设n-1是正确的，来验证n的情况。  
    （3和4源于一点数学归纳法）

- 迭代是一种特殊的递归。
- 查看一个迭代实现时，要找的是在不同迭代之间保持的状态，然后将其作为参数传递。

eg. : 
一个使用``while``循环的函数：

    def sum_digits_iter(n):
        digit_sum = 0
        while n > 0:
            n, last = split(n)
            digit_sum = digit_sum + last
        return digit_sum

在这里，保持状态的时n和数字总和（也就是``digit_sum``），在改为递归时，应该让它们被传递。  
使用递归书写这个函数：

    def sum_digits_rec(n, digit_sum):
        if n == 0:
            return digit_sum
        else:
            n, last = split(n)
            return sum_digits_rec(n, digit_sum + last)

在执行函数体时，到达函数体结尾而未看到返回语句（即``return``），则总是返回``None``。


    def cascade(n):
        if n < 10:
            print(n)
        else: 
            print(n)
            cascade(n//10)
            print(n)

此函数的效果是：

    >>> cascade(1245)
    1245
    124
    12
    1
    12
    124
    1245

没有``return``语句，在``cascade(n//10)``执行完后再执行最后一个``print(n)``，这时参数``n``从递归最后一层向上返回。

- 树形递归：产生树状过程，每当执行递归函数的主体使得对该函数的调用超过一次  

经典的线性递归（阶乘）：

    def factorial(n):
        if n == 1:
            return 1
        return factorial(n - 1) * n

线性递归示意图：
```
factorial(4)
     |
factorial(3)
     |
factorial(2)
     |
factorial(1)
     |
 return 1
     ↑
 return 2
     ↑
 return 6
     ↑
return 24
 ```
 经典的树形递归（斐波那契数列）：

    def fib(n):
        if n == 0:
            return 0
        if n == 1:
            return 1
        return fib(n - 1) + fib(n - 2)

示意图：
```
                fib(3)
               /      \
          fib(2)      fib(1)
          /    \
     fib(1)   fib(0)
```

记录一个案例：
- Counting Partitions:
    - 正整数n使用大小不超过m的部分进行分区的方式有多少种就是n能够以不同方式表示未不超过m的部分的递增顺序之和  

代码如下：

    def count_partitions(n, m):
        if n == 0:
            return 1
        elif n < 0:
            return 0
        elif m == 0:
            return 0
        else:
            with_m = count_partitions(n-m, m)
            without_m = count_partition(n, m-1)
            return with_m + without_m

- for循环：

        for <name> in <expression>:
            <suite>
1. 标题表达式``<expression>``必须产生一个**可迭代**的值；
2. 对序列中的每个元素，把``<name>``绑定到当前帧的元素上，执行``<suite>``。       

示例：

    def cheer():
        for _ in range (3):
            print('Go Bears')

执行起来，``print``会执行三次：

    >>> cheer()
    Go Bears
    Go Bears
    Go Bears

---

- 字典：
    1. 字典是键值对的集合；
    2. 字典的键不能是列表或字典；
    3. 两个键不能相等。

字典推导式：

    {<key exp>: <value exp> for <name> in <iter exp> of <filter exp>}

示例：

    {x: x * x for x in range(5)}
等价于：

    d = {}
    for x in range(5):
        d[x] = x * x
得到：

    {
        0: 0,
        1: 1,
        2: 4,
        3: 9,
        4: 16
    }

而``if``作为筛选：

    {x: x * x for x in range(10) if x % 2 == 0}

得到：

    {
        0: 0,
        2: 4,
        4: 16,
        6: 36,
        8: 64
    }

字典推导式short version: 

    {<key exp>: <value exp> for <name> in <iter exp>}
即去掉``if``的筛选部分。
---
### 递归类别整理
```
递归（Recursion）
│
├── 按调用次数分类
│   ├── 线性递归（Linear Recursion）
│   └── 树形递归（Tree Recursion）
│
├── 按建立结果的方法分类
│   ├── 返回值递归（Build on Return Value）
│   └── 参数累积递归（Accumulator）
│
└── 按递归调用的位置分类
    ├── 尾递归（Tail Recursion）
    └── 非尾递归（Non-tail Recursion）
```
- 尾递归：递归调用是函数执行的最后一件事。  
例如：

        def fact_times(n, k):
            if n == 0:
                return k
            return fact_times(n-1, k*n)
    递归是最后一步
- 非尾递归的例子：

        def factorial(n):
            if n == 1:
                return 1
            return factorial(n - 1) * n
    递归后还需要进行运算
## 数据抽象

- 数据抽象：一种方法论，函数通过其强制在数据的表示和使用之间建立一个抽象**屏障**。
- 数据抽象使用**选择器**和**构造函数**定义行为。
    - 构造函数和选择器必须是彼此的**补充**。
    - 说点人话：数据抽象不要求使用者知道数据的内部表示，而是通过构造函数（创建数据）和选择器（读取数据）来操作数据。

| part of the program that…… | treat rationals as…… | using…… |
| --- | --- | --- |
| use rational numbers to perform computation| whole data values | add_rational, mul_rational…… （操作数值的函数）|
| create rationals or implement rational oerations | numerations and denominators | rational, numer, denom （rational的构造）|
|implement selectors and constructor for rationals | two-element lists | list literals and element selections |
---
<sub>注：该表格源于课程示例，如果时间久远看不懂了，可以回看！</sub>  
> 每一级做每一层级的事情，上层无需直到底层的数据结构。
---
#### 插播一点递归：
- 有些递归函数通过操纵递归调用的返回值建立起它们的结果，而其她递归函数则通过将信息作为参数传递到递归调用中来建立它们的结果。
    - 把当前层的工作留到递归返回以后再做；
    - 把当前层的工作提前做完，并通过参数传给下一层。

以阶乘为例：

    def fact(n):
        if n == 0:
            return 1
        else:
            return n * fact(n-1)

这个阶乘函数会在递归返回后再进行计算。  
计算过程如下：

    fact(4)
    ↓
    fact(3)
    ↓
    fact(2)
    ↓
    fact(1)
    ↓
    return 1
    ↑
    return 2*1
    ↑
    return 3*2*1
    ↑
    return 4*3*2*1

下面是一个k倍阶乘的例子：

    def fact_times(n, k):
        if n == 0 
            return k
        else:
            return fact_times(n-1, k *n)

这个函数会把当前层的工作做完，并且把结果通过参数传递给下一层，最后直接返回答案，不需要再进行计算。  
计算过程如下：

    fact_times(4, 1)
        ↓
    fact_times(3, 4*1)
        ↓
    fact_times(2, 3*4*1)
        ↓
    fact_times(1, 2*3*4*1)
        ↓
    fact_times(0, 1*2*3*4*1)
        ↓
    return 24

---

#### 生成器
- 生成器函数：一种产生值而不是返回值的函数。   

在编写程序时，如果只想要其中的一些可能，就可以使用yield。

    yield from generator(x)
            ||
    for n in generator(x):
        yield n
---

