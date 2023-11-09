# `Day10` - 开始案例, 准备工作和第一个接口, 部门列表接口


## PartA: 准备工作

- 如图, 导入数据库和创建对应的文件

![logo](../../_media/img/java/day10_a1.png ':size=800')

- 1.数据库

```sql
-- 部门管理
create table dept(
    id int unsigned primary key auto_increment comment '主键ID',
    name varchar(10) not null unique comment '部门名称',
    create_time datetime not null comment '创建时间',
    update_time datetime not null comment '修改时间'
) comment '部门表';

insert into dept (id, name, create_time, update_time) values(1,'学工部',now(),now()),(2,'教研部',now(),now()),(3,'咨询部',now(),now()), (4,'就业部',now(),now()),(5,'人事部',now(),now());



-- 员工管理(带约束)
create table emp (
  id int unsigned primary key auto_increment comment 'ID',
  username varchar(20) not null unique comment '用户名',
  password varchar(32) default '123456' comment '密码',
  name varchar(10) not null comment '姓名',
  gender tinyint unsigned not null comment '性别, 说明: 1 男, 2 女',
  image varchar(300) comment '图像',
  job tinyint unsigned comment '职位, 说明: 1 班主任,2 讲师, 3 学工主管, 4 教研主管, 5 咨询师',
  entrydate date comment '入职时间',
  dept_id int unsigned comment '部门ID',
  create_time datetime not null comment '创建时间',
  update_time datetime not null comment '修改时间'
) comment '员工表';

INSERT INTO emp
	(id, username, password, name, gender, image, job, entrydate,dept_id, create_time, update_time) VALUES
	(1,'jinyong','123456','金庸',1,'1.jpg',4,'2000-01-01',2,now(),now()),
	(2,'zhangwuji','123456','张无忌',1,'2.jpg',2,'2015-01-01',2,now(),now()),
	(3,'yangxiao','123456','杨逍',1,'3.jpg',2,'2008-05-01',2,now(),now()),
	(4,'weiyixiao','123456','韦一笑',1,'4.jpg',2,'2007-01-01',2,now(),now()),
	(5,'changyuchun','123456','常遇春',1,'5.jpg',2,'2012-12-05',2,now(),now()),
	(6,'xiaozhao','123456','小昭',2,'6.jpg',3,'2013-09-05',1,now(),now()),
	(7,'jixiaofu','123456','纪晓芙',2,'7.jpg',1,'2005-08-01',1,now(),now()),
	(8,'zhouzhiruo','123456','周芷若',2,'8.jpg',1,'2014-11-09',1,now(),now()),
	(9,'dingminjun','123456','丁敏君',2,'9.jpg',1,'2011-03-11',1,now(),now()),
	(10,'zhaomin','123456','赵敏',2,'10.jpg',1,'2013-09-05',1,now(),now()),
	(11,'luzhangke','123456','鹿杖客',1,'11.jpg',5,'2007-02-01',3,now(),now()),
	(12,'hebiweng','123456','鹤笔翁',1,'12.jpg',5,'2008-08-18',3,now(),now()),
	(13,'fangdongbai','123456','方东白',1,'13.jpg',5,'2012-11-01',3,now(),now()),
	(14,'zhangsanfeng','123456','张三丰',1,'14.jpg',2,'2002-08-01',2,now(),now()),
	(15,'yulianzhou','123456','俞莲舟',1,'15.jpg',2,'2011-05-01',2,now(),now()),
	(16,'songyuanqiao','123456','宋远桥',1,'16.jpg',2,'2007-01-01',2,now(),now()),
	(17,'chenyouliang','123456','陈友谅',1,'17.jpg',NULL,'2015-03-21',NULL,now(),now());
```

- 数据库配置文件

```ini
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/tlias
spring.datasource.username=root
spring.datasource.password=root

mybatis.configuration.log-impl=org.apache.ibatis.logging.stdout.StdOutImpl

mybatis.configuration.map-underscore-to-camel-case=true
```

- 统一json返回类, 放在pojo

```java
package com.tlias.pojo;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@Data
@NoArgsConstructor
@AllArgsConstructor
public class Result {
    private Integer code;//响应码，1 代表成功; 0 代表失败
    private String msg;  //响应信息 描述字符串
    private Object data; //返回的数据

    //增删改 成功响应
    public static Result success(){
        return new Result(1,"success",null);
    }
    //查询 成功响应
    public static Result success(Object data){
        return new Result(1,"success",data);
    }
    //失败响应
    public static Result error(String msg){
        return new Result(0,msg,null);
    }
}
```


