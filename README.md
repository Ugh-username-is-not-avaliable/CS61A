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


