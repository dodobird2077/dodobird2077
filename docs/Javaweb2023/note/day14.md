# SpringBoot原理

## 一.打包和运行, 配置顺序等

### 打包:
1.打开右边的Maven板块, tlias - Lifecycle - package, 双击打包, 打包生成的文件在: target/tlias-0.0.1-SNAPSHOT.jar

### 运行jar包:
java -jar tlias-0.0.1-SNAPSHOT.jar --server.port=8090


## 二. Bean管理

### 1.获取Bean

![logo](../../_media/img/java/day14_1.png ':size=500')

### 2.Bean管理

默认的bean是单例的, 也就是实例化多少次都是同一个bean对象, 绝大部分用默认的

![logo](../../_media/img/java/day14_2.png ':size=500')

![logo](../../_media/img/java/day14_3.png ':size=500')

### 3.第三方bean
统一放在 config目录下和pojo平级

![logo](../../_media/img/java/day14_4.png ':size=500')




## 三.起步依赖
原理是maven的依赖传递


### 自动配置, 原理


![logo](../../_media/img/java/day14_5.png ':size=500')


### 案例 (还没手动写一遍)

**需求**：自定义aliyun-oss-spring-boot-starter，完成阿里云OSS操作工具类 AliyunOSSUtils 的自动配置。
**目标**：引入起步依赖引入之后，要想使用阿里云OSS，注入 AliyunOSSUtils直接使用即可。


![logo](../../_media/img/java/day14_6.png ':size=500')