### 具体代码, 有空再补全

```java
// 入口文件
package com.tlias;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class TliasApplication {
    
    // 在此启动后, 访问 localhost:8080/depts, 如果返回404: 执行mvn clean test
    public static void main(String[] args) {
        SpringApplication.run(TliasApplication.class, args);
    }

}
```

```java
// 控制器
package com.tlias.controller;

import com.tlias.pojo.Result;
import lombok.extern.slf4j.Slf4j;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import java.util.logging.Logger;

@Slf4j
@RestController
public class DeptController {

    //private static Logger log = Logger.getLogger(String.valueOf(DeptController.class));

    @RequestMapping("/depts")
    public Result list() {
        System.out.println("查询全部的部门数据");
        log.info("查询全部的部门数据");
        return Result.success();
    }
}
```

```java
// 此文件暂时还未使用到
package com.tlias.mapper;

import org.apache.ibatis.annotations.Mapper;

@Mapper
public interface DeptMapper {
}
```

```java
// 此文件暂时还未使用到
package com.tlias.service;

public interface DeptService {
}
```

```java
// 此文件暂时还未使用到
package com.tlias.service.impl;

import com.tlias.service.DeptService;
import org.springframework.stereotype.Service;

@Service
public class DeptServiceImpl implements DeptService {

}
```

项目截图

![logo](../../_media/img/java/day10_a2.png ':size=600')

## PartB: 部门列表接口


- 最终结果

![logo](../../_media/img/java/day10_b1.png ':size=600')


- 过程:

![logo](../../_media/img/java/day10_b2.png ':size=600')

- 1.负责接收请求的控制器代码

```java
package com.tlias.controller;

import com.tlias.pojo.Dept;
import com.tlias.pojo.Result;
import com.tlias.service.DeptService;
import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import java.util.List;
import java.util.logging.Logger;

@Slf4j
@RestController
public class DeptController {

    @Autowired
    private DeptService deptService;

    @GetMapping("/depts")
    public Result list() {
        List<Dept> deptList = deptService.list();
        return Result.success(deptList);
    }
}
```

- 2.负责具体业务的Service层代码(Service层调用DAO层)

```java
package com.tlias.service.impl;

import com.tlias.mapper.DeptMapper;
import com.tlias.pojo.Dept;
import com.tlias.service.DeptService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;

@Service
public class DeptServiceImpl implements DeptService {

    // 调用部门管理的Mapper(Service调用DAO层)
    @Autowired
    private DeptMapper deptMapper;

    @Override
    public List<Dept> list() {
        return deptMapper.list();
    }
}
```

- 3.负责数据库查询的的Mapper和实体类
- 3.1 Mapper

```java
package com.tlias.mapper;

import com.tlias.pojo.Dept;
import org.apache.ibatis.annotations.Mapper;
import org.apache.ibatis.annotations.Select;

import java.util.List;

@Mapper
public interface DeptMapper {

    /**
     * 查询全部部门
     * @return
     */
    @Select("select * from dept")
    List<Dept> list();
}
```

- 3.2 数据库实体类

```java
package com.tlias.pojo;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;
import java.time.LocalDateTime;

/**
 * 部门实体类
 */
@Data
@NoArgsConstructor
@AllArgsConstructor
public class Dept {
    private Integer id; //ID
    private String name; //部门名称
    private LocalDateTime createTime; //创建时间
    private LocalDateTime updateTime; //修改时间
}
```


## Part.C 前后端联调


- 注意, 这里使用教程准备的Nginx, 它里面设置好了Nginx 反向代理,端口等, 连前端文件都放在里面
- 将本低的nginx先暂停, 用他这个即可,java的程序启动, 会代理到java的8080端口


![logo](../../_media/img/java/day10_c1.png ':size=300')

[点击此处下载文件](../../_media/file/nginx.zip)


```sh
worker_processes  1;
events {
    worker_connections  1024;
}

http {
    include       mime.types;
    default_type  application/octet-stream;
    sendfile        on;
    keepalive_timeout  65;
    server {
        listen       90;
        server_name  localhost;

        location / {
            root   html;
            index  index.html index.htm;
        }

        location ^~ /api/ {
			rewrite ^/api/(.*)$ /$1 break;
			proxy_pass http://localhost:8080;
        }
        
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }
}
```


