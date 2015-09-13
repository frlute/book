# 日常代码知识点纪录

## 关于类
 - 父类**必须**保存在`lib`下，在`logics`中父类使用**object**，在`models`中父类使用**ModelBase**,此父类为我们自己写的，保存在`lib/db`下，并且每次调用需要初始化父类，使用:
>super(当前类名, self).__init__(self.uid)
