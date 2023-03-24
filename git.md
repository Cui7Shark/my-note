# git 学习

## 1.基础指令

- 获取本地仓库

![image-20230323201333608](D:\C++编程学习资料\我的笔记\images\image-20230323201333608.png)

```
git add .  (全部文件)添加至暂存区
git status  查看修改的状态
git commit -m "备注"  提交
git log    获取提交日志
git-log   配置的别名
git reset --hard *commitID* 版本回退
git reflog 看到已经删除的提交记录

```

![image-20230323201637820](D:\C++编程学习资料\我的笔记\images\image-20230323201637820.png)

```git
git init 初始化仓库
touch file01.txt
git add .
git commit -m "commit file01"
git log
git-log 精简查看log
git reset --hard commitID 回退
```

## 2.分支

```
#查看分支
git branch 
创建分支
git branch 分支名
#切换分支
git checkout 分支名
#直接切换到一个不存在的分支(创建并切换分支)
git checkout -b  分支名
#合并分支
git merge 分支名称
#删除分支
git branch -d b1   做检查
git branch -D b1   不检查，强制删除
```

## 3.解决冲突

![image-20230323204718343](D:\C++编程学习资料\我的笔记\images\image-20230323204718343.png)

![image-20230323211335499](D:\C++编程学习资料\我的笔记\images\image-20230323211335499.png)

### 开发中分支使用原则与流程

- master 生产分支
- dev    开发分支

![image-20230323211855858](D:\C++编程学习资料\我的笔记\images\image-20230323211855858.png)

### 总结

![image-20230323212754340](D:\C++编程学习资料\我的笔记\images\image-20230323212754340.png)

## 4.远程仓库

- 创建公钥-连接远程仓库

  ![image-20230323215945056](D:\C++编程学习资料\我的笔记\images\image-20230323215945056.png)![image-20230323220104086](D:\C++编程学习资料\我的笔记\images\image-20230323220104086.png)![image-20230323220138353](D:\C++编程学习资料\我的笔记\images\image-20230323220138353.png)

- 以码云为例

  1. 添加仓库

  git remote add <远端仓库名称> <仓库路径>

  ![image-20230323220148992](D:\C++编程学习资料\我的笔记\images\image-20230323220148992.png)

  2. 查看仓库

     git remote

     ![image-20230323220233953](D:\C++编程学习资料\我的笔记\images\image-20230323220233953.png)

  3. 推送到远程仓库

     ![image-20230323220252907](D:\C++编程学习资料\我的笔记\images\image-20230323220252907.png)

![image-20230323220326328](D:\C++编程学习资料\我的笔记\images\image-20230323220326328.png)

建立本地和远端分支的关系

下次直接

```
git push
```

![image-20230323220932664](D:\C++编程学习资料\我的笔记\images\image-20230323220932664.png)

```
#查看分支的详细信息
git branch -vv 
```

