\#define do{...}while(0)这种奇怪形式的宏定义经常在实际工程中应用，它的意义如下：

### **1. 增加代码的适应性**

下面的宏定义没有使用do{...}while(0)

```c
#define FOO(x) foo(x); bar(x);
```

这样宏定义，单独调用不会出现问题，例如：

```c
FOO(100)
```

宏扩展后变成：

```c
foo(x);bar(x);
```

这样调用FOO没有任何问题，但是FOO（x）不能放入控制语句中，例如：

```c
if (condition) FOO(x); 
else ...;
```

经过宏扩展后，变成了

```c
if (condition) 
    foo(x);
    bar(x);
else ...;
```

这样就导致了**语法错误**，语法错误并不可怕，在编译阶段就能发现，更致命的是他有可能导致**逻辑错误，**例如：

```c
if (condition)
    FOO(x);
```

这段代码经过扩展后变成：

```c
if (condition)
    foo(x); bar(x);
```

这样一来，无论condition是true还是false，bar(x)都会被调用。

这时候do{...}while(0)的价值就体现出来了，修改一下FOO的定义

```c
#define FOO(x) do {
        foo(x); 
        bar(x); 
    } while (0)
```

这样FOO,放入控制语句中就没有问题了。

也许有人说：把foo(x);bar(x)用大括号括起来不就行了吗？比如这样定义：

```c
#define FOO(x){
                foo(x); 
                bar(x); 
              }
```

再看下面代码：

```c
if (condition) 
    FOO(x); 
else ...;
```

扩展后：

```c
if (condition)
{
    foo(x);
    bar(x);
} ; //注意最后这个分号，语法错误 else ...;
```

照样语法错误；

### **2.增加代码的扩展性**

我理解的扩展性，主要是宏定义中还可以引用其他宏，比如：

```c
#define FOO(x) do{
    OTHER_FOO(x)
} while(0)
```

这样我们不用管OTHER_FOO是但语句还是符合语句，都不会出现问题

### **3.增加代码的灵活性**

 灵活性主要体现在，我们可以从宏中break出来，例如下面的定义：

```c
#define FOO(x)  do{ \
    foo(x);  \
    if(condition(x)) \
        break; \
    bar(x) \
    ..... \} while(0)
```
