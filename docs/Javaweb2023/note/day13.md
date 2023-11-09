# Day 13: 事务,AOP

## 一.事务

### A.事务管理入门

##### 说明
- 1.给Service层的方法加个注释就可以了, 比PHP的简单很多


##### 步骤
以删除部门时, 同时删除部门下的员工为场景

- 1.部门管理控制器中, 不用改动 (代码省略部分)

```java
package com.tlias.controller;

@RestController
public class DeptController {

    @DeleteMapping("/depts/{id}")
    public Result delete(@PathVariable Integer id) {
        log.info("根据ID删除部门", id);
        deptService.delete(id);
        return Result.success();
    }

}
```

- 2.部门Service实现

```java
package com.tlias.service.impl;

import com.tlias.mapper.DeptMapper;
import com.tlias.mapper.EmpMapper;
import com.tlias.pojo.Dept;
import com.tlias.service.DeptService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

import java.time.LocalDateTime;
import java.util.List;

@Service
public class DeptServiceImpl implements DeptService {

    // 调用部门管理的Mapper(Service调用DAO层)
    @Autowired
    private DeptMapper deptMapper;
    @Autowired
    private EmpMapper empMapper;

    @Override
    public List<Dept> list() {
        return deptMapper.list();
    }

    @Transactional //将事务交给SpringBoot处理
    @Override
    public void delete(Integer id) {

        // 1.1删除部门
        deptMapper.deleteById(id);

        // 强制抛出异常进行测试
        // Integer res = 1 / 0;

        // 2.删除员工
        empMapper.deleteByDeptId(id);
    }

    public void add(Dept dept) {
        dept.setCreateTime(LocalDateTime.now());
        dept.setUpdateTime(LocalDateTime.now());
        deptMapper.add(dept);
    }

    public void update(Dept dept)
    {
        dept.setUpdateTime(LocalDateTime.now());
        deptMapper.update(dept);
    }

    public Dept view(Integer id)
    {
        return deptMapper.view(id);
    }

}
```

- 3.员工管理Mapper修改,添加删除方法

```java
package com.tlias.mapper;

import com.tlias.pojo.Emp;
import org.apache.ibatis.annotations.Delete;
import org.apache.ibatis.annotations.Insert;
import org.apache.ibatis.annotations.Mapper;
import org.apache.ibatis.annotations.Select;
import org.springframework.web.bind.annotation.DeleteMapping;

import java.time.LocalDate;
import java.util.List;

@Mapper
public interface EmpMapper {

    // 使用pageHelper来分页, 这里正常写
//    @Select("SELECT * from emp ") // 注释掉, 就会使用xml
    public List<Emp> list(String name, Short gender, LocalDate begin, LocalDate end);

    void delete(List<Integer> ids);


    @Insert("insert into emp (username, name, gender, image, job, entrydate, dept_id, create_time, update_time) " +
            "values (#{username}, #{name}, #{gender}, #{image}, #{job}, #{entrydate}, #{deptId}, #{createTime}, #{updateTime})")
    void insert(Emp emp);

    @Select("SELECT * from emp where id=#{id} limit 1")
    Emp getById(Integer id);


    void update(Emp emp);

    @Select("SELECT * from emp where username=#{username} and password=#{password}")
    Emp getByUsernameAndPassword(Emp emp);
    
    // 添加根据部门ID删除员工
    @Delete("delete from emp where dept_id=#{deptId}")
    void deleteByDeptId(Integer deptId);
}
```

- 4.添加事务日志, 在yml文件中

```yml
#spring事务管理日志
logging:
  level:
    org.springframework.jdbc.support.JdbcTransactionManager: debug
```

### B.事务进阶-rollbackFor



##### 事务进阶
1. rollbackFor
2. propagation

# 一.rollbackFor (发生异常回滚的条件)

