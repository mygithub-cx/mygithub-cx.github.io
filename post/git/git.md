---
title: "Git"
description: "关于git的命令及一些相关的内容"
date: 2021-01-24T18:53:26+08:00
draft: false
# image: /post/git/git.png
categories:
    - tool
tags:
    - git
---

# Git使用

## 登陆

使用ssh的方式更方便  
首先找到用户目录下C:/user/username/.ssh  
没有就自己建  
然后打开gitbash  
命令  
> ssh-keygen -t rsa [-C 邮箱]
> // 前面是生成密钥,后面的参数是加密(使用的加密算法),可以不写

![.ssh目录](/post/git/ssh.png)
添加public公钥
![在gitee上使用public_key](/post/git/key.png)

## git命令

### 第一次使用

设置全局的姓名和邮箱,可以区分是谁干了什么
```cmd
git config --global user.name "姓名"
git config --global user.email *********@**.com
```

### 常用命令

- 查看状态
    ```cmd
    git status
    ```

- 查看提交记录
    1. 无参数版
        ```cmd
        git log
        ```
    1. 图形化显示分支情况
        ```cmd
        git log --graph
        ```

- 来
    从网上其它仓库直接克隆
    ```cmd
    git clone https:// *** *** ***.git
    ```

- 去
    1. 初始化
        ```cmd
        git init
        ```
    1. 都提交到暂存区
        ```cmd
        git add .
        ```
    1. 提交到仓库
        1. 提交
        ```cmd
        git commit -m "提交描述"
        ```
        1. 更正最近一次提交,不产生新的快照
        ```cmd
        git commit --amend [-m "新提交描述"]
        ```

- 回
    1. 放弃更改
        ```cmd
        git restore -- <filename> 
        //将文件返回提交的模样,也就是放弃修改(这个做不好的话新写的文件就没有了!)
        ```
    1. **reset**
        1. **从暂存区返回**
            ```cmd
            git reset HEAD <filename>
            // 反悔了,将最近提交的文件从暂存区取回,加上文件名表示只移除特定文件
            ```
        1. **从仓库返回**:文件状态恢复HEAD后有几个"~"就表示退到多少次提交之前,也可以用数字 HEAD~3 相当于 HEAD~~~  
        回到指定的快照直接输入几位识别码就行,例如:  
            ```cmd
            git reset 3j2ak43d3f
            ```
            1. soft
                ```cmd
                git reset --soft fc234dx
                // 只改变仓库内容,工作区和暂存区不受影响
                ```
            1. mixed
                ```cmd
                git reset --mixed HEAD~ 
                // (默认) 将快照回滚到暂存区,工作区不受影响
                ```
            1. hard
                ```cmd
                git reset --hard 243c4dx2werc234
                // 将快照还原到当前工作目录,所有区都受影响
                ```
        1. 只恢复单个文件
            ```cmd
            git reset 3412qw43 filepath/filename
            ```

- diff
    1. 比较暂存区与工作区(默认)
        ```cmd
        git diff
        ```
    1. 比较工作区与仓库
        ```cmd
        git diff 快照id
        // 如果是比较最新提交的快照可以直接用 HEAD
        ```
    1. 比较两个历史版本
        ```cmd
        git diff 快照id1 快照id2
        ```

- 分支
    - 创建分支
        ```cmd
        git branch [分支名]
        ```
    - 切换分支
        ```cmd
        git checkout [分支名]
        ```
    - 合并分支,合并到本分支
        ```cmd
        git merge [分支名]
        ```
    - 删除分支
        ```cmd
        git branch -d [分支名]
        ```
    - 匿名分支:随便使用,不会对文件产生任何影响,可以做任何实验性的事情
        ```cmd
        git checkout HEAD~
        ```
    ![分支](/post/git/branch.png)

- 与远程仓库的交互
    1. 查看连接状态
        ```cmd
        git remote -v
        ```
    1. 建立连接
        ```cmd
        git remote [新连接的名字] [连接的地址]
        ```
    1. 连接目标重命名
        ```cmd
        git remote rename [oldName] [newName]
        ```
    1. 推送到远程仓库
        ```cmd
        git push [仓库名,例如origin]
        ```
    
- 琐碎的小命令:并不是git的命令
    ```cmd
    cd
    psw
    ls [-la]
    mkdir
    touch
    rm [-r]
    vim
    cat
    history
    clear
    ```

## 一些文件
- **.git文件夹**
- **.gitignore**:忽略文件