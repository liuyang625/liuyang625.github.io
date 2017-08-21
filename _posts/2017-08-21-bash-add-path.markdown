---
layout: post
title:  "macOS 修改bash环境变量PATH及自建脚本直接输入脚本名执行的方法"
date:   2017-08-21 14:40:32 +0800
categories: jekyll update
---
# macOS 修改bash环境变量PATH及自建脚本直接输入脚本名执行的方法

## bash添加PATH环境变量的方法

* 新建`~/Desktop/Myshell`文件夹。
* 打开~/.bash_profile文件，配置PATH环境变量
* 添加一行：`export PATH=$PATH:~/Desktop/Myshell`保存。（`~/Desktop/Myshell`为需要添加的目录）
* 执行 `source ~/.bash_profile`。
* 重启terminal, 执行`echo $PATH`查看添加结果，不同的目录会以冒号`：`分隔，如下：
``` shell
  $ [liuyang] [~] $ echo $PATH
  /opt/subversion/bin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/Users/liuyang108/.rvm/bin:/Users/liuyang108/.rvm/bin:/Users/liuyang108/Desktop/Myshell

```
* 后续可以把自己的bash脚本添加到该目录下，就能直接执行脚本名，而无需输入路径信息。

## 自建脚本直接输入脚本名执行方法
* `~/Desktop/Myshell`目录下新建脚步文件`mypush`
* 修改脚步文件的执行权限：`chmod +x ~/Desktop/Meshell/mypush`
* 将目录`~/Desktop/Myshell`添加到PATH环境变量
* terminal执行`mypush`即可

注：mypush文件为简化的git push脚本，意在执行 `git push origin branchName:refs/for/branchName`,脚本内容如下：

``` shell
#!/bin/bash

localBranch=`git symbolic-ref --short -q HEAD`
if [ -z "$localBranch" ]; then
    echo "[Error] Invalid Path"
    exit 1
fi

if [ $# -eq 0 ]; then
    remoteBranch=$localBranch
elif [ $# -eq 1 ]; then
    remoteBranch=${1}
else
    echo "[Error] Only one param needed"
    exit 2
fi

git pull --rebase origin ${remoteBranch}

if [ $? -ne 0 ]; then
    exit 3
fi

git push origin ${localBranch}:refs/for/${remoteBranch}
exit 0
```
