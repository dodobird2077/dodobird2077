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
