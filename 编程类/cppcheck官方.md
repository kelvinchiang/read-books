## cppcheck官方文档

当前，官方最高版本是2.15，我们自己机器安装的版本是1.90



### 主要检测的对象

```
Undefined behaviour

Dead pointers
Division by zero
Integer overflows
Invalid bit shift operands
Invalid conversions
Invalid usage of STL
Memory management
Null pointer dereferences
Out of bounds checking
Uninitialized variables
Writing const data
```

Using a battery of tools is better than using one tool.



### 设置抑制规则

如果你需要过滤特定的错误，恶意通过抑制规则来实现。

拦截或抑制规则如下：

```
[error id]:[filename]:[line]
[error id]:[filename2]
[error id]
```

如一个最简单的suppressions.txt可以写成如下格式：

```
$cat suppressions.txt
unknownMacro
```

这样，“未定义的宏”这个错误就会在最终的生成的报告中被忽略掉了。

可以被抑制的粒度，由大到小依次为：报错id、文件名、行数。支持正则匹配。



### 执行命令

```
cppcheck . --suppresion-list=./suppressions.txt --error-exitcode=1 --output-file=cppcheck_output.html

```

上面命令生成的文件格式其实也是txt格式而已。

