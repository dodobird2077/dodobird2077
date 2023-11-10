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

// 冒泡排序 (错误写法)
function BubbleSort($arr) 
{ 
    for($i = 0; $i < count($arr); $i++) { 
        for($j = 0; $j < count($arr); $j++) { 
            echo "[i:{$i}]={$arr[$i]},  [j:{$j}]={$arr[$j]} " . PHP_EOL;
            if ($arr[$i] > $arr[$j]) { 
                $num = $arr[$i];
                $arr[$i] = $arr[$j];
                $arr[$j] = $num;
            } 
        } 
    } 
    return $arr; 
}

// 冒泡排序, 正确写法
function BubbleSortTrue($arr) 
{ 
    // (外层)循环控制冒泡的(轮数)
    for ($i = 0; $i < count($arr); $i++) { 

        // (内层)循环用于比较相邻元素并进行(交换)
        // 注意：每轮冒泡将当前未排序部分的最大元素移动到正确的位置
        // 因此，内层循环不需要再考虑已经排序好的元素，从而提高效率

        for ($j = 0; $j < count($arr) - $i - 1; $j++) { 
            // 如果前一个元素大于后一个元素，则交换它们的位置
            if ($arr[$j] > $arr[$j + 1]) {
                // 交换元素
                $temp = $arr[$j];
                $arr[$j] = $arr[$j + 1];
                $arr[$j + 1] = $temp;
            }
        } 
    } 
    // 返回排序好的数组
    return $arr; 
} 

$array = [4, 2, 7, 1, 9, 5];
$res = BubbleSortTrue($array); 
print_r($res);
// 结果 [1,2,4,5,7,9]
```

![logo](../_media/img/suanfa/bubble.gif)

- 图中可以看出：
    -   第一轮排序将数字 6 移动到最右边；
    -   第二轮排序将数字 5 移动到最右边，同时中途将 1 和 2 排了序；
    -   第三轮排序时，没有发生交换，表明排序已经完成，不再继续比较

- `$i, 就是用来控制多少轮的, $j则是用来进行交换的`


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

### 2.2 自己显示substr方法, 主要是通过: `$str[$i]`来进行字符串的截取