![logo](../../_media/img/java/day10_c2.png ':size=700')


## Part.D 分页查询的实现


### 分页查询思路

![logo](../../_media/img/java/day10_d1.png ':size=500')

### 代码实现

- 1. 封装一个PageBean类, 用来返回分页列表数据

```java
package com.tlias.pojo;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;
import java.util.List;

/**
 * 分页查询结果封装类
 */
@Data
@NoArgsConstructor
@AllArgsConstructor
public class PageBean {

    private Long total;//总记录数
    private List rows;//数据列表
}
```

- 2.在控制器中定义Http接口

```java
package com.tlias.controller;

import com.tlias.pojo.PageBean;
import com.tlias.pojo.Result;
import com.tlias.service.EmpService;
import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.format.annotation.DateTimeFormat;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

import java.time.LocalDate;

@Slf4j
@RestController
public class EmpController {

    @Autowired
    private EmpService empService;

    @GetMapping("/emps")
    public Result page(
            @RequestParam(defaultValue = "1") Integer page,
            @RequestParam(defaultValue = "10") Integer pageSize
    )
    {
        log.info("分页查询, 参数: {}, {}", page, pageSize);
        PageBean pageBean = empService.page(page, pageSize);
        return Result.success(pageBean);
    }

}
```

- 3.在Service中定义接口类

```java
package com.tlias.service;

import com.tlias.pojo.PageBean;

public interface EmpService {
    PageBean page(Integer page, Integer pageSize);
}
```

- 4.接口实现类中实现分页方法, 调用DAO层的Mapper

```java
package com.tlias.service.impl;

import com.tlias.mapper.EmpMapper;
import com.tlias.pojo.Emp;
import com.tlias.pojo.PageBean;
import com.tlias.service.EmpService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;

@Service
public class EmpServiceImpl implements EmpService {

    @Autowired
    private EmpMapper empMapper;


    @Override
    public PageBean page(Integer page, Integer pageSize) {

        // 1.获取总记录数
        Long count = empMapper.count(); // 总数目

        // 2.获取分页结果列表
        Integer start = (page - 1) * pageSize;
        List<Emp> empList = empMapper.page(start, pageSize);

        // 3.封装pageBean
        PageBean pageBean = new PageBean(count, empList);

        return pageBean;
    }
}
```

- 5.Mapper(DAO层) 查询数据库

```java
package com.tlias.mapper;

import com.tlias.pojo.Emp;
import org.apache.ibatis.annotations.Mapper;
import org.apache.ibatis.annotations.Select;

import java.util.List;

@Mapper
public interface EmpMapper {

    @Select("SELECT count(*) from emp")
    public Long count();

    @Select("SELECT * from emp limit #{start},#{pageSize}")
    public List<Emp> page(Integer start, Integer pageSize);
}
```


## Part.E 分页插件PageHelper


### 分页插件 PageHelper

- 1.在pom导入依赖

```xml
<dependency>
    <groupId>com.github.pagehelper</groupId>
    <artifactId>pagehelper-spring-boot-starter</artifactId>
    <version>1.4.2</version>
</dependency>
```

- 2.注释掉原来的EmpMapper方法, 添加普通的select查询

```java
package com.tlias.mapper;
import com.tlias.pojo.Emp;
import org.apache.ibatis.annotations.Mapper;
import org.apache.ibatis.annotations.Select;
import java.util.List;
@Mapper
public interface EmpMapper {
//    @Select("SELECT count(*) from emp")
//    public Long count();
//
//    @Select("SELECT * from emp limit #{start},#{pageSize}")
//    public List<Emp> page(Integer start, Integer pageSize);
    // 使用pageHelper来分页, 这里正常写
    @Select("SELECT * from emp")
    public List<Emp> list();
}
```

- 3.在service的实现类, 修改获取列表数据的代码

