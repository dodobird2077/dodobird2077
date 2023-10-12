# 安装准备

## 1.软件准备
1. PHP >= 8.0.2 (官方开发环境为PHP8.0.2版本)
2. Mysql >= 5.7
3. NodeJs >= 14.13.1(目前使用16.9)
4. Npm >= 6.14.0 (安装完nodeJs自带)
5. Composer

## 2.php禁用函数
```
proc_open
proc_close
proc_get_status
```

## 3.pnpm安装
```bash
# 安装pnpm
npm install -g pnpm

# 确定pnpm安装成功
pnpm -v
```

## 4.url重写
```ts
// 找到其中的
location / {
    index index.php index.html error/index.html;
    ......
}
修改为：（将index.html放到第一位）
location / {
    index index.html index.php error/index.html;
    ......
}
```