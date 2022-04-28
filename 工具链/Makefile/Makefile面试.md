***介绍一下make? 为什么使用make***

1、包含多个源文件的项目在编译时有长而复杂的命令行，可以通过makefile保存这些命令行来简化该工作
2、make可以减少重新编译所需要的时间，因为make可以识别出哪些文件是新修改的
3、make维护了当前项目中各文件的相关关系，从而可以在编译前检查是否可以找到所有的文件

makefile：一个文本形式的文件，其中包含一些规则告诉make编译哪些文件以及怎样编译这些文件，每条规则包含以下内容：
一个target，即最终创建的东西
一个和多个dependencies列表，通常是编译目标文件所需要的其他文件
需要执行的一系列commands，用于从指定的相关文件创建目标文件

make执行时按顺序查找名为GNUmakefile，makefile或者Makefile文件，通常，大多数人常用Makefile
Makefile规则：

```c
target: dependency dependency [..]    command    command    [..]
注意：command前面必须是制表符
```

例子：

```c
editor: editor.o screen.o keyboard.o
    gcc -o editor editor.o screen.o keyboard.o
editor.o : editor.c editor.h keyboard.h screen.h
    gcc -c editor.c
screen.o: screen.c screen.h
    gcc -c screen.c
keyboard.o : keyboard.c keyboard.h
		gcc -c keyboard.c
clean:
		rm editor *.o
```