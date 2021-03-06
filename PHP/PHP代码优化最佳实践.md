## PHP代码优化最佳实践

### 1. 尽可能的使用PHP的内置方法

花点时间去熟悉和学习PHP的内置方法

### 2. 使用Json替代xml

`json_encode`和`json_decode`等PHP的内置方法，运行速度都非常快，所以**应该优先使用Json**

### 3. 使用缓存技术

### 4. 使用isset()和empty()

与count()、strlen()和sizeof()函数相比，`isset()`和`empty()`对于检测一个变量是否为空等场景更加简单和高效

### 5. 减少不必要的类

如果你不打算重复使用一个类或者方法，那么它就没什么存在的价值。而如果你必须要定义和使用一个类，则需要合理规划类中的方法，**对于不是特别公用的方法，尽量将他们放到子类中去，因为调用子类中的方法，比调用父类方法速度更快**

### 6. 使用聚合函数减少数据库查询

查询数据库时，使用聚合函数，可以减少检索数据库的频率，并且使程序运行的更快

### 7. 使用强大的字符串操作函数

**strtr函数**则比str_replace()函数快四倍

### 8. 尽量使用单引号

如果可能，尽量使用单引号替代双引号。程序运行时，会检查双引号中的变量，这会拖慢程序的性能

### 9. 尝试使用恒等运算符

由于“===”仅检查闭合范围，因此比使用“==”进行比较速度更快。











