# day5

## Part-A.请求和响应

### 一.请求: 

```java
package com.itheima.controller;
import com.itheima.pojo.User;
import org.springframework.format.annotation.DateTimeFormat;
import org.springframework.web.bind.annotation.*;
import javax.servlet.http.HttpServletRequest;
import java.lang.reflect.Array;
import java.time.LocalDateTime;
import java.util.Arrays;
import java.util.List;

@RestController
public class RequestController {
    // 1.通过servlet接收, 了解即可
    @RequestMapping("/simple01")
    public String simple(HttpServletRequest request)
    {
        // 获取请求参数
        String name = request.getParameter("name");
        return "simple: " + name;
    }
    // 2.简单参数
    @RequestMapping("/simple")
    public String simpleParam(String name, Integer age)
    {
        System.out.println( name +":"+ age);
        return "OK";
    }
    // 3.对象参数(需要先定义对象)
    @RequestMapping("/simplePojo")
    public String simplePojo(User user)
    {
        System.out.println(user);
        return "OK";
    }

    // 4.数组参数
    @RequestMapping("/arrayParam")
    public String arrayParam(String[] hobby)
    {
        System.out.println(Arrays.toString(hobby));
        return "OK";
    }
    // 5.集合参数(需要导入List包)
    @RequestMapping("/listParam")
    public String lstParam(@RequestParam List<String> hobby)
    {
        System.out.println(hobby);
        return "OK";
    }
    // 6.时间参数
    @RequestMapping("/dateParam")
    public String dateParam(@DateTimeFormat(pattern = "yyyy-MM-dd HH:mm:ss") LocalDateTime updateTime)
    {
        System.out.println(updateTime);
        return "ok";
    }
    // 7.json参数
    @RequestMapping("/jsonParam")
    public String jsonParam(@RequestBody User user)
    {
        System.out.println(user);
        return  "ok";
    }
    @RequestMapping("/pathParam/{id}")
    public String pathParam(@PathVariable Integer id)
    {
        System.out.println(id);
        return "OK";
    }
}
```

### 2.详情

- 1. 简单参数接收
`URL: localhost:8080/simple?name=张三&age=333`

- 2.简单实体参数接收(将参数封装到实体类中)
```
2.1 创建接收对象 com/itheima/pojo/User.java, 使用 alt+insert快速添加getter,setter和toString
```
- 3.T5-04 数组集合参数

```java
package com.itheima.pojo;
public class User {
    private String name;
    private Integer age;
    private Address address;
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public Integer getAge() {
        return age;
    }
    public Address getAddress() {
        return address;
    }
    public void setAddress(Address address) {
        this.address = address;
    }
    public void setAge(Integer age) {
        this.age = age;
    }
    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                ", age=" + age +
                ", address=" + address +
                '}';
    }
}

package com.itheima.pojo;
public class Address {
    private String province;
    private String city;
    public String getProvince() {
        return province;
    }
    public void setProvince(String province) {
        this.province = province;
    }
    public String getCity() {
        return city;
    }
    public void setCity(String city) {
        this.city = city;
    }
    @Override
    public String toString() {
        return "Address{" +
                "province='" + province + '\'' +
                ", city='" + city + '\'' +
                '}';
    }
}
```

![logo](../../_media/img/java/day5_a1.png ':size=300')



![logo](../../_media/img/java/day5_a2.png ':size=500')


T5.06 路径参数
localhost:8080/pathParam/10086

### 二.响应: 

创建这两个类, 一个是控制器, 一个是封装响应请求


![logo](../../_media/img/java/day5_a3.png ':size=200')


![logo](../../_media/img/java/day5_a4.png ':size=400')


### 代码

```java
package com.itheima.pojo;
/**
 * 统一响应结果封装类
 */
public class Result {

    private Integer code; // 1.成功. 0.失败
    private String msg; // 提示信息
    private Object data; //数据

    public Result()
    {
    }
    public Result(Integer code, String msg, Object data)
    {
        this.code = code;
        this.msg = msg;
        this.data = data;
    }
    public Integer getCode() {
        return code;
    }
    public void setCode(Integer code) {
        this.code = code;
    }
    public String getMsg() {
        return msg;
    }
    public void setMsg(String msg) {
        this.msg = msg;
    }
    public Object getData() {
        return data;
    }
    public void setData(Object data) {
        this.data = data;
    }

    // 静态方法, 方便调用
    public static Result success(Object data)
    {
        return new Result(1, "success", data);
    }
    public static Result success()
    {
        return new Result(1, "success", null);
    }
    public static Result error(String msg)
    {
        return new Result(0, msg, null);
    }
}
```

