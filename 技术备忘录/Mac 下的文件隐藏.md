# Mac 下的文件隐藏

## 显示

显示隐藏文件命令：

```shell
defaults write com.apple.finder AppleShowAllFiles -bool true
KillAll Finder
```

隐藏隐藏文件命令：

```shell
defaults write com.apple.finder AppleShowAllFiles -bool false
KillAll Finder
```

活在 Finder 中按 `shift + cmd + .`，也可隐藏或显示隐藏文件

## 修改

修改文件为隐藏：

```shell
chflags hidden 文件名
```

修改文件为非隐藏：

```
chflags nohidden 文件名
```

