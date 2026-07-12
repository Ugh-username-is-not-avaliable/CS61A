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

### homework 3
*date: 2026/7/10*

<sub>碎碎念一下，好像过去有些久了呢……之前跑去读C#写星露谷Mod了，感觉自己还是得学，所以回来继续写CS61A作业了。</sub>

### Q1：

错误第一版：
1. ``num_eights(0)``会一直递归，只约束了``count_eight(a)``参数为0时的情况，但却没有约束``num_eights(0)``，所以无限递归了。


    def num_eights(n):
        """Returns the number of times 8 appears as a digit of n.

        >>> num_eights(3)
        0
        >>> num_eights(8)
        1
        >>> num_eights(88888888)
        8
        >>> num_eights(2638)
        1
        >>> num_eights(86380)
        2
        >>> num_eights(12345)
        0
        >>> num_eights(8782089)
        3
        >>> from construct_check import check
        >>> # ban all assignment statements
        >>> check(HW_SOURCE_FILE, 'num_eights',
        ...       ['Assign', 'AnnAssign', 'AugAssign', 'NamedExpr', 'For', 'While'])
        True
        """
        "*** YOUR CODE HERE ***"
        def count_eight(a):
            if a % 10 == 8:
                return 1
            else:
                return 0
        return num_eights(n//10) + count_eight(n)

改后答案：


    def num_eights(n):
        def count_eight(a):
            if a % 10 == 8:
                return 1
            else:
                return 0
        if n == 0:
            return 0   
        return num_eights(n//10) + count_eight(n)

### Q3:

错误第一版：
1. 没有分清``if``和``elif``的区别，如果是
    if i < n:
        ……
        i += 2
        ……
    if i == n:
        ……

在i经过第一个if块之后，它有可能**直接进入**下一个if块，从而提前返回。这里应该用``elif``以表示三个条件是同时判断的;
2. 直接写``return``后面什么都不跟，返回的就是None，而None不属于int类型。


    def interleaved_sum(n, odd_func, even_func):
        """Compute the sum odd_func(1) + even_func(2) + odd_func(3) + ..., up
        to n.

        >>> identity = lambda x: x
        >>> square = lambda x: x * x
        >>> triple = lambda x: x * 3
        >>> interleaved_sum(5, identity, square) # 1   + 2*2 + 3   + 4*4 + 5
        29
        >>> interleaved_sum(5, square, identity) # 1*1 + 2   + 3*3 + 4   + 5*5
        41
        >>> interleaved_sum(4, triple, square)   # 1*3 + 2*2 + 3*3 + 4*4
        32
        >>> interleaved_sum(4, square, triple)   # 1*1 + 2*3 + 3*3 + 4*3
        28
        >>> from construct_check import check
        >>> check(HW_SOURCE_FILE, 'interleaved_sum', ['While', 'For', 'Mod']) # ban loops and %
        True
        """
        "*** YOUR CODE HERE ***"
        def part(i, n):
            if i < n:
                k = odd_func(i) + even_func(i+1)
                i += 2
                sum = k + part(i,n)
            if i == n:
                k = odd_func(i)
                return k
            if i == (n + 1):
                return
            return sum
        return part(1, n)

改后答案：


    def interleaved_sum(n, odd_func, even_func):
        def part(i, n):
            if i < n:
                k = odd_func(i) + even_func(i+1)
                i += 2
                sum = k + part(i,n)
            elif i == n:
                k = odd_func(i)
                return k
            elif i == (n + 1):
                return 0
            return sum
        return part(1, n)

### Q4: 

第一版错误：
1. 算出的结果是考虑了硬币重复使用的结果，然而题目要的是不重复使用的；
2. 这里没有选择传递coin，如果想要在执行过程中保存某种状态，则需要一个参数传递它的状态。

        def count_coins(total):
            """Return the number of ways to make change using coins of value of 1, 5, 10, 25.
            >>> count_coins(15)
            6
            >>> count_coins(10)
            4
            >>> count_coins(20)
            9
            >>> count_coins(100) # How many ways to make change for a dollar?
            242
            >>> count_coins(200)
            1463
            >>> from construct_check import check
            >>> # ban iteration
            >>> check(HW_SOURCE_FILE, 'count_coins', ['While', 'For'])
            True
            """
            "*** YOUR CODE HERE ***"
            coin = 25
            if total == 0:
                return 1
            elif total < 0:
                return 0
            else:
                return count_coins(total-coin) + count_coins(total-next_smaller_coin(coin)) + count_coins(total-next_smaller_coin(next_smaller_coin(coin))) + count_coins(total-next_smaller_coin(next_smaller_coin(next_smaller_coin(coin))))

第二版错误：
1. 尝试第一个数值15，函数会变成：``（15，5）→（5，5）→（0，5）``而实际上还有一种情况``（15，5）→（5，5）→（5，1）``是成立的，这是因为我写的函数在最开始用``n = n - coin``用余额判断条件是否成立，忽略了余额刚好等于某硬币面值时的其她可能。

        def count_coins(total):
            def count_with_coins(n, coin):
                n = n - coin
                if n == 0:
                    return 1
                elif n < 0:
                    return 0
                if coin == 1:
                    k = count_with_coins(n, coin)
                else:
                    k = count_with_coins(n, coin) + count_with_coins(n, next_smaller_coin(coin))
                return k
            return count_with_coins(total, 25) + count_with_coins(total, 10) + count_with_coins(total, 5) + count_with_coins(total, 1)

第三版错误：
1. 虽然我已经知道了正确答案，但是它和我想法不一样，我要看看我错哪了；
2. 虽然有的时候错在坚持错误答案，但在我没找到我错哪之前我都是对的；
3. 递归树的节点变得**不唯一**了，且在``n = n - coin``后的n表示被消耗的**余额**，而如果在``n = n - coin``之前判断，那么会出现n的定义变化的情况。

        def count_coins(total):
            def count_with_coins(n, coin):
                if n == coin & coin != 1:
                    return 1 + count_with_coins(n, next_smaller_coin(coin))
                n = n - coin
                if n == 0:
                    return 1
                elif n < 0:
                    return 0
                if coin == 1:
                    k = count_with_coins(n, coin)
                else:
                    k = count_with_coins(n, coin) + count_with_coins(n, next_smaller_coin(coin))
                return k
            return count_with_coins(total, 25) + count_with_coins(total, 10) + count_with_coins(total, 5) + count_with_coins(total, 1)

最后我选择了放弃……总结一下错误思路：  
*递归分了两个维度，但这两个维度未必是**正交**的。*

标准答案：
把选择作为递归分支：用这个硬币；不用硬币并且进入更小的硬币：
1. 递减的方式同样可以避免重复使用；
2. 设计状态为（当前目标，当前允许选择范围）；
3. 分支为：不用当前选择 → 选择范围变小；用当前选择 → 目标减少，选择范围不变。

        def constrained_count(total, smallest_coin):
            def constrained_count_small(total, largest_coin):
                    if total == 0:
                        return 1
                    if total < 0:
                        return 0
                    if largest_coin == None:
                        return 0
                    without_coin = constrained_count_small(total, next_smaller_coin(largest_coin))
                    with_coin = constrained_count_small(total - largest_coin, largest_coin)
                    return without_coin + with_coin
                return constrained_count_small(total, 25)
