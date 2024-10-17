### 注意事项

1. 使用sh、bat时，如果内部引用变量，则最好用单引号，双引号有被注入风险。

   ```
   pipeline {
     agent any
     parameters {
       string(name: 'STATEMENT', defaultValue: 'hello; ls /', description: 'What should I say?')
     }
     stages {
       stage('Example') {
         steps {
           /* WRONG! */
           sh("echo ${STATEMENT}")
           /* CORRECT */
           sh('echo ${STATEMENT}')
         }
       }
     }
   }
   ```

   

2. pipeline支持docker镜像的使用，也支持dockerfile。



### Shared Libraries

可以定义一个共享库来给所有pipeline使用。所有的pipeline都可以立刻生效。

配置路径：Configure System -> Global Pipeline Libraries

```
(root)
+- src                     # Groovy source files
|   +- org
|       +- foo
|           +- Bar.groovy  # for org.foo.Bar class
+- vars
|   +- foo.groovy          # for global 'foo' variable
|   +- foo.txt             # help for 'foo' variable
+- resources               # resource files (external libraries only)
|   +- org
|       +- foo
|           +- bar.json    # static helper data for org.foo.Bar
```

**var** 目录是保存脚本文件，这些脚本文件作为pipeline使用的变量，脚本名字就是对应变量名。

> 注意：提交给共享库的代码享有不受限制的Jenkins权限。

如果勾选了“Load implicitly”，所有的pipeline会自动加载生效；如果没有勾选，则需要使用`@Library`。

```groovy
@Library('my-shared-library') _
/* Using a version specifier, such as branch, tag, etc */
@Library('my-shared-library@1.0') _
/* Accessing multiple libraries with one statement */
@Library(['my-shared-library', 'otherlib@abc1234']) _
```

另外，还可以在pipeline中动态加载共享库。

```
def lib = library('my-shared-library')
```

共享库应该是无状态的，仅用于一个函数的集合。如果保存状态的话，jenkins controller重启就会消失。



