```java
package com.itheima.controller;
import com.itheima.pojo.Address;
import com.itheima.pojo.Result;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
import java.util.ArrayList;
import java.util.List;
@RestController
public class ResponseController {
    /*
    // 响应字符串
    @RequestMapping
    public String hello()
    {
        return "hello";
    }
    // 响应json
    @RequestMapping("/getAddr")
    public Address getAddr()
    {
        Address addr = new Address();
        addr.setProvince("河南");
        addr.setCity("驻马店");
        return addr;
    }
     // 响应集合
    @RequestMapping("/listAddr")
    public List<Address> listAddr()
    {
        List<Address> list = new ArrayList<>();
        Address addr = new Address();
        addr.setProvince("河南");
        addr.setCity("驻马店");
        list.add(addr);
        return list;
    }
    */
    // 响应字符串
    @RequestMapping
    public Result hello()
    {
        return Result.success("hello");
    }
    // 响应json
    @RequestMapping("/getAddr")
    public Result getAddr()
    {
        Address addr = new Address();
        addr.setProvince("河南");
        addr.setCity("郑州");
        return Result.success(addr);
    }
    // 响应集合
    @RequestMapping("/listAddr")
    public List<Address> listAddr()
    {
        List<Address> list = new ArrayList<>();
        Address addr = new Address();
        addr.setProvince("河南");
        addr.setCity("驻马店");
        list.add(addr);
        return list;
    }
}
```

## Part-B.请求和响应案例-员工列表


**注意事项:**
- 1.如果导入dom4j显示红色, 在右侧maven刷新
- 2.课件文件在黑马程序员公众号的资料中,转百度云下载
- 3.主要就是导入文件复杂一点


![logo](../../_media/img/java/day5_b1.png ':size=500')


- 完成上面文件导入后:  1.创建`EmpController`
```java
package com.itheima.controller;
import com.itheima.pojo.Emp;
import com.itheima.pojo.Result;
import com.itheima.utils.XmlParserUtils;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
import java.util.List;

@RestController
public class EmpController {
    @RequestMapping("listEmp")
    public Result list()
    {
        // 1.加载并解析xml
        String file = this.getClass().getClassLoader().getResource("emp.xml").getFile();
        System.out.println(file);
        List<Emp> empList = XmlParserUtils.parse(file, Emp.class);
        // 2.数据处理
        empList.stream().forEach(emp -> {
            String gender = emp.getGender();
            if ("1".equals(gender)) {
                emp.setGender("男");
            } else if ("2".equals(gender)) {
                emp.setGender("女");
            }
            String job = emp.getJob();
            if ("1".equals(gender)) {
                emp.setGender("讲师");
            } else if ("2".equals(gender)) {
                emp.setGender("班主任");
            } else if ("3".equals(gender)) {
                emp.setGender("就业指导");
            }
        });
        // 3.响应数据
        return Result.success(empList);
    }
}
```

- 2.postman中测试: localhost:8080/listEmp 
   
   结果: 返回了Json

- 3.浏览器中访问 


![logo](../../_media/img/java/day5_b2.png ':size=500')

## Part-C.三层架构改造员工列表


### 目标: 将原本的控制器, 拆分成三层:
- 1.控制器(接受请求和响应)
- 2.service层(业务逻辑)
- 3.dao层(数据获取)


![logo](../../_media/img/java/day5_c1.png ':size=500')


![logo](../../_media/img/java/day5_c2.png ':size=500')


### 步骤

- 1.创建`src/main/java/com/itheima/service/EmpService.java` 接口类和相应目录

```java
package com.itheima.service;
import com.itheima.pojo.Emp;
import java.util.List;
public interface EmpService {
    public List<Emp> listEmp();
}
```

- 2.创建 impl/EmpServiceA.java 实现上面的接口, 快捷键 ctrl+i

```java
package com.itheima.service.impl;
import com.itheima.dao.EmpDao;
import com.itheima.dao.impl.EmpDaoA;
import com.itheima.pojo.Emp;
import com.itheima.service.EmpService;
import java.util.List;
public class EmpServiceA implements EmpService {
    private EmpDao empDao = new EmpDaoA();
    @Override
    public List<Emp> listEmp() {
        // 1.调用dao获取数据
        List<Emp> empList = empDao.listEmp();
        // 2.数据处理
        empList.stream().forEach(emp -> {
            String gender = emp.getGender();
            if ("1".equals(gender)) {
                emp.setGender("男");
            } else if ("2".equals(gender)) {
                emp.setGender("女");
            }
            String job = emp.getJob();
            if ("1".equals(gender)) {
                emp.setGender("讲师");
            } else if ("2".equals(gender)) {
                emp.setGender("班主任");
            } else if ("3".equals(gender)) {
                emp.setGender("就业指导");
            }
        });
        return empList;
    }
}
```

