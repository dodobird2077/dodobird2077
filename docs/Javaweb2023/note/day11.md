
# Day11. 添加,编辑员工, 文件上传,阿里云OSS

## Part.A 添加员工
- 1.控制器

```java
package com.tlias.controller;

import com.tlias.pojo.Emp;
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

    @PostMapping("/emps")
    public Result save(@RequestBody Emp emp) {

        log.info("新增员工, {}", emp);
        empService.save(emp);
        return Result.success();
    }
}
```

- 2.Service中

```java
package com.tlias.service;

import com.tlias.pojo.Emp;
import com.tlias.pojo.PageBean;

import java.time.LocalDate;
import java.util.List;

public interface EmpService {
    PageBean page(Integer page, Integer pageSize, String name, Short gender, LocalDate begin,LocalDate end);

    void delete(List<Integer> ids);

    void save(Emp emp);
}
```

- 2.2 Service 实现类

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

import java.time.LocalDate;
import java.time.LocalDateTime;
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

    // 新增员工
    @Override
    public void save(Emp emp) {
        emp.setCreateTime(LocalDateTime.now());
        emp.setUpdateTime(LocalDateTime.now());
        empMapper.insert(emp);
    }

}
```

- 3.Mapper

```java
package com.tlias.mapper;

import com.tlias.pojo.Emp;
import org.apache.ibatis.annotations.Insert;
import org.apache.ibatis.annotations.Mapper;
import org.apache.ibatis.annotations.Select;

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
}
```


## Part.B 文件上传(本地存储)及调试模式

### 调试模式
启动时, 点击那个绿色的虫子


### 文件上传前端测试代码
- 1.在resource目录下, 添加upload.html和测试txt文件(中国梦.txt)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>上传文件</title>
</head>
<body>

    <form action="/upload" method="post" enctype="multipart/form-data">
        姓名: <input type="text" name="username"><br>
        年龄: <input type="text" name="age"><br>
        头像: <input type="file" name="image"><br>
        <input type="submit" value="提交">
    </form>

</body>
</html>
```

### 服务端代码

- 1.添加控制器

```java
package com.tlias.controller;

import com.tlias.pojo.Result;
import lombok.extern.slf4j.Slf4j;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.multipart.MultipartFile;

import java.io.File;
import java.io.IOException;
import java.util.UUID;

@Slf4j
@RestController
public class UploadController {

    // 文件上传
    @PostMapping("/upload")
    public Result upload(String username, Integer age, MultipartFile image) throws IOException {
        log.info("文件上传,{},{},{}", username, age, image);

        // 原始文件名
        String originalFile = image.getOriginalFilename();

        // 构造唯一的文件名
        Integer index = originalFile.lastIndexOf(".");
        String extend = originalFile.substring(index);
        String newFilename = UUID.randomUUID().toString() + extend;

        image.transferTo(new File("D:\JAVA\upload\"+newFilename));
        return Result.success();
    }
}
```

- 2.文件大小限制

```yml
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/tlias
spring.datasource.username=root
spring.datasource.password=root

mybatis.configuration.log-impl=org.apache.ibatis.logging.stdout.StdOutImpl

mybatis.configuration.map-underscore-to-camel-case=true

spring.servlet.multipart.max-file-size=10MB
spring.servlet.multipart.max-request-size=100MB
```

- 3.结果

![logo](../../_media/img/java/day11_b1.png ':size=500')
![logo](../../_media/img/java/day11_b2.png ':size=500')
![logo](../../_media/img/java/day11_b3.png ':size=500')


## Part.C 阿里云OSS


##### 一. 账号注册和OSS服务开通

![logo](../../_media/img/java/day11_c1.png ':size=500')

##### 二.入门测试程序
- 1.在Test目录创建Demo.java测试类
- 2.从阿里云复制Java的示例代码, 配置账号等信息
- 3.运行测试类, 发现文件已经上传


##### 三.集成到项目中

![logo](../../_media/img/java/day11_c2.png ':size=500')


## Part.D 修改员工

- 修改员工有两个接口, 一个是点击编辑显示详情
- 第二个是点击确定更新员工


- 1.控制器

```java
package com.tlias.controller;

import com.tlias.pojo.Emp;
import com.tlias.pojo.PageBean;
import com.tlias.pojo.Result;
import com.tlias.service.EmpService;
import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.format.annotation.DateTimeFormat;
import org.springframework.web.bind.annotation.*;
import org.springframework.web.multipart.MultipartFile;

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

    @PostMapping("/emps")
    public Result save(@RequestBody Emp emp) {

        log.info("新增员工, {}", emp);
        empService.save(emp);
        return Result.success();
    }

    // 修改员工1
    @GetMapping("/emps/{id}")
    public Result getById(@PathVariable Integer id) {
        log.info("根据ID返回员工,{}", id);
        Emp emp = empService.getById(id);
        return Result.success(emp);
    }

    // 修改员工2
    @PutMapping("/emps")
    public Result update(@RequestBody  Emp emp) {
        empService.update(emp);
        return Result.success();
    }

}
```

- 2.服务

```java
package com.tlias.service;

import com.tlias.pojo.Emp;
import com.tlias.pojo.PageBean;
import java.time.LocalDate;
import java.util.List;

public interface EmpService {
    PageBean page(Integer page, Integer pageSize, String name, Short gender, LocalDate begin,LocalDate end);

    void delete(List<Integer> ids);
    void save(Emp emp);
    Emp getById(Integer id);
    void update(Emp emp);
}
```

- 3.服务实现类

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

import java.time.LocalDate;
import java.time.LocalDateTime;
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

    // 新增员工
    @Override
    public void save(Emp emp) {
        emp.setCreateTime(LocalDateTime.now());
        emp.setUpdateTime(LocalDateTime.now());
        empMapper.insert(emp);
    }

    @Override
    public Emp getById(Integer id) {
       return empMapper.getById(id);
    }

    // 更新员工
    @Override
    public void update(Emp emp) {
        emp.setUpdateTime(LocalDateTime.now());
        empMapper.update(emp);
    }
}
```

- 4.Mapper代码

```java
package com.tlias.mapper;

import com.tlias.pojo.Emp;
import org.apache.ibatis.annotations.Insert;
import org.apache.ibatis.annotations.Mapper;
import org.apache.ibatis.annotations.Select;

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
}
```

- 4.1 update对应的xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.tlias.mapper.EmpMapper">
    <update id="update">
        update emp
        <set>
            <if test="username != null and username != ''">
                username = #{username},
            </if>
            <if test="password != null and password != ''">
                password = #{password},
            </if>
            <if test="name != null and name != ''">
                name = #{name},
            </if>
            <if test="gender != null">
                gender = #{gender},
            </if>
            <if test="image != null and image != ''">
                image = #{image},
            </if>
            <if test="job != null">
                job = #{job},
            </if>
            <if test="entrydate != null">
                entrydate = #{entrydate},
            </if>
            <if test="deptId != null">
                dept_id = #{deptId},
            </if>
            <if test="updateTime != null">
                update_time = #{updateTime}
            </if>
        </set>
        where id = #{id}
    </update>
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