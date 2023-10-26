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

## 解决冲突演示
-  `web/package.json`

- 使用vscode 的GitLens插件来解决冲突, 文件顶部有个比较的功能, 比较好用

- `这里我是先应用线上的, 然后再把本地那个amap和qr-code的插件复制过去, 最后那个不认识就没保留`

![logo](/../_media/img/git-vscode-conflit.png ':size=600')

- 合并后

```js
{
    "name": "build-admin",
    "version": "2.0.4",
    "license": "Apache-2.0",
    "scripts": {
        "dev": "vite --force",
        "build": "vite build && esno ./src/utils/build.ts",
        "lint": "eslint --ext .js,.jsx,.ts,.vue src",
        "lint-fix": "eslint --ext .js,.jsx,.ts,.vue src --fix",
        "format": "npx prettier --write ."
    },
    "dependencies": {
        
        //// 本地2.0.2版本的插件////////////
        "@amap/amap-jsapi-loader": "^1.0.1",
        "qrcodejs2-fix": "^0.0.1",
        /////////////////////////////////

        "@element-plus/icons-vue": "2.1.0",
        "@vueuse/core": "10.2.0",
        "axios": "1.4.0",
        "countup.js": "2.6.2",
        "echarts": "5.4.2",
        "element-plus": "2.3.7",
        "esno": "0.16.3",
        "font-awesome": "4.7.0",
        "lodash-es": "4.17.21",
        "mitt": "3.0.0",
        "nprogress": "0.2.0",
        "pinia": "2.1.4",
        "pinia-plugin-persistedstate": "3.1.0",
        "screenfull": "6.0.2",
        "sortablejs": "1.15.0",
        "v-code-diff": "1.5.1",
        "vue": "3.3.4",
        "vue-i18n": "9.2.2",
        "vue-qr": "4.0.9",
        "vue-router": "4.2.2"
    },
    "devDependencies": {
        "@types/lodash-es": "4.17.7",
        "@types/node": "20.3.1",
        "@types/nprogress": "0.2.0",
        "@types/sortablejs": "1.15.1",
        "@typescript-eslint/eslint-plugin": "5.60.0",
        "@typescript-eslint/parser": "5.60.0",
        "@vitejs/plugin-vue": "4.2.3",
        "async-validator": "4.2.5",
        "eslint": "8.43.0",
        "eslint-config-prettier": "8.8.0",
        "eslint-plugin-vue": "9.15.1",
        "prettier": "2.8.8",
        "sass": "1.63.6",
        "typescript": "5.1.3",
        "vite": "4.3.9",
        "vue-eslint-parser": "9.3.1"
    }
}
```