# Git

## 1.相关教程

1. 相关教程
- 按照教程所说, 最常用的还是ide中使用git

```
1.尚硅谷新版git(3小时)
https://www.bilibili.com/video/BV1wm4y1z7Dg/?spm_id_from=333.788.video.desc.click&vd_source=b620a69473c31bfeacc5e3bc26991cb3

2.狂神(快速入门)
https://www.bilibili.com/video/BV1FE411P7B3/?spm_id_from=333.337.search-card.all.click&vd_source=b620a69473c31bfeacc5e3bc26991cb3

3.黑马git(3.5小时) √ 选中
https://www.bilibili.com/video/BV1MU4y1Y7h5/?p=2&spm_id_from=pageDriver&vd_source=b620a69473c31bfeacc5e3bc26991cb3
```

2. 常用工具
    1. sourceTree 
    2. 乌龟版git


## 课程记录

1. 分布式版本库: 
    - 每个人电脑上都有一个完整的版本库


2. 安装和配置(P5)

```bash
  # 设置用户信息 
  $ git config --global user.name "用户名"
  $ git config --global user.email "邮箱"
  
  # 查看配置信息
  $ git config --global user.name
  $ git config --global user.email
```

3. 初始化本地仓库(P6)
    - 1.在桌面上新建一个GitTest目录
    - 2.右键 git bash here
    - 3.执行 `git init`

4. git基础操作(P6, 较长)

- 文件状态

| 工作区(workspace) | 暂存区(index) | 仓库(repository) |
| ------- | ------- | ------- |
| 1.未跟踪(untracked)  | -  | -  |
| 2.未暂存(unstaged)  | 3.已暂存(staged)  | commit  |

1. 未跟踪 (新建的文件)
2. 未暂存 (修改已经有的文件)
3. 已暂存 (提交到仓库前的暂缓区)

- 操作

1. `git add`  添加到暂存区

2. `git status` 查看状态

3. `git commit`  提交到仓库

4. `git log` 查看日志

```bash
git log [option]
    --all 显示所有分支
    --pretty=oneline 将提交信息显示为一行
    --abbrev-commit 使的输出commitld更简短
    --graph 以图的形式显示

# 常用: (可以加个别名)
# 1.按一行显示简短的日志
git log --pretty=oneline --abbrev-commit

# 2.按照分支显示简短日志
git log --pretty=oneline --abbrev-commit --all --graph 
```

### 5. 版本回退 (退回到某个版本)

```bash
# git reset --hard 版本号
git reset --hard 12de115 

# 1.回退前
$ git log --pretty=oneline --abbrev-commit
6690452 (HEAD -> master) cn01
bd78eb8 edit file
44e8ffa edit file01
12de115 add file01

# 2.回退后 (发现后面提交的版本都不见了, 如果需要查看用git reflog)
$ git log --pretty=oneline --abbrev-commit
12de115 (HEAD -> master) add file01
```

6. 添加文件到忽略列表(.gitignore)

```bash
# 例如:
*.a
*.txt
*.log
```

### P8.基础练习(学习中)