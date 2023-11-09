# Day12.登录功能,JWT使用,过滤器, 拦截器


## Part.A 登录功能的实现

- 1.添加LoginController

```java
package com.tlias.controller;

import com.tlias.pojo.Emp;
import com.tlias.pojo.Result;
import com.tlias.service.EmpService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class LoginController {

    @Autowired
    public EmpService empService;

    @PostMapping("/login")
    public Result login(@RequestBody Emp emp) {
        Emp e = empService.login(emp);
        return e != null ? Result.success() : Result.error("账号或者密码错误");
    }
}
```

- 2.1 EmpService 中

```java
// 省略
public interface EmpService {
    // 省略
    Emp login(Emp emp);
}
```

- 2.2 service实现类EmpServiceImpl中

```java
package com.tlias.service.impl;

@Service
public class EmpServiceImpl implements EmpService {
    // 省略
     
    @Override
    public Emp login(Emp emp) {
        return empMapper.getByUsernameAndPassword(emp);
    }

}
```

- 3.Mapper中

```java
package com.tlias.mapper;
// 省略

@Mapper
public interface EmpMapper {

    // 省略

    @Select("SELECT * from emp where username=#{username} and password=#{password}")
    Emp getByUsernameAndPassword(Emp emp);
}
```


![logo](../../_media/img/java/day12_a1.png ':size=500')

## Part.B JTW令牌的生成和校验

![logo](../../_media/img/java/day12_b1.png ':size=500')

- 单元测试代码如下

```java
package com.tlias;

import io.jsonwebtoken.Claims;
import io.jsonwebtoken.Jwts;
import io.jsonwebtoken.SignatureAlgorithm;
import org.junit.jupiter.api.Test;
import org.springframework.boot.test.context.SpringBootTest;

import java.util.Date;
import java.util.HashMap;
import java.util.Map;

// 如果用不到Springboot的东西, 可以暂时注释掉, 编译速度会更快
//@SpringBootTest
class TliasApplicationTests {


    @Test
    void contextLoads() {
    }

    // 1.生成jwt令牌
    @Test
    public void testJWT() {

        // claims, 英文是申明. 宣称的意思
        Map<String, Object> claims = new HashMap<>();
        claims.put("id", 1);
        claims.put("name", "tome");

        String jwt = Jwts.builder()
            .signWith(SignatureAlgorithm.HS256, "itheima") // 1.签名算法
            .setClaims(claims)  // 2.自定义内容
            .setExpiration(new Date(System.currentTimeMillis()+ 3600 * 1000))  // 3.设置有效时间为1h
            .compact();

        System.out.println(jwt);
    }


    // 2.解析jwt令牌
    @Test
    public void testParseJwt() {
        String token = "eyJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoidG9tZSIsImlkIjoxLCJleHAiOjE2OTUwMDQ3NDV9.GveXfqiHGyn74JrygTGdg7rYu16fDlShGhhdQiYnQRY";
        Claims claims = Jwts.parser()
                .setSigningKey("itheima")
                .parseClaimsJws(token)
                .getBody();
        System.out.println(claims);
    }
}
```



## Part.C 登录后下发Token


##### 注意事项:

- 需要在pom中添加一个依赖解决jwt在jdk11的版本问题

```xml
<!-- 为了解决jwt报错的问题   -->
<dependency>
    <groupId>javax.xml.bind</groupId>
    <artifactId>jaxb-api</artifactId>
    <version>2.3.1</version>
</dependency>
```

#### 代码
- 1.控制器

```java
package com.tlias.controller;

import com.tlias.pojo.Emp;
import com.tlias.pojo.Result;
import com.tlias.service.EmpService;
import com.tlias.utils.JwtUtils;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RestController;

import java.util.HashMap;
import java.util.Map;

@RestController
public class LoginController {

    @Autowired
    public EmpService empService;

    @PostMapping("/login")
    public Result login(@RequestBody Emp emp) {
        Emp e = empService.login(emp);

        //
        if (e != null) {
            Map<String, Object> claims = new HashMap<>();
            claims.put("id", e.getId());
            claims.put("name", "张三");
            claims.put("username", "zhangsan");

            // 调用自定义的工具类
            String jwt = JwtUtils.generateJwt(claims);

            return Result.success(jwt);
        } else {
            return Result.error("账号或者密码错误");
        }
    }
}
```

- 2.jwt工具类(创建对应目录)

