# 练习题

## A.方向

### 一.

##### 语言层面：
了解 PHP 8 语法特征，与 PHP 7 有什么不同，带来了那些改变。
了解现代框架，了解其高性能的原因，未来大部分 PHP 项目很可能都会基于此构建，比如 Workerman。Workerman 是什么
掌握的框架和熟练使用的某一框架。

#####  数据层面
MYSQL 的优化、索引、事务、数据类型，每一项都可以深入复习下。
盘点那些被问烂了的 Mysql 面试题

Redis 的基本知识、语法、数据类型，各种数据类型的运用环境。
在面试前必须要知道的 Redis 面试内容

关键事故术语背后是什么比如：缓存雪崩、穿透、死锁
Redis 击穿、穿透、雪崩产生原因以及解决思路

##### 网络层面
- TCP/IP 、HTTP1/HTTP2、套接字
- TCP/IP 协议
- 软件工程、架构
- 编程思想
- 代码风格 PHP 代码简洁之道
- 主流架构
- 算法

### 二
- https://www.kancloud.cn/martist/be_new_friends
- https://www.kancloud.cn/martist/phper-will-get-bat-tmd-offer-in-2021

### 三

看看 redis、mysql 方面的书，比如 redis 的《redis 运维与开发》、《redis 设计与实现》，mysql 方面可以看看掘金小册 小孩子的《从根上理解 mysql》。
计算机网络的话主要，可以看看 tcp 三次握手、四次挥手这些老掉牙的东西，还有 web 安全方面，比如 SQL 注入、XSS、CSRF 防范，还有设计模式，
另外 php 框架的源码也得看看一两个，主要生命周期，依赖注入、服务定位器什么之类。

## 1.手写算法

### 1.1 php实现冒泡排序

```php
function bubbleSort($arr) {
    $len = count($arr);
    for ($i = 0; $i < $len - 1; $i++) {
        for ($j = 0; $j < $len - $i - 1; $j++) {
            if ($arr[$j] > $arr[$j + 1]) {
            }
        }
    }
}
```

### 1.2 二分查找算法

```php
function binarySearch($arr, $target) {
    $len = count($arr);
    $left = 0;
    $right = $len - 1;
    while ($left <= $right) {
        $mid = floor(($left + $right) / 2);
        if ($arr[$mid] == $target)
    }
}
```


## 2.常见练习题

### 2.1 php新特性

#### 2.1.1 php7新特性

#### 2.1.2 php7.4新特性

#### 2.1.2 php8新特性
