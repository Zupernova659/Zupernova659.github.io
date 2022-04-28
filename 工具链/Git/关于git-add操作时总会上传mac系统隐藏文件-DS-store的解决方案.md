.DS_Store是Mac OS保存文件夹的自定义属性的隐藏文件，如文件的图标位置或背景色，相当于Windows的desktop.ini。

当git仓库中进行了多项文件修改时，传统的方法为了剔除大量的.DS_Store文件只能手动一项一项添加待加入的文件，很麻烦。

为了解决这一问题，有如下两种方案：

**方案一：禁止.DS_store生成**

打开 “终端” ，复制黏贴下面的命令，回车执行，重启Mac即可生效。

```
defaults write com.apple.desktopservices DSDontWriteNetworkStores -bool TRUE
```

**方案二：设置git过滤规则**

具体参考[Git过滤规则](https://www.cnblogs.com/zupernova/p/15529106.html)

在根目录下创建文件.gitignore，写入内容

```
.DS_Store
*.code-workspace
```

即可让git在执行add操作时屏蔽所有符合条件的文件，使其不出现在未追踪文件列表中
