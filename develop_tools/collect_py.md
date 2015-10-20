# Python 部分命令收集

- eval的用法
    - 作用： 将**字典型字符串**转换为字典

json越来越流行，通过python获取到json格式的字符串后，可以通过eval函数转换成dict格式：
```
>>> a='{"name":"yct","age":10}'

>>> eval(a)
{'age': 10, 'name': 'yct'}
```