##### 默认情况下，只有出现 `RuntimeException`(运行时异常) 才回滚异常。
##### rollbackFor属性用于控制出现何种异常类型，回滚事务。


- 1.服务实现类中

```java
@Transactional(rollbackFor = Exception.class) // 指定出现所有的异常都回滚
@Override
public void delete(Integer id) throws Exception {

    // 1.1删除部门
    deptMapper.deleteById(id);

    // 强制抛出异常进行测试
    // Integer res = 1 / 0;
    if (true) {
        throw new Exception("test");
    }

    // 2.删除员工
    empMapper.deleteByDeptId(id);
}
```


- 2.控制器这里要加上throw Exception 因为服务实现类中加了(这个没在课程中说)

```java
@DeleteMapping("/depts/{id}")
public Result delete(@PathVariable Integer id) throws Exception {
    log.info("根据ID删除部门", id);
    deptService.delete(id);
    return Result.success();
}
```

#### 二.propagation (事务传播,嵌套)

![logo](../../_media/img/java/day13_A1.png ':size=500')

- `REQUIRED` ：大部分情况下都是用该传播行为即可。
- `REQUIRES_NEW` ：当我们不希望事务之间相互影响时，可以使用该传播行为。比如：下订单前需要记录日志，不论订单保存成功与否，都需要保证日志记录能够记录成功。

![logo](../../_media/img/java/day13_A2.png ':size=500')




## 二.AOP


### A2.AOP入门Demo-面向方法编程


面向方法编程, 面向切面编程

#### 介绍
- 典型应用场景:
1. 记录操作日志
2. 权限控制
3. 事务管理

- 优势:
1. 代码无入侵(无需修改原代码, 就可以实现功能)
2. 减少重复代码
3. 提高效率
4. 维护方便

- 案例
1. 在不修改原代码的情况下, 统计每个Service层方法的运行时间

![logo](../../_media/img/java/day13_A21.png ':size=500')

#### 代码
- 1.添加pom依赖

```xml
<!--AOP-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-aop</artifactId>
</dependency>
```

- 2.添加AOP类

```java
package com.tlias.aop;

import lombok.extern.slf4j.Slf4j;
import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.springframework.stereotype.Component;

@Slf4j
@Component
@Aspect // 定义为AOP类
public class TimeAspect {

    @Around("execution(* com.tlias.service.*.*(..))") // 切入点表达式
    public Object recordTime(ProceedingJoinPoint joinPoint) throws Throwable {
        // 1.记录开始时间
        long begin = System.currentTimeMillis();

        // 2.调用原始方法运行
        Object result = joinPoint.proceed();

        // 3.记录结束时间,计算执行耗时
        long end = System.currentTimeMillis();

        log.info(joinPoint.getSignature() + "执行耗时,{}ms", (end - begin));

        return result; //注意要return 否则service层可能获取不到post数据
    }
}
```

### B2.AOP核心概念,名词


- 核心的五个概念 - 五条悟(不用死记硬背)
1. `连接点: JoinPoint`
    - 可以被AOP控制的方法(里面含有方法执行时的相关信息)
2. `通知: Advice`
    - 指那些重复的逻辑代码(比如我在方法开始前记录开始时间, 开始后记录结束时间的代码), 将这些代码抽离出来就是Advice(最终体现在一个方法)
3. `切入点: PointCut`
    - 匹配链接点的条件(就是@Around里面指定的哪些类,方法会执行AOP) `Advice` 仅会在切入点匹配到的方法执行时被调用(例如切入点指定service层的代码, 那么Service层login方法执行时, 就会触发Advice)
4. `切面：Aspect`
    - 描述通知与切入点的对应关系（通知+切入点）- 待理解
5. `目标对象：Target`
    - 通知所应用的对象

![logo](../../_media/img/java/day13_B2_1.png ':size=500')


### C2.AOP的通知,切入点,连接点(概念)



#### 一.通知

- 1. 看起来就像Vue的生命周期函数

