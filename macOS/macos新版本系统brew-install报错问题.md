```shell
Warning: Bottle missing, falling back to the default domain...
```

```shell
tar: Error opening archive: Failed to open '/Users/zupernova/Library/Caches/Homebrew/downloads/551f73f417594bd810c1d61e1cacd24de3cc3e4cce825f0b9f763c0e0d32d84d--gdbm-1.20.big_sur.bottle.tar.gz'
Error: Failure while executing; `tar --extract --no-same-owner --file /Users/zupernova/Library/Caches/Homebrew/downloads/551f73f417594bd810c1d61e1cacd24de3cc3e4cce825f0b9f763c0e0d32d84d--gdbm-1.20.big_sur.bottle.tar.gz --directory /private/tmp/d20210728-71473-bqhpdw` exited with 1. Here's the output:
tar: Error opening archive: Failed to open '/Users/zupernova/Library/Caches/Homebrew/downloads/551f73f417594bd810c1d61e1cacd24de3cc3e4cce825f0b9f763c0e0d32d84d--gdbm-1.20.big_sur.bottle.tar.gz'
```

***解决方案***

更改环境变量，将HOMEBREW_BOTTLE_DOMAIN改正确即可

```shell
echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.ustc.edu.cn/homebrew-bottles/bottles' >> ~/.zshrc
source ~/.zshrc
```

此前brew已经换源为中科大源