使用git的过程中发现，就算文件的内容没改变，只有文件的权限改变的话，git也会检测到文件被修改了。

例如：

```bash
git diff test.c
diff --git test.c
old mode 100644
new mode 100755
```

解决方法是配置一下：

```bash
git config --global core.filemode false
```

有些时候，你发现这样配置之后没有什么效果，那是因为该容器内还有自己的配置信息，这个配置信息会覆盖 global 的配置，那么就需要对该容器做一下配置：

```bash
git config core.filemode false
```

这样git就会忽略文件模式的改变了。