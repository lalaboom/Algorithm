Python中的三元表达式可以将if-else语句放到一行里。语法如下：

```
value = true-expr if condition else false-expr
```
`true-expr`或`false-expr`可以是任何Python代码。它和下面的代码效果相同：

```
if condition:
    value = true-expr
else:
    value = false-expr
```
下面是一个更具体的例子：

```
In [126]: x = 5

In [127]: 'Non-negative' if x >= 0 else 'Negative'
Out[127]: 'Non-negative'
```
和if-else一样，只有一个表达式会被执行。因此，三元表达式中的if和else可以包含大量的计算，但只有True的分支会被执行。因此，三元表达式中的if和else可以包含大量的计算，但只有True的分支会被执行。

虽然使用三元表达式可以压缩代码，但会降低代码可读性。

Ref:《利用python进行数据分析》chapter2