```java

package com.tlias.service.impl;
import com.github.pagehelper.Page;
import com.github.pagehelper.PageHelper;
import com.tlias.mapper.EmpMapper;
import com.tlias.pojo.Emp;
import com.tlias.pojo.PageBean;
import com.tlias.service.EmpService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import java.util.List;
@Service
public class EmpServiceImpl implements EmpService {
    @Autowired
    private EmpMapper empMapper;
// 注释掉原来的service实现类的分页方法
//    @Override
//    public PageBean page(Integer page, Integer pageSize) {
//
//        // 1.获取总记录数
//        Long count = empMapper.count(); // 总数目
//
//        // 2.获取分页结果列表
//        Integer start = (page - 1) * pageSize;
//        List<Emp> empList = empMapper.page(start, pageSize);
//
//        // 3.封装pageBean
//        PageBean pageBean = new PageBean(count, empList);
//
//        return pageBean;
//    }
    // 使用pageHelper来分页
    @Override
    public PageBean page(Integer page, Integer pageSize) {
        // 1.设置分页参数
        PageHelper.startPage(page, pageSize);
        // 2.执行查询
        List<Emp> empList = empMapper.list();
        Page<Emp> p = (Page<Emp>) empList;
        // 3.封装pageBean
        PageBean pageBean = new PageBean(p.getTotal(), p.getResult());
        return pageBean;
    }
}
```

- 4.controller的代码不需要修改

![logo](../../_media/img/java/day10_e1.png ':size=500')

## Part.F 条件查询(Xml)

### 注意xml创建目录需要逐级创建

- 1.首先改写controller, 增加name,gender,begin,end这四个参数

```java
package com.tlias.controller;
import com.tlias.pojo.PageBean;
import com.tlias.pojo.Result;
import com.tlias.service.EmpService;
import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.format.annotation.DateTimeFormat;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;
import java.time.LocalDate;
@Slf4j
@RestController
public class EmpController {
    @Autowired
    private EmpService empService;
    @GetMapping("/emps")
    public Result page(
            @RequestParam(defaultValue = "1") Integer page,
            @RequestParam(defaultValue = "10") Integer pageSize,
            String name,
            Short gender,
            @DateTimeFormat(pattern = "yyyy-MM-dd") LocalDate begin,
            @DateTimeFormat(pattern = "yyyy-MM-dd") LocalDate end
    )
    {
        log.info("分页查询, 参数: {}, {},{},{},{},{}", page, pageSize,name,gender,begin,end);
        PageBean pageBean = empService.page(page, pageSize,name,gender,begin,end);
        return Result.success(pageBean);
    }
}
```

- 2.修改被controller调用的Service, 和Service的实现类

```java
package com.tlias.service;
import com.tlias.pojo.PageBean;
import java.time.LocalDate;
public interface EmpService {
    PageBean page(Integer page, Integer pageSize, String name, Short gender, LocalDate begin,LocalDate end);
}

// servie实现类

package com.tlias.service.impl;
import com.github.pagehelper.Page;
import com.github.pagehelper.PageHelper;
import com.tlias.mapper.EmpMapper;
import com.tlias.pojo.Emp;
import com.tlias.pojo.PageBean;
import com.tlias.service.EmpService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import java.time.LocalDate;
import java.util.List;
@Service
public class EmpServiceImpl implements EmpService {
    @Autowired
    private EmpMapper empMapper;
    // 使用pageHelper来分页
    @Override
    public PageBean page(Integer page, Integer pageSize, String name, Short gender, LocalDate begin, LocalDate end) {
        // 1.设置分页参数
        PageHelper.startPage(page, pageSize);
        // 2.执行查询
        List<Emp> empList = empMapper.list(name, gender, begin, end);
        Page<Emp> p = (Page<Emp>) empList;
        // 3.封装pageBean
        PageBean pageBean = new PageBean(p.getTotal(), p.getResult());
        return pageBean;
    }
}

```

- 3.修改mapper, 添加参数. 然后发现SQL需要改写成动态SQL, 所以准备改成xml格式

```java
package com.tlias.mapper;
import com.tlias.pojo.Emp;
import org.apache.ibatis.annotations.Mapper;
import org.apache.ibatis.annotations.Select;
import java.time.LocalDate;
import java.util.List;
@Mapper
public interface EmpMapper {
    // 使用pageHelper来分页, 这里正常写
    // @Select("SELECT * from emp ")
    public List<Emp> list(String name, Short gender, LocalDate begin, LocalDate end);
}

```