![logo](../../_media/img/java/day13_C2_1.png ':size=500')

![logo](../../_media/img/java/day13_C2_2.png ':size=500')

- 2.通知顺序
由类名决定, 也可以给@Order(1),排序


![logo](../../_media/img/java/day13_C2_3.png ':size=500')

#### 二.切入点表达式(了解)

`先了解, 用到时候再查; 感觉就是一个正则表达式一样, 用于匹配你要切入哪些方法(请求等)`

![logo](../../_media/img/java/day13_C2_4.png ':size=500')


- 5.2@annotation根据注解匹配切入点
![logo](../../_media/img/java/day13_C2_6.png ':size=500')

#### 三.连接点(JoinPoint)

`可以被AOP控制的方法, 被称为连接点`

```java
package com.tlias.aop;
import lombok.extern.slf4j.Slf4j;
import org.aspectj.lang.JoinPoint;
import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.aspectj.lang.annotation.Pointcut;
import org.springframework.stereotype.Component;
import java.util.Arrays;
//切面类
@Slf4j
@Aspect
@Component
public class MyAspect8 {
    @Pointcut("execution(* com.tlias.service.DeptService.*(..))")
    private void pt(){}
    @Before("pt()")
    public void before(JoinPoint joinPoint){
        log.info("MyAspect8 ... before ...");
    }
    @Around("pt()")
    public Object around(ProceedingJoinPoint joinPoint) throws Throwable {
        log.info("MyAspect8 around before ...");
        //1. 获取 目标对象的类名 .
        String className = joinPoint.getTarget().getClass().getName();
        log.info("目标对象的类名:{}", className);
        //2. 获取 目标方法的方法名 .
        String methodName = joinPoint.getSignature().getName();
        log.info("目标方法的方法名: {}",methodName);
        //3. 获取 目标方法运行时传入的参数 .
        Object[] args = joinPoint.getArgs();
        log.info("目标方法运行时传入的参数: {}", Arrays.toString(args));
        //4. 放行 目标方法执行 .
        Object result = joinPoint.proceed();
        //5. 获取 目标方法运行的返回值 .
        log.info("目标方法运行的返回值: {}",result);
        log.info("MyAspect8 around after ...");
        return result;
    }
}
```


### D2.综合案例-AOP实现日志记录

#### 目标 :
将案例中的增删改相关接口操作的日志, 记录到数据表中
日志信息包含: 操作人, 操作时间, 执行方法的全类名, 执行方法名, 方法运行参数, 返回值, 方法运行时长

#### 实现方式:
使用AOP, 并使用Around通知的方式

#### 准备工作:
	1. 在案例工程中引入AOP起步依赖
	2. 导入资料中准备好数据表结构, 并创建实体类pojo
	
#### 编码:
	1. 自定义注解@Log
        2. 定义切面类, 完成记录操作日志的逻辑代码
        
#### 相关准备

- 1.起步依赖

```xml
 <!--AOP-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-aop</artifactId>
</dependency>
```

- 2.日志表

```sql
-- 操作日志表
create table operate_log(
    id int unsigned primary key auto_increment comment 'ID',
    operate_user int unsigned comment '操作人ID',
    operate_time datetime comment '操作时间',
    class_name varchar(100) comment '操作的类名',
    method_name varchar(100) comment '操作的方法名',
    method_params varchar(1000) comment '方法参数',
    return_value varchar(2000) comment '返回值',
    cost_time bigint comment '方法执行耗时, 单位:ms'
) comment '操作日志表';
```

#### 步骤

- 1.创建日志实体类(模型对象)

```java
package com.tlias.pojo;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

import java.time.LocalDateTime;

@Data
@NoArgsConstructor
@AllArgsConstructor
public class OperateLog {
    private Integer id; //ID
    private Integer operateUser; //操作人ID
    private LocalDateTime operateTime; //操作时间
    private String className; //操作类名
    private String methodName; //操作方法名
    private String methodParams; //操作方法参数
    private String returnValue; //操作方法返回值
    private Long costTime; //操作耗时
}
```

