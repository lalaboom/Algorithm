### 日期和时间

Python内建的`datetime`模块提供了`datetime`、`date`和`time`类型。`datetime`类型结合了`date`和`time`，是最常使用的：

```
In [102]: from datetime import datetime, date, time

In [103]: dt = datetime(2011, 10, 29, 20, 30, 21)

In [104]: dt.day
Out[104]: 29

In [105]: dt.minute
Out[105]: 30
```
根据`datetime`实例，你可以用`date`和`time`提取出各自的对象：

```
In [106]: dt.date()
Out[106]: datetime.date(2011, 10, 29)

In [107]: dt.time()
Out[107]: datetime.time(20, 30, 21)
```
`strftime`方法可以将datetime格式化为字符串：

```
In [108]: dt.strftime('%m/%d/%Y %H:%M')
Out[108]: '10/29/2011 20:30'
```
`strptime`可以将字符串转换成`datetime`对象：

```
In [109]: datetime.strptime('20091031', '%Y%m%d')
Out[109]: datetime.datetime(2009, 10, 31, 0, 0)
```
表2-5列出了所有的格式化命令
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190831150826346.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3RlbmdjaGVuZ3R1NDEzOQ==,size_16,color_FFFFFF,t_70)
当你聚类或对时间序列进行分组，替换datetimes的time字段有时会很有用。例如，用0替换分和秒：

```
In [110]: dt.replace(minute=0, second=0)
Out[110]: datetime.datetime(2011, 10, 29, 20, 0)
```
两个datetime对象的差会产生一个`datetime.timedelta`类型：

```
In [111]: dt2 = datetime(2011, 11, 15, 22, 30)

In [112]: delta = dt2 - dt

In [113]: delta
Out[113]: datetime.timedelta(17, 7179)

In [114]: type(delta)
Out[114]: datetime.timedelta
```
结果`timedelta(17, 7179)`指明了`timedelta`将17天、7179秒的编码方式。

将`timedelta`添加到`datetime`，会产生一个新的偏移`datetime`：

```
In [115]: dt
Out[115]: datetime.datetime(2011, 10, 29, 20, 30, 21)

In [116]: dt + delta
Out[116]: datetime.datetime(2011, 11, 15, 22, 30)
```

ref:《利用python进行数据分析》chapter2
