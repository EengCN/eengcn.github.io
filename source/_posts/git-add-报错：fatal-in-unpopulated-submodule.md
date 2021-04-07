---
title: 'git add ., 报错：fatal: in unpopulated submodule ''*/*'''
date: 2019-02-25 16:04:47
tags: git
copyright: true
---

原因：一个仓库的子文件夹存在另一个git

发现问题：
我执行 `git push`指令之后，发现远程仓库的子文件夹里并没有文件，然后我再本地进入到当前子文件夹，执行`git add .`，报错如题。

解决方法：在根目录执行以下指令
> 1.删除子文件夹下的.git文件夹
> 2.清楚子文件夹的git缓存，执行 `git rm -r --cached 子文件夹名/`
> 3.执行git add 子文件夹名/* 添加这个文件夹下所有文件

接下来正常执行 `git commit -m "添加子文件夹文件"`，`git push`。这时再去远程仓库查看子文件夹已有文件了。



