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



## 1.部署一个dcat项目

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

### 简单配置项目

```
1.设为中文
config/app.php 设置 locale 参数的值为 zh_CN

2.常见配置和问题
https://learnku.com/docs/dcat-admin/2.x/common-problem/8088
```

然后访问localhost和localhost/admin即可

## 2.搭建另一个虚拟域名网站

1. 使用php8镜像 (yml中开启, 拉取不了的话手动拉取)
2. 添加一个nginx的vhost配置 `services\nginx\conf.d\demo88.conf`

```conf
server {
    listen       80;
    server_name  demo88.cc;
    root   /www/bd;
    index  index.php index.html index.htm;
    access_log /dev/null;
    error_log  /var/log/nginx/nginx.localhost.error.log  warn;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }
    location ~ [^/]\.php(/|$) {
        fastcgi_pass   php80:9000;
        include        fastcgi-php.conf;
        include        fastcgi_params;
    }
}
```

3. windows 修改配置hosts文件 `C:\Windows\System32\drivers\etc\hosts`

```bash
127.0.0.1 demo88.cc
```

3. 添加主页文件, index.php中只有个phpinfo(); 注意不要有乱码

```
ls /www/bd
index.html  index.php
```

4. 进入nginx容器, 重载Nginx配置, 访问域名, 成功 

`中途更换过一些域名, 到时会访问到真实的线上域名, 多更换测试几次成功了`



## 3.部署buildAdmin项目(上线项目, 非本地开发环境)

- 说明: 由于bd框架, 使用的是php think run + npm dev的方式来运行, 感觉不太适合docker环境, 这里只测试部分编译后的bd项目, 模拟其上线测试

1. 使用上一步配置来搭建

2. 添加一个nginx的vhost配置

3. 直接在github下载此项目(因为私有项目不能直接clone)

4. 复制数据库. 并配置数据库, 让php80可以访问Mysql容器

```ini
[DATABASE]
TYPE = mysql
HOSTNAME = mysql5
DATABASE = bd
USERNAME = root
PASSWORD = 123456
HOSTPORT = 3306
CHARSET = utf8mb4
DEBUG = true
```

5. 配置nginx文件

```conf
server {
    listen       80;
    server_name  demo88.cc;
    root   /www/bd/public;
    index  index.php index.html index.htm;
    #charset koi8-r;
    
    access_log /dev/null;
    #access_log  /var/log/nginx/nginx.localhost.access.log  main;
    error_log  /var/log/nginx/nginx.localhost.error.log  warn;

    location / {
        # 将 index.html 放在第一位可以实现隐藏 index.html
        index index.html index.php error/index.html;

        # 这一段即为 URL重写规则 请确保存在
        if (!-e $request_filename) {
            rewrite  ^(.*)$  /index.php?s=/$1  last;
            break;
        }
        # 结束
    }
    #error_page  404              /404.html;

    # redirect server error pages to the static page 
    #
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }

    # proxy the PHP scripts to Apache listening on 127.0.0.1:80
    #
    #location ~ \.php$ {
    #    proxy_pass   http://127.0.0.1;
    #}

    # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
    #
    location ~ [^/]\.php(/|$) {
        fastcgi_pass   php80:9000;
        include        fastcgi-php.conf;
        include        fastcgi_params;
    }

    # deny access to .htaccess files, if Apache's document root
    # concurs with nginx's one
    #
    #location ~ /\.ht {
    #    deny  all;
    #}
}

```

6. 浏览器打开 即可

`http://demo88.cc` 

## 4.测试部署普通TP6项目

1. 打开TP6官网, 下载tp6.0.10版本
2. 进入php容器
3. 安装项目: `composer create-project topthink/think /www/tp6`
4. 配置nginx, 这里使用默认的php7.4

```conf
server {
    listen       80;
    server_name  demotp6.cc;
    root   /www/tp6/public;
    index  index.php index.html index.htm;
    access_log /dev/null;
    error_log  /var/log/nginx/nginx.localhost.error.log  warn;
    location / {
        # 将 index.html 放在第一位可以实现隐藏 index.html
        index index.html index.php error/index.html;
        # 这一段即为 URL重写规则 请确保存在
        if (!-e $request_filename) {
            rewrite  ^(.*)$  /index.php?s=/$1  last;
            break;
        }
    }
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }
    location ~ [^/]\.php(/|$) {
        fastcgi_pass   php:9000;
        include        fastcgi-php.conf;
        include        fastcgi_params;
    }
}
```

## 5.测试redis, 使用dcat项目测试, 写接口进行测试

### 5.1 创建redis容器
- 1.首先在yml中打开redis, 然后执行docker-compose up -d redis, 单独构建redis容器


### 5.2 配置laravel项目

A. 修改laravel的 .env文件

```ini
REDIS_HOST=redis #redis容器的名称
REDIS_PASSWORD=null
REDIS_PORT=6379
```

### 5.3 创建测试控制器

1. 进入php7的容器
2. 执行`php artisan make:controller TestApi` 生成测试控制器

```php
namespace App\Http\Controllers;
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Redis;

class TestApi extends Controller
{
    //
    public function redis(Request $request)
    {
        $key = 'redis_cache'; // 缓存的key
        $data = Redis::get($key); // 从缓存中获取数据
        if (!$data) {
            $data = [
                'name' => 'John Doe',
                'age' => 30,
                'time' => date("Y-m-d H:i:s")
            ];
            Redis::set($key, json_encode($data), 10); // 将数据保存到缓存中
        }
        return response()->json(json_decode(Redis::get($key), true));
    }
}
```
3. 修改路由文件 `routes\api.php`, 添加路由

```php
Route::get('/redis', "App\Http\Controllers\TestApi@redis");
```
4. 执行 `php artisan route:list` 查看路由
5. postman中访问测试接口 `localhost/api/redis`, **done**

```js
{
    "name": "John Doe",
    "age": 30,
    "time": "2023-11-04 06:48:01"
}
```

## 6.测试rabbitmq

## 7.测试elasticsearch

## 8.测试mongodb

## 9.测试webman部署

## 10.研究curl, SSL

## 11.测试定时任务和supervisor
