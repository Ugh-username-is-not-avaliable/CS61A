# CS61A

My CS61A learning journey.

Continuing ...

## Homework

### homework 1
*date: 2026/6/15*

#### Q1:

错把`f`**函数**当成**数值**了：

    def a_plus_abs_b(a, b):
        """Return a+abs(b), but without calling abs.

        >>> a_plus_abs_b(2, 3)
        5
        >>> a_plus_abs_b(2, -3)
        5
        >>> a_plus_abs_b(-1, 4)
        3
        >>> a_plus_abs_b(-1, -4)
        3
        """
        if b < 0:
            f = a - b
        else:
            f = a + b
        return f(a,b)

正确的：

    def a_plus_abs_b(a, b):
        if b < 0:
            f = sub
        else:
            f = add
        return f(a, b)  

**注：**比较规范的是f(a, b)，多一个空格，而不是f(a,b)但这不影响python代码

### Q4:

1. 注意整数和浮点数的问题：`/`会让数字变成**浮点数**，而`//`才是**整数**；
2. 注意是否**展示**答案，也就是说题目到底要的是什么（遗忘了print）。

    def hailstone(n):
        """Print the hailstone sequence starting at n and return its
        length.

        >>> a = hailstone(10)
        10
        5
        16
        8
        4
        2
        1
        >>> a
        7
        >>> b = hailstone(1)
        1
        >>> b
        1
        """
        "*** YOUR CODE HERE ***"
        i = 1
        while n > 1:
            if n % 2 == 0:
                i += 1
                n = n / 2
            else:
                n = n * 3 + 1
                i += 1
        return i

**没有print！！**

正确的：

    def hailstone(n):
        i = 1
        print(n)
        while n > 1:
            if n % 2 == 0:
                i += 1
                n = n // 2
                print(n)
            else:
                n = n * 3 + 1
                i += 1
                print(n)
        return i

### homework 1 能跑但却不够简洁
*date: 2026/6/18*

#### Q3：
我的：

    def largest_factor(n):
        """Return the largest factor of n that is smaller than n.

        >>> largest_factor(15) # factors are 1, 3, 5
        5
        >>> largest_factor(80) # factors are 1, 2, 4, 5, 8, 10, 16, 20, 40
        40
        >>> largest_factor(13) # factor is 1 since 13 is prime
        1
        """
        "*** YOUR CODE HERE ***"
        i = n - 1
        while i < n:
            if n % i == 0:
                return i
            elif i == 0:
                return 1
            else:
                i -= 1

能跑，但是很冗余：
1. ``while i < n``的条件很奇怪，这个条件其实是永远成立的，也就是说函数靠``return``退出;
2. ``elif i == 0``是多余的条件，因为``i = 1``的时候就会输出了。

标准答案：

    def largest_factor(n):
        factor = n - 1
        while factor > 0:
            if n % factor == 0:
                return factor
            factor -= 1

### homework 2
*date: 2026/6/18*

#### Q2:

1. ``return``会使循环终止
**提示：**在循环中如果有``return``，很可能是控制循环提前终止；
2. 注意if函数的条件。

    def accumulate(fuse, start, n, term):
        """Return the result of fusing together the first n terms in a sequence 
        and start.  The terms to be fused are term(1), term(2), ..., term(n). 
        The function fuse is a two-argument commutative & associative function.

        >>> accumulate(add, 0, 5, identity)  # 0 + 1 + 2 + 3 + 4 + 5
        15
        >>> accumulate(add, 11, 5, identity) # 11 + 1 + 2 + 3 + 4 + 5
        26
        >>> accumulate(add, 11, 0, identity) # 11 (fuse is never used)
        11
        >>> accumulate(add, 11, 3, square)   # 11 + 1^2 + 2^2 + 3^2
        25
        >>> accumulate(mul, 2, 3, square)    # 2 * 1^2 * 2^2 * 3^2
        72
        >>> # 2 + (1^2 + 1) + (2^2 + 1) + (3^2 + 1)
        >>> accumulate(lambda x, y: x + y + 1, 2, 3, square)
        19
        """
        "*** YOUR CODE HERE ***"
        i = n
        while i > 1:
            if n == 1:
                return fuse(start, term(1))
            else:
                i -= 1
                return fuse(term(i+1), term(i))

第二版：

    def accumulate(fuse, start, n, term):
        total = 0
        while n >= 0:
            if n == 0:
                total = fuse(term(0),start)
                return total
            else:
                total = fuse(total, term(n))
                n -= 1

问题：
1. 起点为``total = 0``, 要考虑这样会对``fuse(total, term(n))``造成什么样的影响（如果fuse是mul，则一直为0）；
2. 和题目要求的积累方向相反。

第三版：

    def accumulate(fuse, start, n, term):
        i = 0
        total = start
        while i <= n:
            if n == 0:
                total = total
            else:
                total = fuse(total, term(i))
                i += 1
        return total

问题：
1. 要注意``while``的**停止条件**;
2. 要注意``i``的起始值（是1还是0？）。

正确的版本：

    def accumulate(fuse, start, n, term):
        i = 1
        total = start
        while i <= n:
            if n == 0:
                return total
            else:
                total = fuse(total, term(i))
                i += 1
        return total

#### Q3:

错误第一版：
1. i在``g(x)``里，其实它永远不会变（把i变成了**局部变量**）；
2. 计算了``g(f(x))``但却没有返回值；



    def make_repeater(f, n):
        """Returns the function that computes the nth application of f.

        >>> add_three = make_repeater(increment, 3)
        >>> add_three(5)
        8
        >>> make_repeater(triple, 5)(1) # 3 * 3 * 3 * 3 * 3 * 1
        243
        >>> make_repeater(square, 2)(5) # square(square(5))
        625
        >>> make_repeater(square, 3)(5) # square(square(square(5)))
        390625
        """
        "*** YOUR CODE HERE ***"
        def g(x):
            i = 1
            if i == n:
                return f(x)
            else:
                g(f(x))
                i += 1

正确答案：
（其实和标准答案不一样）
1. 用``for i in range(n)``其实也可以不用计数器，不过好像差别不大；
2. 关键在于形成迭代调用，形成x→f(x)→f(f(x))这种递归形式，最简单的就是``x = f(x)``;
3. ``return g ``是返回函数，而``return g()`` 是返回函数的结果。


    def make_repeater(f, n):
        def g(x):
            for i in range(n):
                x = f(x)
            return x
        return g


