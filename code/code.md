# 激活码导入与导出

主要介绍不同平台之间激活码之间导入与导出的脚本等知识。

## 详细代码
```
#!/usr/bin/python
# encoding: utf-8


from models.code import ActivationCode


data = {}
ac = ActivationCode()
r = ac.redis

# 激活码导出
for key in r.keys():
    type = r.type(key)          # 数据类型  不同类型不同的获取与存入方式

    if type == 'set':
        value = list(r.smembers(key))
    elif type == 'hash':
        value = r.hgetall(key)

    data[key] = {'type': type, 'value': value}

import pickle                               # 详细看看关于pickle模块的应用
pickle.dump(data, open('log.txt', 'w'))

# 导入代码
data = pickle.load(open('log.txt', 'r'))
for k, values in data.items():
    # type = r.type(k)
    if values['type'] == 'set':
        for s in values['values']:
            value = r.sadd(k, s)
    elif values['type'] == 'hash':
            value = r.hmset(k, values['values'])
```

### 激活码导出数据格式
#### set 类型数据
```
non_code_sets_211 {'type': 'set', 'value': ['2119X674Y3QA9', '21139XXPZTVMQ', 
'211M7CNJWPW2J', '211RKZYAQ2UQ7', '211F3YJTAPZRP', '211W773EAPHKZ', 
'211UK3FQFE3V3', '211CUT2KHHEYT', '211E9JCRWDTV7', '211WMF2J3D243']}
```
#### hash 类型数据
```
one_code_sets_384_2015-09-10-01:31:11 {'type': 'set', 'value': 
['384W4C67YHRN4', '384VAYNPTREHX', '384CF923PWUVE', '384467QRRQGR9', 
'384HN4TRDRUV7', '384VT49Y2344U', '384C3FAYYUVYR', '384ZRQTUYPJUD', 
'384C39HGEMAZP', '384QPDGENH6GG']}
```
注：主要考察关于`redis数据库`的基本用法以及`pickle`的用法，需要下面详细介绍这两部分的使用。