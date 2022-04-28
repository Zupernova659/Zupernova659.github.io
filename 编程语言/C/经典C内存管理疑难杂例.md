### 一定要弄懂GetMemory 

### **堆栈**

栈中分配局部变量空间，是系统自动分配空间。定义一个 char a；系统会自动在栈上为其开辟空间。由于栈上的空间是自动分配自动回收的，所以栈上的数据的生存周期只是在函数的运行过程中，运行后就释放掉，不可以再访问。

堆区分配程序员申请的内存空间，堆上的数据只要程序员不释放空间，就一直可以访问到，不过缺点是一旦忘记释放会造成内存泄露。

静态区是分配静态变量，全局变量空间的。

```c
inta = 0; 全局初始化区  
char*p1; 全局未初始化区  
main(){  
  int b;      //栈  
  char s[] = "abc";    //栈  
  char *p2;      //栈  
  char *p3 = "123456";   // 123456\0在常量区，p3在栈上。  
  static int c =0；      //全局（静态）初始化区  
  p1 = (char *)malloc(10);    //堆  
}  
```

### **GetMemory1**

```c
voidGetMemory1(char *p)    
{
  p = (char *)malloc(100);    
}    
voidTest1(void)    
{    
  char *str = NULL;    
  GetMemory1(str);      
  strcpy(str, "hello world");    
  printf(str);  //str一直是空，程序崩溃     
}
```

结果： 

![img](https://img2020.cnblogs.com/blog/1943620/202111/1943620-20211118195523563-1384916852.png)

分析：

毛病出在函数GetMemory1 中。**编译器总是要为函数的每个参数制作临时副本**，指针参数p的副本是 _p，编译器使 _p = p。如果函数体内的程序修改了_p的内容，就导致参数p的内容作相应的修改。这就是指针可以用作输出参数的原因。在本例中，_p申请了新的内存，只是把 _p所指的内存地址改变了，但是p丝毫未变。所以函数GetMemory并不能输出任何东西。事实上，每执行一次GetMemory1就会泄露一块内存，因为没有用free释放内存。**Test1中调用GetMemory1时，函数参数为str的副本不是str本身**。

### **GetMemory2**

```c
voidGetMemory2(char **p, int num)    
{
  *p = (char *)malloc(num);    
}    
voidTest2(void)    
{    
  char *str = NULL;    
  GetMemory2(&str, 100);    
  strcpy(str, "hello");      
  printf(str);        
}    
```


结果：输出hello 

分析：动态分配的内存不会自动释放；

没有测试是否成功分配了内存，应该有if (*p == NULL) { ……} 之类的语句处理内存分配失败的其情况。

### **GetMemory3**

```c
char* GetMemory3(void)    
{      
  char p[] = "hello world";    
  return p;    
}    
voidTest3(void)    
{    
  char *str = NULL;    
  str = GetMemory3();        
  printf(str);    
}
```

结果：输出乱码。 

分析：字符数组p存在于**栈空间，是局部变量，函数返回后，内存空间被释放**，因此输出无效值。字符数组的值是可以修改的，例如p[0] = 't‘。

### **GetMemory4**

```c
char*GetMemory4(void)    
{    
  char *p = "hello";    
  return p;    
}    
voidTest4(void)    
{    
  char *str = NULL;    
  str = GetMemory4();   
  cout<< str << endl;    
}
```

结果：输出hello
分析：p指向的是字符串常量，字符串常量保存在只读的数据段，是全局区域，但不是像全局变量那样保存在普通数据段（静态存储区）。无法对p所指的内存的内容修改，例如p[0] = 'y;这样的修改是错误的。

### **GetMemory5**

```c
char*GetMemory5(void)    
{     
  return "hello";    
}    
voidTest3(void)    
{    
  char *str = NULL;    
  str = GetMemory5();     
  printf(str);    
}    
```


结果：输出hello

分析：直接返回常量区。

### **GetMemory6**

```c
voidGetMemory6(void) {  
  char *str = (char*)malloc(100);  
  strcpy(str, "hello");  
  free(str);  
  //str = NULL，加上这句程序才不会有野指针  
  if (str != NULL) {  
    strcpy(str, "world");  
    printf(str);  
  }  
}  
voidmain(){    
  GetMemory6();  
}    
```

结果：能够输出world，但程序存在问题。

分析：程序出现了**野指针**。

**野指针**只会出现在像C和C++这种没有自动内存垃圾回收功能的高级语言中, 所以java或c#肯定不会有野指针的概念. **当我们用malloc为一个指针分配一个空间后, 用完这个指针，把它free掉，但是没有让这个指针指向NULL或某一个特定的空间**。如上面程序一样，将str进行free后，只是释放了指针所指的内存，但指针并没有释放掉，此时指针所指的是垃圾内存；这样的话，if语句永为真，if判断无效。delete也存在同样的问题。

**防止产生野指针**：

- 指针变量一定要初始化为NULL，因为任何指针变量刚被创建时不会自动成为NULL指针，它的缺省值是随机的。
- 当free或delete后，将指针指向NULL。通常判断一个指针是否合法，都是使用if语句测试该指针是否为NULL。

 
