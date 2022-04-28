```makefile
SRC=$(wildcard *.c) //同时将文件夹内所有.c编译为同名的目标文件
OBJ=$(patsubst %.c,%,$(SRC))
%:%.c//此处使用了内建隐含规则
    cc -c $< -o $@
all:$(OBJ)
```

 
