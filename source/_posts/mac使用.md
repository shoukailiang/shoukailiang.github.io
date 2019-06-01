---
title: mac使用
date: 2019-06-01 22:30:50
tags: 
  - 工具
categories:
 - 工具
---
此不为技术文，作为本人记录使用。。。。
# 环境变量
> .bash_profile

在.zshrc最后一行加上 
```
source ~/.bash_profile
```
```
# nvm 
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh" # This loads nvm
# deno
export PATH=/Users/shoukailiang/.deno/bin:$PATH

# vscode
export PATH=/Applications/Visual\ Studio\ Code.app/Contents/Resources/app/bin:$PATH

```

# 苹果下挂载efi 分区
```
sudo mkdir /Volumes/EFI
sudo mount_msdos /dev/disk0s1 /Volumes/EFI
```
