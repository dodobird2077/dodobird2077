# Day8 - MyBatis


## PartA

- 1.首先, 创建SpringBoot项目时, 要同时勾选这两项

![logo](../../_media/img/java/day8_a1.png ':size=300')

- 2.项目结构

![logo](../../_media/img/java/day8_a2.png ':size=300')

- 3.配置文件 - application.properties

```
spring.datasource.url=jdbc:mysql://localhost:3306/ssm
spring.datasource.username=root
spring.datasource.password=root
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
```


- 4.代码

```java
package com.mybatos04.mapper;

import com.mybatos04.pojo.User;
import org.apache.ibatis.annotations.Mapper;
import org.apache.ibatis.annotations.Select;

import java.util.List;

@Mapper //运行时, 会自动生成时生成该接口实现类对象, 并且将对象交给IOC容器管理
public interface UserMapper {

    // 查询用户信息
    @Select("select * from user")
    public List<User> list();
}
```

- 实体类

```java
package com.mybatos04.pojo;

public class User {

    /**
     * 使用 Alt + ins创建 Getter&Setter, toString, Constructor(有参构造和无参构造. 无参构造不勾字段)
     */

    private Integer id;
    private String name;
    private Short age;
    private Short gender;
    private String phone;

    // 无参构造
    public User() {
    }

    // 有参构造
    public User(Integer id, String name, Short age, Short gender, String phone) {
        this.id = id;
        this.name = name;
        this.age = age;
        this.gender = gender;
        this.phone = phone;
    }

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Short getAge() {
        return age;
    }

    public void setAge(Short age) {
        this.age = age;
    }

    public Short getGender() {
        return gender;
    }

    public void setGender(Short gender) {
        this.gender = gender;
    }

    public String getPhone() {
        return phone;
    }

    public void setPhone(String phone) {
        this.phone = phone;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", name='" + name + ''' +
                ", age=" + age +
                ", gender=" + gender +
                ", phone='" + phone + ''' +
                '}';
    }
}
```

### 启动测试类

```java
package com.mybatos04;

import com.mybatos04.mapper.UserMapper;
import com.mybatos04.pojo.User;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;

import java.util.List;

@SpringBootTest // Springboot 整合单元测试的注解
class Mybatos04ApplicationTests {

    @Autowired
    private UserMapper userMapper;

    @Test
    void contextLoads() {
    }

    @Test
    public void testListUser() {
        List<User> list = userMapper.list();
        list.stream().forEach(user -> {
            System.out.println(user);
        });
    }

}
```


![logo](../../_media/img/java/day8_a3.png ':size=300')

## PartB Mybatis 配置SQL提示


- P118

- 1.添加提示, 此时有SQL提示, 但是user报红, 表示没有读取到数据表

![logo](../../_media/img/java/day8_b1.png ':size=500')


- 2.配置数据库链接信息(每个项目都是独立的, 之前配置项目对此不生效)

![logo](../../_media/img/java/day8_b2.png ':size=500')

配置完后, 正常了


![logo](../../_media/img/java/day8_b3.png ':size=300')