- 4.准备添加xml, 在resouce下添加文件夹(com.tlias.mapper) 和mapper同名同包 (重点: resources目录下创建文件夹需要一级级创建)

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.tlias.mapper.EmpMapper">
    <select id="list" resultType="com.tlias.pojo.Emp">
        select * from emp
        <where>
            <if test="name != null and name != ''">
                name like concat('%', #{name}, '%')
            </if>
            <if test="gender != null">
                and gender = #{gender}
            </if>
            <if test="begin != null and end != null">
                and entrydate between #{begin} and #{end}
            </if>
        </where>
        order by update_time desc
    </select>
</mapper>
```

## Part.G 删除员工(流程示例)


- 这是个批量删除的接口, 不过也很好体现了流程,Day10完结:
- web请求->控制器->Service(实现类)->Mapper->xml(Mybatis)

- 1.控制器

```java
package com.tlias.controller;

import com.tlias.pojo.PageBean;
import com.tlias.pojo.Result;
import com.tlias.service.EmpService;
import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.format.annotation.DateTimeFormat;
import org.springframework.web.bind.annotation.*;

import java.time.LocalDate;
import java.util.List;

@Slf4j
@RestController
public class EmpController {

    @Autowired
    private EmpService empService;

    @GetMapping("/emps")
    public Result page(
            @RequestParam(defaultValue = "1") Integer page,
            @RequestParam(defaultValue = "10") Integer pageSize,
            String name,
            Short gender,
            @DateTimeFormat(pattern = "yyyy-MM-dd") LocalDate begin,
            @DateTimeFormat(pattern = "yyyy-MM-dd") LocalDate end
    )
    {
        log.info("分页查询, 参数: {}, {},{},{},{},{}", page, pageSize,name,gender,begin,end);
        PageBean pageBean = empService.page(page, pageSize,name,gender,begin,end);
        return Result.success(pageBean);
    }

    // 删除员工
    @DeleteMapping("/emps/{ids}")
    public Result delete(@PathVariable List<Integer> ids)
    {
        log.info("批量删除操作, {}", ids);
        empService.delete(ids);
        return Result.success();
    }

}
```

- 2.Service层-业务逻辑(Service和它的实现类)

```java
package com.tlias.service;

import com.tlias.pojo.PageBean;

import java.time.LocalDate;
import java.util.List;

public interface EmpService {
    PageBean page(Integer page, Integer pageSize, String name, Short gender, LocalDate begin,LocalDate end);

    void delete(List<Integer> ids);
}

// Service实现类

package com.tlias.service.impl;

import com.github.pagehelper.Page;
import com.github.pagehelper.PageHelper;
import com.tlias.mapper.EmpMapper;
import com.tlias.pojo.Emp;
import com.tlias.pojo.PageBean;
import com.tlias.service.EmpService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.time.LocalDate;
import java.util.List;

@Service
public class EmpServiceImpl implements EmpService {

    @Autowired
    private EmpMapper empMapper;

    // 使用pageHelper来分页
    @Override
    public PageBean page(Integer page, Integer pageSize, String name, Short gender, LocalDate begin, LocalDate end) {

        // 1.设置分页参数
        PageHelper.startPage(page, pageSize);

        // 2.执行查询
        List<Emp> empList = empMapper.list(name, gender, begin, end);
        Page<Emp> p = (Page<Emp>) empList;

        // 3.封装pageBean
        PageBean pageBean = new PageBean(p.getTotal(), p.getResult());

        return pageBean;
    }

    // 删除员工
    @Override
    public void delete(List<Integer> ids) {
        empMapper.delete(ids);
    }

}
```

- 3.1 Mapper(Dao层)

```java
package com.tlias.mapper;

import com.tlias.pojo.Emp;
import org.apache.ibatis.annotations.Mapper;
import org.apache.ibatis.annotations.Select;

import java.time.LocalDate;
import java.util.List;

@Mapper
public interface EmpMapper {

    // 使用pageHelper来分页, 这里正常写
//    @Select("SELECT * from emp ") // 注释掉, 就会使用xml
    public List<Emp> list(String name, Short gender, LocalDate begin, LocalDate end);

    // 删除员工
    void delete(List<Integer> ids);

}
```

- 3.2 xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.tlias.mapper.EmpMapper">
    <delete id="delete">
        delete from emp
        where id in
        <foreach collection="ids" item="id" separator="," open="(" close=")">
            #{id}
        </foreach>
    </delete>
    <select id="list" resultType="com.tlias.pojo.Emp">
        select * from emp
        <where>
            <if test="name != null and name != ''">
                name like concat('%', #{name}, '%')
            </if>
            <if test="gender != null">
                and gender = #{gender}
            </if>
            <if test="begin != null and end != null">
                and entrydate between #{begin} and #{end}
            </if>
        </where>

        order by update_time desc
    </select>
</mapper>
```

![logo](../../_media/img/java/day10_g1.png ':size=500')
![logo](../../_media/img/java/day10_g2.png ':size=500')