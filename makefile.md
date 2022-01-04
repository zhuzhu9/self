makefile 工程管理文件，管理源文件、头文件、库文件、二进制文件

#### 简单版

fun.c fun.h main.c

makefile 

gcc fun.c main.c -o main

​	gcc *.c -o main

#### 如何书写makefile（规则）

1. makefile核心：目标 和 依赖
   - 依赖：需要编译的文件对象，*.c
   - 目标：需要生成的执行文件，
   - gcc \*.c -o main           依赖\*.c   main目标
2. makefile书写规则
   - 目标：依赖（各个依赖文件之间用空格连接）
   - \<tab键规则>
   - $^：代表所有的依赖文件
   - &@：代表目标文件
3. HelloWorld
   - 目标文件一定要存在，依赖文件可以不要

**写一个makefile，实现gcc main.c add.c -o add**

add:main.c add.c

​	gcc $^ -o $@

**写一个makefile，实现三个数获取最大值 gcc main.c max.c -o max**

max:main.c max.c

​	gcc $^ -o $@

指定执行文件的时候 make 文件对象（make add或make max）

clean: 删除对象

​	rm add max 删除目标对象

makefile会自动检测文件的更新时间！



#### 变量的种类

##### 自定义变量（默认字符串类型）

规则：

1. 命名规则和C语言的命名规则一致
2. 大小写敏感
3. 不使用#

