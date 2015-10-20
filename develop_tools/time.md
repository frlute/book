# Python时间模块常用总结

时间格式：
    - 时间戳(1443495263.138949)
    - 字符串('2015-09-29')
    - 数组的形式(time.struct_time(tm_year=2015, tm_mon=9, tm_mday=29, tm_hour=11, tm_min=40, tm_sec=14, tm_wday=1, tm_yday=272, tm_isdst=0)或datetime.datetime(2015, 9, 29, 11, 40, 39, 628315))

## 显示当前时间

### time

- 字符串

```
In [1]: time.strftime('%F')
Out[1]: '2015-10-12'

In [46]: time.ctime(time.time())
Out[46]: 'Mon Oct 19 17:08:06 2015'
```
`%H`表示显示当前小时,`%M`表示分钟。

- 时间戳

```
In [9]: time.time()
Out[9]: 1445229009.847559
```

- 数组

```
In [10]: time.localtime()
Out[10]: time.struct_time(tm_year=2015, tm_mon=10, tm_mday=19, tm_hour=12, tm_min=31, tm_sec=8, tm_wday=0, tm_yday=292, tm_isdst=0)

Out[66]: datetime.datetime.now().timetuple()
Out[66]: time.struct_time(tm_year=2015, tm_mon=10, tm_mday=19, tm_hour=17, tm_min=59, tm_sec=25, tm_wday=0, tm_yday=292, tm_isdst=-1)
```

### datetime

- 字符串

```

```

- 时间戳

```

```

- 数组

```
In [15]: datetime.datetime.now()
Out[15]: datetime.datetime(2015, 10, 19, 12, 49, 11, 103343)

In [61]: datetime.datetime.today()
Out[61]: datetime.datetime(2015, 10, 19, 17, 51, 47, 673824)

In [63]: datetime.datetime.fromtimestamp(time.time())
Out[63]: datetime.datetime(2015, 10, 19, 17, 54, 6, 741568)
```
第一种方式后面可跟`.year/.month/.day/.hour/.minute/.second`等显示当前年，月，日, weekday()表示周几等。
```
date.weekday()：返回weekday，如果是星期一，返回0；如果是星期2，返回1，以此类推；
data.isoweekday()：返回weekday，如果是星期一，返回1；如果是星期2，返回2，以此类推；
```

##  不同时间格式之间的转换
转换类型: `%Y-%m-%d %H:%M:%S`。

- tips: 
    - gmtime()与mktime（）可以将时间戳与数组时间表示方法自由转换。
    - strftime()可以将struct_time类型自由转换成字符型.
    - strptime(string, format) 将时间字符串根据指定的格式化符转换成数组形式的时间.
    -  datetime中提供了strftime方法，可以将一个datetime型日期转换成字符串.
    -  datetime.strptime(date_string, format)：将格式字符串转换为datetime对象.
    -  date.fromtimestamp(timestamp)：根据给定的时间戮，返回一个date对象.(time与datetime之间的互相转换)


### 字符串转时间戳
- time类型

- datetime类型 

### 字符串转数组

- time

```
In [14]: time.strptime('2015-10-19 12:46:51', _format)
Out[14]: time.struct_time(tm_year=2015, tm_mon=10, tm_mday=19, tm_hour=12, tm_min=46, tm_sec=51, tm_wday=0, tm_yday=292, tm_isdst=-1)
```

- datetime

```
In [28]: datetime.datetime.strptime('2015-10-19 15:16:03', _format)
Out[28]: datetime.datetime(2015, 10, 19, 15, 16, 3)
```

### 时间戳转字符串

- time

```
In [46]: time.ctime(time.time())         time.ctime() 默认为当前时间
Out[46]: 'Mon Oct 19 17:08:06 2015'
```

- datetime

```
In [29]: datetime.datetime.fromtimestamp(time.time())
Out[29]: datetime.datetime(2015, 10, 19, 16, 46, 23, 70124)
```

### 时间戳转数组

- time

```
In [11]: time.gmtime(time.time())
Out[11]: time.struct_time(tm_year=2015, tm_mon=10, tm_mday=19, tm_hour=4, tm_min=32, tm_sec=22, tm_wday=0, tm_yday=292, tm_isdst=0)
```

- datetime 

```

```

### 数组转时间戳

- time

```
In [12]: time.mktime(time.localtime())
Out[12]: 1445229214.0
```

- datetime

```

```

###数组转字符串

- time

```
In [13]: time.strftime(_format, time.localtime())
Out[13]: '2015-10-19 12:46:51'

In [42]: time.asctime(time.localtime())
Out[42]: 'Mon Oct 19 16:56:14 2015'
```

- datetime 

```
In [25]: datetime.datetime.now().strftime(_format)
Out[25]: '2015-10-19 15:16:03'
```

## 时间的运算

- 天数显示

- 时间的加减

```
In [84]: tom
Out[84]: datetime.datetime(2015, 10, 22, 21, 16, 33, 572101)

In [85]: now
Out[85]: datetime.datetime(2015, 10, 19, 21, 16, 1, 823402)

In [86]: tom-now
Out[86]: datetime.timedelta(3, 31, 748699)
```

## 其他时间模块介绍

### time
- clock()
clock() -> floating point number。

该函数有两个功能:

(1)在第一次调用的时候，返回的是程序运行的实际时间;

(2)以第二次之后的调用，返回的是自第一次调用后,到这次调用的时间间隔.

```
import time  
if __name__ == '__main__':  
    time.sleep(1)  
    print "clock1:%s" % time.clock()  
    time.sleep(1)  
    print "clock2:%s" % time.clock()  
    time.sleep(1)  
    print "clock3:%s" % time.clock(
```

输出：
```
  clock1:3.35238137808e-006
  clock2:1.00004944763
  clock3:2.00012040636
  其中第一个clock输出的是程序运行时间
  第二、三个clock输出的都是与第一个clock的时间间隔
```

### datetime

```
In [70]: datetime.datetime.now().isocalendar()
Out[70]: (2015, 43, 1)      年， 一年中的第几周  周几


In [79]: datetime.datetime.now()
Out[79]: datetime.datetime(2015, 10, 19, 21, 12, 28, 241711)

In [81]: datetime.datetime.now().replace(day=3)
Out[81]: datetime.datetime(2015, 10, 3, 21, 12, 36, 563011)
```


## 参考文档

[python datetime处理时间](http://www.cnblogs.com/lhj588/archive/2012/04/23/2466653.html "datetime介绍")

[python time处理时间](http://blog.csdn.net/kiki113/article/details/4033017)

