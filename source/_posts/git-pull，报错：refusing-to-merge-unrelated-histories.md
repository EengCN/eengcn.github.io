---
title: git pull，报错：refusing to merge unrelated histories
date: 2019-02-25 15:10:28
tags: git
copyright: true
---

**git 2.9.0之后**，合并两个没有共同历史记录的分支，会报`fatal:refusing to merge unrelated histories`错误。

我遇到这个错误的场景，我在Github创建了一个仓库，写了个Readme.md，License文件。然后把本地的仓库与这个新建的远程仓库，关联上，然后将这个本地仓库上传到远程仓库。

> 1.使用 `$ git remote add origin git@github.com:xxx/xxx.git` 关联上远程仓库

> 2.使用`$ git pull`指令，使用pull指令后报了 `fatal:refusing to merge unrelated histories` 错误，因为这个两个仓库没有共享相同的历史记录（本地master分支，与远程master分支没有相同的祖先）。

这时要pull远程master分支并合并到本地master，需要加上 `--allow-unrelated-histories`

以默认仓库名，master为例 ：

``` git
$ git pull origin master --allow-unrelated-histories
```


