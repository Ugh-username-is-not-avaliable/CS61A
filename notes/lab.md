## 一些值得留意的记录


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

