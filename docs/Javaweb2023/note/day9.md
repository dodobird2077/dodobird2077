# MyBatis 增删改查, xml配置SQL

## PartA 增删改查


### 准备工作


- 1.数据表
```sql
CREATE TABLE `emp` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `username` varchar(20) DEFAULT NULL,
  `password` varchar(32) DEFAULT '123456',
  `name` varchar(10) DEFAULT NULL,
  `gender` tinyint(1) unsigned DEFAULT NULL,
  `image` varchar(255) DEFAULT NULL,
  `job` tinyint(3) unsigned DEFAULT NULL,
  `entrydate` datetime DEFAULT NULL,
  `dept_id` int(11) DEFAULT '0',
  `create_time` datetime DEFAULT NULL,
  `update_time` datetime DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=7 DEFAULT CHARSET=utf8mb4;
```

- 2.准备工作

![logo](../../_media/img/java/day9_a1.png ':size=500')

- 3.新建SpringBoot项目所需要的依赖

![logo](../../_media/img/java/day9_a2.png ':size=300')

- 4.项目结构

![logo](../../_media/img/java/day9_a3.png ':size=600')

### 二.代码
1. 实体类Emp.java

```java
package com.emp01.pojo;

import lombok.Data;
import java.time.LocalDate;       // 导入正确的包
import java.time.LocalDateTime;   // 导入正确的包

@Data
public class Emp {
    private Integer id;
    private String username;
    private String password;
    private String name;
    private Short gender;
    private String image;
    private Short job;
    private LocalDate entrydate;
    private Integer deptId;
    private LocalDateTime createTime;
    private LocalDateTime updateTime;
}
```

2. 映射接口 EmpMapper

```java
package com.emp01.mapper;

import com.emp01.pojo.Emp;
import org.apache.ibatis.annotations.*;

import java.time.LocalDate;
import java.util.List;

@Mapper
public interface EmpMapper {

    // 查询用户信息
    @Select("select * from emp")
    public List<Emp> list();

    // 根据ID删除数据
    @Delete("delete from emp where id=#{id}")
    public void delete(Integer id);

    // 增加信息
    @Options(keyProperty = "id", useGeneratedKeys = true) // 增加这个注解, 会自动生成主键值,赋值給emp对象的ID属性
    @Insert("INSERT INTO `emp` (`username`, `password`, `name`, `gender`, `image`, `job`, `entrydate`, `dept_id`, `create_time`, `update_time`) " +
            "VALUES (#{username}, #{password}, #{name}, #{gender}, #{image}, #{job}, #{entryDate}, #{deptId}, #{createTime}, #{updateTime})")
    public void insert(Emp emp);

    // 更新 (注意大小写. 正确:dept_id= #{deptId}, 错误:dept_id= #{dept_id},)
    @Update("UPDATE `emp` SET `username`= #{username}, `password`= #{password}, `name`= #{name}, `gender`= #{gender}, `image`= #{image}, `job`= #{job}, `entrydate`= #{entrydate}, `dept_id`= #{deptId} WHERE `id`= #{id}")
    public void update(Emp emp);

    // 查询.返回对象
    @Select("select * from emp where id = #{id}")
    public Emp getById(Integer id);


    // 根据条件查询
    @Select("select * from emp  where name like concat('%', #{name}, '%') and gender = #{gender} and " +
            "entrydate between #{begin} and #{end} order by update_time desc;")
    public List<Emp> check(String name, Short gender, LocalDate begin, LocalDate end);



}
```

3. 数据库配置文件

```java
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/ssm
spring.datasource.username=root
spring.datasource.password=root

mybatis.configuration.log-impl=org.apache.ibatis.logging.stdout.StdOutImpl
```

4. 单元测试类

```java
package com.emp01;

import com.emp01.mapper.EmpMapper;
import com.emp01.pojo.Emp;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;

import java.time.LocalDate;
import java.time.LocalDateTime;
import java.util.List;

@SpringBootTest
class Emp01ApplicationTests {

    @Autowired  // 依赖注入
    private EmpMapper empMapper;

    // 测试删除
    @Test
    public void testDelete() {
        empMapper.delete(3);
    }

    /**
     * 如果报错, 一般是因为实体类中的属性漏写, 或者没有使用驼峰命名
     * */
    @Test
    public void testInsert() {
        Emp emp = new Emp();
        emp.setUsername("黄忠");
        emp.setName("黄忠");
        emp.setImage("1.jpg");
        emp.setGender((short)1);
        emp.setJob((short)1);
        emp.setEntrydate(LocalDate.of(2000, 1, 1));
        emp.setCreateTime(LocalDateTime.now());
        emp.setUpdateTime(LocalDateTime.now());
        emp.setDeptId(1);


        // 执行新增操作
        empMapper.insert(emp);

        // 获取新增的ID
        Integer empId = emp.getId();
        System.out.println("新增的员工ID:" + empId);
    }

    // 更新员工
    @Test
    public void testUpdate()
    {
        // 构造员工对象
        Emp emp = new Emp();
        emp.setId(6);
        emp.setUsername("Tom1");
        emp.setName("Tom1");
        emp.setImage("1.jpg");
        emp.setGender((short)1);
        emp.setJob((short)1);
        emp.setEntrydate(LocalDate.of(2020, 1, 1));
        emp.setCreateTime(LocalDateTime.now());
        emp.setUpdateTime(LocalDateTime.now());
        emp.setDeptId(1);

        empMapper.update(emp);
    }

    @Test
    public void testGetById()
    {
        Emp emp = empMapper.getById(5);
        System.out.println(emp);
    }

    @Test
    public void testCheck()
    {
        List<Emp> emp = empMapper.check("张", (short)1, LocalDate.of(2000, 1, 1), LocalDate.of(2030, 1, 1));
        System.out.println(emp);
    }
}
```

5. pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.7.14</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <groupId>com.emp01</groupId>
    <artifactId>emp01</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>emp01</name>
    <description>emp01</description>
    <properties>
        <java.version>11</java.version>
    </properties>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>2.3.1</version>
        </dependency>

        <dependency>
            <groupId>com.mysql</groupId>
            <artifactId>mysql-connector-j</artifactId>
            <scope>runtime</scope>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter-test</artifactId>
            <version>2.3.1</version>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <configuration>
                    <excludes>
                        <exclude>
                            <groupId>org.projectlombok</groupId>
                            <artifactId>lombok</artifactId>
                        </exclude>
                    </excludes>
                </configuration>
            </plugin>
        </plugins>
    </build>

</project>
```



## PartB - MyBatis通过xml配置SQL-基础完结

 - 一般情况下, 简单的SQL用注解, 复杂的SQL用xml映射. 灵活使用切换
 - 插件. MyBatisX, 用于跳转等功能,在Mappper接口的方法中Alt+回车可生成xml

1. 注解方式

![logo](../../_media/img/java/day9_b1.png ':size=300')

2. 使用xml的方式

![logo](../../_media/img/java/day9_b2.png ':size=500')

### 代码

- 1.单元测试类

```java
@Test
public void testCheck()
{
    List<Emp> emp = empMapper.checkXml("张", (short)1, LocalDate.of(2000, 1, 1), LocalDate.of(2030, 1, 1));
    System.out.println(emp);
}
```

- 2.映射接口

```java
public List<Emp> checkXml(String name, Short gender, LocalDate begin, LocalDate end);
```

- 3.xml文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.emp01.mapper.EmpMapper">
    <select id="checkXml" resultType="com.emp01.pojo.Emp">
        select * from emp  where name like concat('%', #{name}, '%') and gender = #{gender} and entrydate between #{begin} and #{end} order by update_time desc
    </select>
</mapper>
```


![logo](../../_media/img/java/day9_b3.png ':size=300')

### 动态SQL标签: 
```
<if></if>
<where></where>
<foreach></foreach>
<sql></sql>
<include></include>
```

##### 1.改造查询语句: 当name,gender等搜索条件无值时, 传入 name = null 会导致查询不到信息

- 1.1使用标签前

```xml
<select id="checkXml" resultType="com.emp01.pojo.Emp">
      select *
      from emp
      where name like concat('%', #{name}, '%')
        and gender = #{gender}
        and entrydate between #{begin} and #{end}
      order by update_time desc
</select>
```
    
- 1.2使用标签后

```xml
<mapper namespace="com.emp01.mapper.EmpMapper">
    <select id="checkXml" resultType="com.emp01.pojo.Emp">
        select *
        from emp
        <where>
          <if test="name != null">
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

##### 2.改造更新语句: 当更新传入的image,gender,job,dept_id没传值时,会导致数据被更新为null

- 2.1 改造前

```java
// 更新员工
@Test
public void testUpdate()
{
    // 构造员工对象
    Emp emp = new Emp();
    emp.setId(6);
    emp.setUsername("Tom1");
    emp.setName("Tom1");
    // emp.setImage("1.jpg");
    // emp.setGender((short)1);
    // emp.setJob((short)1);
    // emp.setDeptId(1);
    empMapper.update(emp);
}

// Mapper中
// 更新 (注意大小写. 正确:dept_id= #{deptId}, 错误:dept_id= #{dept_id},)
@Update("UPDATE `emp` SET `username`= #{username}, `password`= #{password}, `name`= #{name}, `gender`= #{gender}, `image`= #{image}, `job`= #{job}, `entrydate`= #{entrydate}, `dept_id`= #{deptId} WHERE `id`= #{id}")
public void update(Emp emp);
```

- 2.2 改造后 (注意字段名小写,变量名驼峰)

```java
<update id="updateNew" >
    update emp
    <set>
        <if test="username != null"> username = #{username},</if>
        <if test="name != null"> name = #{name},</if>
        <if test="gender != null"> gender = #{gender},</if>
        <if test="image != null"> image = #{image},</if>
        <if test="job != null"> job = #{job},</if>
        <if test="entrydate != null"> entrydate = #{entrydate},</if>
        <if test="deptId != null"> dept_id = #{deptId},</if>
        <if test="updateTime != null"> update_time = #{update_time},</if>
    </set>
    where id = #{id}
</update>
```

- 3.1 使用foreach标签的删除方法

```java
// Mapper接口中: 批量删除员工
public void deleteByIds(List<Integer> ids);

//单元测试类:
@Test
public void testDel()
{
    List<Integer> ids = Arrays.asList(7,8,9);
    empMapper.deleteByIds(ids);
}
```

```xml
<!--批量删除员工信息-->
<!--
    collection:遍历的集合
    item: 遍历出来的元素
    separator: 分隔符
    open: 遍历开始前拼接的SQL片段
    close: 同上
   -->
<delete id="deleteByIds">
    delete from emp where id in
    <foreach collection="ids" item="id"  separator="," open="(" close=")">
        #{id}
    </foreach>
</delete>
```

- 4.sql片段的封装和引用

![logo](../../_media/img/java/day9_b4.png ':size=300')