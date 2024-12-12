# 《Groovy in Action》阅读笔记

G r o o v y 的 语 法 是 面 向 行 的 ，但 是 执 行 g r o o v y 代 码 确 不 是 这 样 的 ，不 像 别 的 脚 本 语 言 ， groovy代码不是一行一行的解释执行的。

根据代码的任意字符串来运行的能力是脚本语言的显著特征，这意味着groovy能像脚本语言一样使用，虽然groovy本身是一个一般的编程语言。

java数据类型分为专用类型和包装类型；groovy虽然允许声明变量为专有类型（如int），后台真正使用的类型是包装类型。也就是说，groovy没有专有类型全部是包装类型。



### java.lang.String和Gstring

双引号的字符串是一个Gstring实例。单引号的不是。Gstring允许占位符。

```groovy
line = "me $me - you $you"

date = new Date(0)
out = "Date is ${date.toGMTString()}!"

```

### 书中例子纠错

在Listing 4.9章节，有两个地方错误，实际执行的正确脚本如下：

```groovy
assert [1, [2,3]].flatten() == [1,2,3]
assert [1,2,3].intersect([4,3,1]) == [1,3]
assert [1,2,3].disjoint([4,5,6])

list = [1,2,3]
popped = list.pop()
assert popped == 1
assert list == [2,3]

assert [1,2].reverse() == [2,1]
assert [3,1,2].sort() == [1,2,3]

def list = [[1,0], [0,1,2]]
list = list.sort{a,b -> a[0] <=> b[0]}
assert list == [[0,1,2], [1,0]]

```

所以，书中提供的示例，最好都能够手动敲击一下来实验。计算机科学是一门实践的科学。

### 闭包

一个闭包是被包装为一个对象的代码块，实际上闭包像一个可以接受参数并且能够有返回值的方法。

闭包是一个普通对象，因为你能够通过一个变量引用到它，正如你能够引用到任何别的对象一样。

闭包仅仅是一个对象，groovy提供了一种非常容易创建闭包的方式和启用了一些非常常用的行为。

在groovy中传递一个闭包给一个方法是非常平常，没有特殊规则。同样，如果闭包仅仅需要一个参数，groovy提供了一个缺省的名称 --- it，因此，你不需要进行特别的声明。

> 闭包是由一些代码组成的对象，并且groovy为闭包提供了简洁的语法。

groovy使用闭包来指定这些每次都被执行的代码块，并且增加了许多额外的方法（each、find、findAll、collect等等）到集合类上，使他们容易使用。

处理资源时使用闭包

```
new File('myfile.txt').eachLine{ println it}

log = ''
(1..10).each{ counter -> log += counter}
assert log == '12345678910'

log = ''
(1..10).each{log+=it}
assert log == '12345678910'

```

注意不像counter，魔术变量it不需要进行声明。隐式参数it。

在groovy中类型是可选的，因此闭包的参数是可选的，如果闭包的参数进行了显式的类型声明，那么类型的检查发生在运行时而不是在编译的时候。

```
def benchmark(repeat, Closure worker) {
    start = System.currentTimeMillis()
    repeat.times{worker(it)}
    end = System.currentTimeMillis()
    return end - start
}
slow = benchmark(10000, {(int) it/2})
println "slow: $slow"
slow = benchmark 10000, {(int) it/2}
println "slow: $slow"
slow = benchmark (10000), ({(int) it/2})
println "slow: $slow"
fast = benchmark(10000) {it.intdiv(2)}
println "fast: $fast"
```

如上，很多情况下，函数调用时，括号都可以省略不写，这是跟其他程序不一样的地方，很灵活。

