- 2.创建日志操作类Mapper

```java
package com.tlias.mapper;

import com.tlias.pojo.OperateLog;
import org.apache.ibatis.annotations.Insert;
import org.apache.ibatis.annotations.Mapper;

@Mapper
public interface OperateLogMapper {

    //插入日志数据
    @Insert("insert into operate_log (operate_user, operate_time, class_name, method_name, method_params, return_value, cost_time) " +
            "values (#{operateUser}, #{operateTime}, #{className}, #{methodName}, #{methodParams}, #{returnValue}, #{costTime});")
    public void insert(OperateLog log);

}
```

- 3.创建自定义注解类

```java
package com.tlias.anno;

import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)

//  Log类是自定义注解类
public @interface Log {
}
```

 - 4.创建AOP切面类用来记录日志

```java
package com.tlias.aop;

import com.alibaba.fastjson.JSONObject;
import com.tlias.mapper.OperateLogMapper;
import com.tlias.pojo.OperateLog;
import com.tlias.utils.JwtUtils;
import io.jsonwebtoken.Claims;
import lombok.extern.slf4j.Slf4j;
import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

import javax.servlet.http.HttpServletRequest;
import java.time.LocalDateTime;
import java.util.Arrays;

@Slf4j
@Component
@Aspect
public class LogAspect {

    // http请求类, 用于获取请求参数
    @Autowired
    private HttpServletRequest request;

    // 日志记录类
    @Autowired
    private OperateLogMapper operateLogMapper;

    @Around("@annotation(com.tlias.anno.Log)")
    public Object recordLog(ProceedingJoinPoint joinPoint) throws Throwable {

        // 调用目标方法运行,
        Object result = joinPoint.proceed();


        // 操作人: 从jwt中获取
        String jwt = request.getHeader("token");
        Claims claims = JwtUtils.parseJWT(jwt);
        Integer operateUser = (Integer) claims.get("id");

        // 操作时间
        LocalDateTime operateTime = LocalDateTime.now();

        // 操作类名
        String className = joinPoint.getTarget().getClass().getName();

        // 方法名
        String methodName = joinPoint.getSignature().getName();

        // 操作方法参数
        Object[] args = joinPoint.getArgs();
        String methodParams = Arrays.toString(args);

        // 方法返回值
        long begin = System.currentTimeMillis();
        Object res = joinPoint.proceed();
        long end = System.currentTimeMillis();

        String returnValue = JSONObject.toJSONString(res); // 方法返回值

        // 方法耗时
        long costTime = end - begin;

        // 记录日志
        // 实例化操作日志, 然后获取操作日志的值
        OperateLog operateLog = new OperateLog(null, operateUser, operateTime, className, methodName, methodParams, returnValue, costTime);
        operateLogMapper.insert(operateLog);

        log.info("AOP操作记录日志: {}", operateLog);
        return result;
    }
}
```

- 5.在目标控制器中的增删改方法中添加@Log注解, 然后AOP通过切入点表达式匹配到这些方法, 从而进行切入

```java
package com.tlias.controller;

public class DeptController {
       
    // 省略部分代码  

    @DeleteMapping("/depts/{id}")
    @Log
    public Result delete(@PathVariable Integer id) throws Exception {
        log.info("根据ID删除部门", id);
        deptService.delete(id);
        return Result.success();
    }

    @PostMapping("/depts")
    @Log
    public Result add(@RequestBody Dept dept) {
        log.info("新增部门:{}", dept);
        deptService.add(dept);
        return Result.success();
    }

    @PutMapping("/depts")
    @Log
    public Result update(@RequestBody Dept dept)
    {
        deptService.update(dept);
        return Result.success();
    }

}
```

