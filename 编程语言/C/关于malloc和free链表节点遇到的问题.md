链表初始化代码：

```c
Node* list_init(void){
    Node *head = (Node *)malloc(sizeof(Node));
    Node *p1 = (Node *)malloc(sizeof(Node));
    Node *p2 = (Node *)malloc(sizeof(Node));
    Node *p3 = (Node *)malloc(sizeof(Node));
    Node *p4 = (Node *)malloc(sizeof(Node));
    Node *p5 = (Node *)malloc(sizeof(Node));
    head->val = -1;
    p1->val = 11;
    p2->val = 5;
    p3->val = 7;
    p4->val = 10;
    p5->val = 1;
    head->next = p1;
    p1->next = p2;
    p2->next = p3;
    p3->next = p4;
    p4->next = p5;
    p5->next = NULL;
    return head;
}
```

链表删除代码：

```c
void list_deinit(Node* head){
    while(head){
        Node* tmp = head;
        head = head->next;
        free(tmp);
        tmp = NULL;
    }
}
```

链表打印代码：

```c
void list_print(Node* head){
    int first = 1;
    while (head)
    {
        if(first){
            first=0;
            printf("head->");
        }
        else{
            printf("(%d)->", head->val);
        }       
        head = head->next;
    }
    printf("NULL\n");
}
```

这时，在主函数中：

```c
Node *head = list_init();
list_deinit(head);
list_print(head);
```

运行程序会报段错误，疑问点在于：list_print函数中已经加入了head=NULL的特殊情况判断逻辑，但还是在deinit调用之后报错

经过在init和deinit函数中打印链表各节点地址发现，deinit函数中传入的只是head指针的一个副本；因此，在deinit函数中，head指针的副本以及链表其他节点已经全部释放空间并指向了NULL

但是在主函数中，head指针指向的结构体被释放了空间，但是并没有赋值为NULL，因此，在deinit之后再调用list_print,实际上是传入了一个野指针（此时的head），因此报了段错误。

修改后的程序代码源文件如下：

```c
void list_deinit(Node **head)
{
    while (*head)
    {
        Node** tmp = head;
        *head = (*head)->next;
        free(*tmp);
        *tmp = NULL;
    }
}//二级指针方法可以销毁head本身，并不是传入的副本

list_deinit(&head);//采用此方法调用
```
