# note2


- 目标是创建第一个spring boot程序, 并且在浏览器能够输入'hello world'

- 1.创建项目

![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/061df6dcd0024c7ab2dd87b7e26d92c1~tplv-k3u1fbpfcp-watermark.image?)

- 2.勾选Spring Web (注意, 选择2.7.x版本就行, 不用选最新的)

![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/2e5f78e6a3504c3a9fac1c46ff0f62bd~tplv-k3u1fbpfcp-watermark.image?)

- 3.创建完之后, 删除多余的文件夹(.idea 也删掉)

![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/23379f8035c345e2b7ff4ff49782147e~tplv-k3u1fbpfcp-watermark.image?)

- 4.创建Controller

![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/2bc9b0382b4a422b8519cdf1daf89ab3~tplv-k3u1fbpfcp-watermark.image?)

- 5.编写代码
``` java
package com.itheima.controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

// 添加Rest注解, 表示它是一个用于请求处理的类
@RestController
public class HelloController {
    // 用注解定义请求路由
    @RequestMapping("/hello")
    public String hello() {
        System.out.println("hello spring");
        return "hello spring";
    }
}
```

- 6.启动SpringBoot

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ec1e9a2fb3af456087d6bf17ad7ff083~tplv-k3u1fbpfcp-watermark.image?)

- 7.打开浏览器
http://localhost:8080/hello
输出: hello spring
