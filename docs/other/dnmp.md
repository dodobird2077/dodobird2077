# Docker - dnmp

##### 说明

- 按照github上面的教程一步步走即可

```
https://github.com/yeszao/dnmp
```

- 环境要求
    - git
    - Docker(系统需为Linux，Windows 10 Build 15063+，或MacOS 10.12+，且必须要64位）
    - docker-compose 1.7.0+


## 步骤

1. 克隆项目

```bash
git clone https://github.com/yeszao/dnmp.git
```

2. 如果主机是 Linux系统，`且当前用户不是root用户`，还需将当前用户加入docker用户组, 我这个是root, 所以没加

```
sudo gpasswd -a ${USER} docker
```

3. 拷贝并命名配置文件, 原神启动!

```bash
$ cd dnmp                                           # 进入项目目录
$ cp env.sample .env                                # 复制环境变量文件
$ cp docker-compose.sample.yml docker-compose.yml   # 复制 docker-compose 配置文件。默认启动3个服务：
                                                    # Nginx、PHP7和MySQL8。要开启更多其他服务，如Redis、
                                                    # PHP5.6、PHP5.4、MongoDB，ElasticSearch等，请删
                                                    # 除服务块前的注释
$ docker-compose up                                 # 启动
```

4. 在浏览器中访问 `http://localhost`

PHP代码在文件 `./www/localhost/index.php`



## 部署一个dcat项目

```
1.阿里云镜像
composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/


2.进入到php容器
cd /www/localhost
cd ..
composer  create-project --prefer-dist laravel/laravel localhost  7.*

3.创建数据库: dcat
mysql -uroot -p123456
create database dcat;

4.安装 dcat-admin
composer require dcat/laravel-admin:"2.*" -vvv

5.发布资源
php artisan admin:publish

6.修改app/Providers/AppServiceProvider.php的boot方法,添加
\Schema::defaultStringLength(191);

7.安装数据库
php artisan admin:install


```

### 配置Nginx

```
dnmp\dnmp-master\services\nginx\conf.d\localhost.conf

1.配置nginx重写规则
location / {
    try_files $uri $uri/ /index.php?$query_string;
}

2.修改默认主页
root   /www/localhost/public;

```

### 配置目录权限

```
1.进入php容器

chmod -R 775 /www/localhost/storage/
chmod -R 775 /www/localhost/bootstrap/cache/
chmod 600 .env

chown -R www-data:www-data /www/localhost/storage
```