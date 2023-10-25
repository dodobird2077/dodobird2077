# 版本更新

### 这里指的是, 官方进行了版本更新, 将这些更新拉到本地

- 官方文档 

```
https://doc.buildadmin.com/guide/other/update.html#git%E5%8C%85
```


## 拉取代码（更新示范第一步）

```bash
# 保存工作现场
git stash
Saved working directory and index state WIP on v2: ******** # 命令输出：保存工作区成功

# 检查远程仓库设置
git remote -v
buildadmin      https://gitee.com/wonderful-code/buildadmin.git (fetch)
buildadmin      https://gitee.com/wonderful-code/buildadmin.git (push)
# PS：笔者这里BuildAdmin远程仓库的名称是buildadmin，你仓库的可能不一样，或者干脆没有地址为`https://gitee.com/wonderful-code/buildadmin.git`的远程仓库设置
# 如果没有该仓库地址，请执行命令添加：git remote add buildadmin https://gitee.com/wonderful-code/buildadmin.git

# 拉取最新代码，buildadmin 为远程仓库名称
git pull buildadmin v2
```

## 解决冲突（更新示范第二步）

```bash
看官方文档
```

## 完成合并（更新示例第三步）
```bash
# 处理完冲突后，提交代码
git add .
git commit -m 处理冲突

# 恢复工作现场
git stash pop

# 更新后端依赖
composer update

# 更新前端依赖
cd web
pnpm install

# 更新数据表结构
php think migrate:run
```

## 关于推送到哪个远程仓库
```bash
git remote -v
buildadmin      https://gitee.com/wonderful-code/buildadmin.git (fetch)
buildadmin      https://gitee.com/wonderful-code/buildadmin.git (push)
origin  https://github.com/dodobird2077/demo0922.git (fetch)
origin  https://github.com/dodobird2077/demo0922.git (push)

// 其实直接推送到main就行了, 不用说切换哪个远程仓库, main就是github上面的那个
git push origin main
```

```bash
暂无
```

![img](/buildadmin/update/update.png)