- 3.创建src/main/java/com/itheima/dao/EmpDao.java 接口类和相应目录

``` java
package com.itheima.dao;
import com.itheima.pojo.Emp;
import java.util.List;
/**
 * 用于获取员工数据的接口类
 */
public interface EmpDao {
    public List<Emp> listEmp();
}
```

- 4.创建impl/EmpDaoA.java

```java
package com.itheima.dao.impl;
import com.itheima.dao.EmpDao;
import com.itheima.pojo.Emp;
import com.itheima.utils.XmlParserUtils;
import java.util.List;
public class EmpDaoA implements EmpDao {
    @Override
    public List<Emp> listEmp() {
        // 1.加载并解析xml
        String file = this.getClass().getClassLoader().getResource("emp.xml").getFile();
        System.out.println(file);
        List<Emp> empList = XmlParserUtils.parse(file, Emp.class);
        return empList;
    }
}
```

- 5.最后修改控制器

```java
package com.itheima.controller;
import com.itheima.pojo.Emp;
import com.itheima.pojo.Result;
import com.itheima.service.EmpService;
import com.itheima.service.impl.EmpServiceA;
import com.itheima.utils.XmlParserUtils;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
import java.util.List;
@RestController
public class EmpController {
    private EmpService empService = new EmpServiceA();
    @RequestMapping("listEmp")
    public Result list()
    {
        // 1.调用service, 获取数据
        List<Emp> empList = empService.listEmp();
        // 3.响应数据
        return Result.success(empList);
    }
}
```

## Part-D.控制反转和DI依赖注入


**控制反转-IOC:**

- Inversion Of Control
对象的创建控制权, 由程序自身创建(new ClassName), 转移到外部(容器)


**依赖注入-DI**

- Dependency Injection, 容器为应用程序提供运行时, 所依赖的资源

**Bean对象:**

- IOC容器中创建, 管理的对象


### 实现步骤

- 1.先给Service的实现类ServiceA和DAO的实现类DaoA加上注解: `@Compent`
(表示将类交给IOC容器管理, 成为容器中的Bean)

```java
package com.itheima.service.impl;
import com.itheima.dao.EmpDao;
import com.itheima.dao.impl.EmpDaoA;
import com.itheima.pojo.Emp;
import com.itheima.service.EmpService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;
import java.util.List;
@Component //表示将类交给IOC容器管理, 成为容器中的Bean
public class EmpServiceA implements EmpService {
    // 非IOC方式
    // private EmpDao empDao = new EmpDaoA();
    // IOC方式, 不需要new对象
    @Autowired // // 运行时, IOC容器会提供改类型的bean对象, 并赋值给该变量 - 依赖注入
    private EmpDao empDao;
    @Override
    public List<Emp> listEmp() {
        // 1.调用dao获取数据
        List<Emp> empList = empDao.listEmp();
        // 2.数据处理 -省略代码
        return empList;
    }
}
```

```java
package com.itheima.dao.impl;
import com.itheima.dao.EmpDao;
import com.itheima.pojo.Emp;
import com.itheima.utils.XmlParserUtils;
import org.springframework.stereotype.Component;
import java.util.List;
@Component  // 表示将类交给IOC容器管理, 成为容器中的Bean
public class EmpDaoA implements EmpDao {
    @Override
    public List<Emp> listEmp() {
        // 1.加载并解析xml
        String file = this.getClass().getClassLoader().getResource("emp.xml").getFile();
        System.out.println(file);
        List<Emp> empList = XmlParserUtils.parse(file, Emp.class);
        return empList;
    }
}
```

- 2.然后给调用上面类的地方, 使用DI的方式给变量赋值, 而不是使用New 的方式

```java
package com.itheima.controller;
import com.itheima.pojo.Emp;
import com.itheima.pojo.Result;
import com.itheima.service.EmpService;
import com.itheima.service.impl.EmpServiceA;
import com.itheima.utils.XmlParserUtils;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
import java.util.List;
@RestController
public class EmpController {
    // 非IOC方式, 需要new 对象
    //private EmpService empService = new EmpServiceA();
    @Autowired // 运行时, IOC容器会提供改类型的bean对象, 并赋值给该变量 - 依赖注入
    private EmpService empService;
    @RequestMapping("listEmp")
    public Result list()
    {
        // 1.调用service, 获取数据
        List<Emp> empList = empService.listEmp();
        // 3.响应数据
        return Result.success(empList);
    }
}
```

- 3.运行

### 图解

![logo](../../_media/img/java/day5_c2.png ':size=300')