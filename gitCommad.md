https://www.cnblogs.com/convict/p/10795320.html

## 一、初始化操作

```
https://blog.csdn.net/lw545034502/article/details/90696872
```



## 二、本地操作

* 111

  | 操作               | 区域   |
  | ------------------ | ------ |
  | 本地所做的任何修改 | 工作区 |
  | git add 后         | 暂存区 |
  | git commit 后      | 版本库 |

* 222

  | 命令              | 作用                                 |
  | ----------------- | ------------------------------------ |
  | git diff          | 工作区 vs 暂存区（新添加的文件不算） |
  | git diff head     | 工作区 vs 版本库                     |
  | git diff --cached | 暂存区 vs 版本库                     |

* 333

  | 命令                                 | 作用                                                         |
  | ------------------------------------ | ------------------------------------------------------------ |
  | git init                             | 初始化本地库（命令行的输出头部已经默认在 master 分支了。 但是这个时候，还并未创建 master 分支，只有当有一个提交的时候，才会创建 master 分支） |
  | git add <file name>                  | 将工作区的“新建/修改”添加到暂存区，后面加 .  就表示添加全部  |
  | git commit -m "提交日志" <file name> | 文件从暂存区到本地库，如果不加文件名，就是commit全部         |
  | git rm --cached <file name>          | 移除暂存区的修改                                             |
  | git branch                           | 查看本地分支，分支名称前 `*` 表示是当前分支                  |
  | git branch -a                        | 查看本地和远程的所有分支                                     |
  | git branch <name>                    | 创建分支                                                     |
  | git branch -r                        | 查看远程分支                                                 |
  | git checkout main                    | 切换分支                                                     |
  | git branch -d <name>                 | 删除分支                                                     |
  | git checkout -b <branch name>        | 创建一个分支并切换                                           |

* git status

  * Changes to be committed:   处于暂存区的文件（将要commit的文件嘛）
  * Changes not staged for commit: 还在工作区的文件（还没有在暂存区）





## 三、本地库跟远程库交互

 + git clone <远程库地址>：克隆远程库
 + git push： 如果不加文件名，就是push全部