```java
package com.tlias.utils;

import io.jsonwebtoken.Claims;
import io.jsonwebtoken.Jwts;
import io.jsonwebtoken.SignatureAlgorithm;
import java.util.Date;
import java.util.Map;

public class JwtUtils {

    private static String signKey = "itheima";
    private static Long expire = 43200000L;

    /**
     * 生成JWT令牌
     * @param claims JWT第二部分负载 payload 中存储的内容
     * @return
     */
    public static String generateJwt(Map<String, Object> claims){
        String jwt = Jwts.builder()
                .addClaims(claims)
                .signWith(SignatureAlgorithm.HS256, signKey)
                .setExpiration(new Date(System.currentTimeMillis() + expire))
                .compact();
        return jwt;
    }

    /**
     * 解析JWT令牌
     * @param jwt JWT令牌
     * @return JWT第二部分负载 payload 中存储的内容
     */
    public static Claims parseJWT(String jwt){
        Claims claims = Jwts.parser()
                .setSigningKey(signKey)
                .parseClaimsJws(jwt)
                .getBody();
        return claims;
    }
}
```


## Part.D 过滤器

##### 说明
- 1.过滤器filter并不是SpringBoot的功能, 所以需要加上特殊的注解
- 2.每个http请求, 都会先经过拦截器, 放行之后才能继续前往对应的控制器方法


#### 步骤
- 1.创建filter目录, 和测试过滤器 `DemoFilter`

```java
package com.tlias.filter;

import javax.servlet.*;
import javax.servlet.annotation.WebFilter;
import java.io.IOException;

@WebFilter(urlPatterns = "/*")
public class DemoFilter implements Filter {

    @Override // 初始化方法, 只调用一次
    public void init(FilterConfig filterConfig) throws ServletException {
        System.out.println("初始化方法调用");
        Filter.super.init(filterConfig);
    }

    @Override // 拦截到请求之后调用, 多次
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        System.out.println("拦截到请求");

        // 放行
        filterChain.doFilter(servletRequest, servletResponse);
    }

    @Override
    public void destroy() {
        System.out.println("销毁方法调用");
        Filter.super.destroy();
    }
}
```

- 2.springboot入口文件添加特殊注解

```java
package com.tlias;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.web.servlet.ServletComponentScan;

@ServletComponentScan // 开启对Servlet的支持: 由于拦截器不是springboot内置的, 需要添加支持
@SpringBootApplication
public class TliasApplication {

    public static void main(String[] args) {
        SpringApplication.run(TliasApplication.class, args);
    }

}
```

### D2. 通过过滤器完成登录校验

![logo](../../_media/img/java/day12_d1.png ':size=500')

#### 代码
- 1.过滤器代码 (多余的过滤器, 注释掉@WebFilter即可)
```java
package com.tlias.filter;

import com.alibaba.fastjson.JSONObject;
import com.github.pagehelper.util.StringUtil;
import com.tlias.pojo.Result;
import com.tlias.utils.JwtUtils;
import lombok.extern.slf4j.Slf4j;
import org.springframework.util.StringUtils;

import javax.servlet.*;
import javax.servlet.annotation.WebFilter;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@Slf4j
@WebFilter(urlPatterns = "/*")
public class LoginCheckFilter implements Filter {


    @Override // 拦截到请求之后调用, 多次
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {

        HttpServletRequest req = (HttpServletRequest) servletRequest;
        HttpServletResponse resp = (HttpServletResponse) servletResponse;


        // 1.获取请求的URL
        String url  = req.getRequestURL().toString();
        log.info("请求的URL:{}", url);

        // 2.判断请求是否包含url, 如果包含直接放行
        if (url.contains("login")) {
            filterChain.doFilter(servletRequest, servletResponse);
            return; // 让代码不继续执行
        }

        // 3.获取请求头的信息
        String jwt = req.getHeader("token");

        // 4.判断令牌是否存在如果不存在返回错误(未登录)
        if (!StringUtils.hasLength(jwt)) {
            log.info("请求头token为空, 返回未登录信息");
            Result error = Result.error("NOT_LOGIN");

            // 手动转换 对象转Json
            String notLogin = JSONObject.toJSONString(error);
            resp.getWriter().write(notLogin);
            return;
        }

        // 5.如果jtw存在,校验; 如果解析失败会抛出异常, 捕捉到之后返回登录失败
        try {
            JwtUtils.parseJWT(jwt);
        } catch (Exception e) {
            log.info("token校验失败, 返回未登录信息");
            Result error = Result.error("NOT_LOGIN");

            // 手动转换 对象转Json
            String notLogin = JSONObject.toJSONString(error);
            resp.getWriter().write(notLogin);
            return;
        }

        // 6.放行
        filterChain.doFilter(servletRequest, servletResponse);
        System.out.println("校验成功放行");
    }


}
```

- 2.这里添加了一个pom依赖包
```xml
<!-- 阿里巴巴提供的对象转json   -->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>fastjson</artifactId>
    <version>1.2.76</version>
</dependency>
```

