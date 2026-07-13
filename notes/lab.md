## 一些值得留意的记录

### lab00 & lab01

    def how_big(x):
        if x > 10:
            print('huge')
        elif x > 5:
            return 'big'
        if x > 0:
            print('positive')
        else:
            print(0)
如果此处给一个大于10的``x``，则会执行``if x > 10``和``if x > 0``两个if语块，因为这里用的是print，不是return，第一个if判断结束后会进入下一个if语块。
如果没有``reutrn``，则在最终函数才会return一个``None``，而不是每次``print``都有一个None。
所以``print(how_big(12))``的结果是：
```
>>> print(how_big(12))
big
positive
None
```
返回两个``None``是同一行的：
```
>>> print(how_big(1), how_big(0))
positive
0
None None
```
### lab02
python的``and``和``or``运算符返回的并不是bool值，而是一个**操作数**，当and运算符左边为真值时，返回右边，而``or``运算符由于**短路**，左边为真时直接返回左边。
```
>>> True and 13
13
>>> -1 and 1 > 0
True
```
```
>>> False or 0
0
>>> -1 or 5
-1
```

题干大致要求：编写一个函数``count_cond``，接受一个双参数谓词函数``condition(n, i)``。``count_cond``会返回另一个**单参函数**，该函数接受参数n，并计算从1到n之间所有满足``condition``函数的数值个数。
第一版错误：  
1. 在这里，n没有被定义，n不是count_cond的参数，函数在调用时不知道n是谁传入的参数，而且题目要求返回一个接受参数n的函数。

        def count_cond(condition):
            """Returns a function with one parameter N that counts all the numbers from
            1 to N that satisfy the two-argument predicate function Condition, where
            the first argument for Condition is N and the second argument is the
            number from 1 to N.

            >>> count_fives = count_cond(lambda n, i: sum_digits(n * i) == 5)
            >>> count_fives(10)   # 50 (10 * 5)
            1
            >>> count_fives(50)   # 50 (50 * 1), 500 (50 * 10), 1400 (50 * 28), 2300 (50 * 46)
            4

            >>> is_i_prime = lambda n, i: is_prime(i) # need to pass 2-argument function into count_cond
            >>> count_primes = count_cond(is_i_prime)
            >>> count_primes(2)    # 2
            1
            >>> count_primes(3)    # 2, 3
            2
            >>> count_primes(4)    # 2, 3
            2
            >>> count_primes(5)    # 2, 3, 5
            3
            >>> count_primes(20)   # 2, 3, 5, 7, 11, 13, 17, 19
            8
            """
            "*** YOUR CODE HERE ***"
            i = 0
            count = 0
            while i < n:
                i += 1
                if condition(n, i):
                    count += 1
            return count

改后答案：

    def count_cond(condition):
        def a_function(n):
            i = 0
            count = 0
            while i < n:
                i += 1
                if condition(n, i):
                    count += 1
            return count
        return a_function

#### Q7: 倍数
第一版错误：
1. if只判断一次，是不构成循环的，循环要用``for``或者``while``来。

    def multiple(a, b):
        """Return the smallest number n that is a multiple of both a and b.

        >>> multiple(3, 4)
        12
        >>> multiple(14, 21)
        42
        """
        "*** YOUR CODE HERE ***"
        n = 1
        if n % a == 0 and n % b == 0:
            return n
        else:
            n += 1
改后答案：

    def multiple(a, b):
        n = 1
        while n <= a * b:
            if n % a == 0 and n % b == 0:
                return n
            else:
                n += 1

