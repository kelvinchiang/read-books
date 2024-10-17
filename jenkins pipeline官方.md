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

   

2. 文件