- 3.这里再贴一下JwtUtils的代码, 虽然未修改, 值得关注的是, 解析失败会抛出异常
```java
package com.tlias.utils;

import io.jsonwebtoken.Claims;
import io.jsonwebtoken.Jwts;
import io.jsonwebtoken.SignatureAlgorithm;
import java.util.Date;
import java.util.Map;

public class JwtUtils {

    private static String signKey = "itheima";
    private static Long expire = 43200000L;

    /**
     * 生成JWT令牌
     * @param claims JWT第二部分负载 payload 中存储的内容
     * @return
     */
    public static String generateJwt(Map<String, Object> claims){
        String jwt = Jwts.builder()
                .addClaims(claims)
                .signWith(SignatureAlgorithm.HS256, signKey)
                .setExpiration(new Date(System.currentTimeMillis() + expire))
                .compact();
        return jwt;
    }

    /**
     * 解析JWT令牌(解析失败会抛出异常)
     * @param jwt JWT令牌
     * @return JWT第二部分负载 payload 中存储的内容
     */
    public static Claims parseJWT(String jwt){
        Claims claims = Jwts.parser()
                .setSigningKey(signKey)
                .parseClaimsJws(jwt)
                .getBody();
        return claims;
    }
}
```

## Part.E 拦截器


#### 说明: Interceptor
- 1.拦截器是Springboot提供的, 而过滤器是Java Servlet的
- 2.这里使用拦截器替换掉Filter做登陆校验, Filter注释掉注解
- 3.创建目录interceptor用于存放定义的拦截器
- 4.创建目录config用于注册拦截器

![logo](../../_media/img/java/day12_E1.png ':size=500')

#### 拦截器入门和使用拦截器作为登录校验

1. 定义拦截器 `com/tlias/interceptor/LoginCheckInterceptor.java`

```java
package com.tlias.interceptor;

import com.alibaba.fastjson.JSONObject;
import com.tlias.pojo.Result;
import com.tlias.utils.JwtUtils;
import lombok.extern.slf4j.Slf4j;
import org.springframework.stereotype.Component;
import org.springframework.util.StringUtils;
import org.springframework.web.servlet.HandlerInterceptor;
import org.springframework.web.servlet.ModelAndView;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * 步骤1. 定义拦截器
 */

@Slf4j
@Component // 加入容器中
public class LoginCheckInterceptor  implements HandlerInterceptor {
    @Override // 目标资源方法运行前运行, 返回true放行; 返回false不放行
    public boolean preHandle(HttpServletRequest req, HttpServletResponse resp, Object handler) throws Exception {


        // 1.获取请求的URL
        String url  = req.getRequestURL().toString();
        log.info("请求的URL:{}", url);

        // 2.判断请求是否包含url, 如果包含直接放行
        if (url.contains("login")) {
            return true;
        }

        // 3.获取请求头的信息
        String jwt = req.getHeader("token");

        // 4.判断令牌是否存在如果不存在返回错误(未登录)
        if (!StringUtils.hasLength(jwt)) {
            log.info("请求头token为空, 返回未登录信息");
            Result error = Result.error("NOT_LOGIN");

            // 手动转换 对象转Json
            String notLogin = JSONObject.toJSONString(error);
            resp.getWriter().write(notLogin);
            return false;
        }

        // 5.如果jtw存在,校验; 如果解析失败会抛出异常, 捕捉到之后返回登录失败
        try {
            JwtUtils.parseJWT(jwt);
        } catch (Exception e) {
            log.info("token校验失败, 返回未登录信息");
            Result error = Result.error("NOT_LOGIN");

            // 手动转换 对象转Json
            String notLogin = JSONObject.toJSONString(error);
            resp.getWriter().write(notLogin);
            return false;
        }

        // 6.放行
        System.out.println("校验成功放行");
        return  true;
    }

    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        System.out.println("postHandle...");
    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        System.out.println("afterCompletion...");
    }
}
```

2. 注册拦截器 `com/tlias/config/WebConfig.java`

```java
package com.tlias.config;

import com.tlias.interceptor.LoginCheckInterceptor;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.InterceptorRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

/**
 *   1. /** 表示任何层级路径
 *   2. /* 表示一级路径
 */

@Configuration // 配置类
public class WebConfig implements WebMvcConfigurer {

    @Autowired
    private LoginCheckInterceptor loginCheckInterceptor;

    @Override
    public void addInterceptors(InterceptorRegistry registry) {

        // 步骤2.注册拦截器
        registry.addInterceptor(loginCheckInterceptor)
                .addPathPatterns("/**");
    }
}
```

## Part.F 全局异常处理


##### 核心知识点
- @RestControllerAdvice
- @ExceptionHandler(Exception.class)

##### 步骤
1. 添加一个文件夹 `expection`
2. 添加异常处理类

```java
package com.tlias.expection;

import com.tlias.pojo.Result;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.RestControllerAdvice;

@RestControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(Exception.class)
    public Result ex(Exception exObj) {
        exObj.printStackTrace();
        return Result.error("对不起,操作失败, 请联系管理员");
    }
}
```


![logo](../../_media/img/java/day12_F1.png ':size=800')