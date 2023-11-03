# docker


## 一.概念

### 1.镜像(Image)

- 一个只读的模板，镜像可以`用来创建 Docker 容器`

### 2.容器

- 容器是从镜像创建的运行实例
- 可以把容器看做是一个`简易版的 Linux 环境`
- 可以被启动、开始、停止、删除。每个容器都是相互`隔离`的、保证安全的平台

### 3.仓库(Registry)

- 集中`存放镜像`文件的场所，可以是公有的，也可以是私有的
- 最大的公开仓库是 Docker Hub



## 二.配置国内镜像源

### 1.docker desktop配置

打开 设置 - docker Engine

```
https://www.cnblogs.com/Flat-White/p/17107494.html
```
  
```json
{
  "builder": {
    "gc": {
      "defaultKeepStorage": "20GB",
      "enabled": true
    }
  },
  "experimental": false,
  "features": {
    "buildkit": true
  },
  "registry-mirrors": [
    "https://registry.docker-cn.com",
    "https://docker.mirrors.ustc.edu.cn",
    "http://hub-mirror.c.163.com"
  ]
}
```

## 三 常见问题 

### 1.无法拉取php80镜像问题

```bash
=> ERROR [php80 internal] load metadata for docker.io/library/php:8.0.30-fpm-alpine3.13# 
# 解决方式: 手动拉取, 并修改版本8.0.30为8.0.13(.env中)
docker pull php:8.0.13-fpm-alpine3.13
```

### 2.宿主机使用Navicat链接容器内的mysql

```sh
主机: 127.0.0.1 #没错, 就是这个
端口: 3305  #dnmp中是宿主机的3305映射到容器的3306
用户名: root
密码: 123456
```