### **#include两种声明区别**

**引用的头文件不同** 
\#include < > 引用的是编译器的类库路径里面的头文件。 
\#include“ ”引用的是你程序目录的相对路径中的头文件。 
**用法不同** 
\#include < > 用来包含标准头文件(例如stdio.h或stdlib.h). 
\#include“ ”用来包含非标准头文件。 
**调用文件的顺序不同** 
\#include < > 编译程序会先到标准函数库中调用文件。 
\#include“ ”编译程序会先从当前目录中调用文件。 
**预处理程序的指示不同** 
\#include < > 指示预处理程序到预定义的缺省路径下寻找文件。 
\#include“ ”指示预处理程序先到当前目录下寻找文件，再到预定义的缺省路径下寻找文件。

### 通用gcc编译过程

预处理 gcc/g++ -E hello.c/.cpp -o hello.i
编译 gcc/g++ -S hello.i -o hello.s
汇编 gcc/g++ -c hello.s -o hello.o
链接 gcc/g++ hello.o -o hello

### leetcode刷题sort函数第三个参数cmp必须声明为static的原因

可以知道其实我们写参数cmp时，是把函数名作为实参传递给了sort函数，而sort函数内部是用一个函数指针去调用这个cmp函数的

我们知道class普通类成员函数cmp需要通过对象名.cmp()来调用，而sort()函数早就定义好了，那个时候哪知道你定义的是什么对象，所以内部是直接cmp()的，那你不加static时，去让sort()直接用cmp()当然会报错

static静态成员函数不用加对象名，就能直接访问函数（这也是静态成员函数的一大优点）所以加了static就不会报错
