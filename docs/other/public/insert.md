# 将JT项目的坐标数据入库

## 一. 直接入库, 只有坐标数据



### 1.查看数据结构
![logo](../../_media/img/tcp/geo01.png  ':size=700')


### 2. 创建数据表

```sql
CREATE TABLE `geo_data` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `long` decimal(9,6) DEFAULT NULL,
  `lat` decimal(9,6) DEFAULT NULL,
  `machine_no` varchar(15) DEFAULT NULL,
  `device_time` timestamp NULL DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

```

### 3.创建pojo模型文件 `src/main/java/org/yzh/pojo/GetData`

```java
package org.yzh.pojo;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

// 鼠标悬浮, add lombook to xxxxpath
@Data
@AllArgsConstructor
@NoArgsConstructor
public class GeoData {

    private String long;
    private String lat;
    private String machine_no;
    private String deviceTime;
}
```

### 43.创建mapper映射文件 `src/main/java/org/yzh/mapper/GetDataMapper`

```java
package org.yzh.mapper;

import org.apache.ibatis.annotations.Insert;
import org.apache.ibatis.annotations.Mapper;
import org.yzh.pojo.GetData;

@Mapper
public interface GetDataMapper {

    // 插入数据
    @Insert("INSERT into geo_data (longitude, latitude, machine_no, device_time) values (#{longitude}, #{latitude}, #{machine_no}, #{deviceTime})")
    public void insert(GetData getData);
}

```

---
---

## 二. 使用控制台日志入库

- 目录结构

![logo](../../_media/img/tcp/geo02.png  ':size=700')


### 1. 需要在入口文件添加扫描

```
@MapperScan("org.yzh.mapper") // 确保这里的包路径是正确的
```

- 入库代码:

```java
package org.yzh.protocol.codec;

import io.github.yezhihao.netmc.codec.MessageDecoder;
import io.github.yezhihao.netmc.codec.MessageEncoder;
import io.github.yezhihao.netmc.session.Session;
import io.github.yezhihao.protostar.SchemaManager;
import io.github.yezhihao.protostar.util.Explain;
import io.netty.buffer.ByteBuf;
import io.netty.buffer.ByteBufUtil;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.yzh.mapper.HexMapper;
import org.yzh.pojo.Hex;
import org.yzh.protocol.basics.JTMessage;

/**
 * JT消息编解码适配器
 * @author yezhihao
 * https://gitee.com/yezhihao/jt808-server
 */
public class JTMessageAdapter implements MessageEncoder<JTMessage>, MessageDecoder<JTMessage> {

    protected static final Logger log = LoggerFactory.getLogger(JTMessageAdapter.class.getSimpleName());

    private final JTMessageEncoder messageEncoder;

    private final JTMessageDecoder messageDecoder;

    @Autowired
    private HexMapper hexMapper;

    public JTMessageAdapter(String... basePackages) {
        this(new SchemaManager(basePackages));
    }

    public JTMessageAdapter(SchemaManager schemaManager) {
        this(new JTMessageEncoder(schemaManager), new JTMessageDecoder(schemaManager));
    }

    public JTMessageAdapter(JTMessageEncoder messageEncoder, JTMessageDecoder messageDecoder) {
        this.messageEncoder = messageEncoder;
        this.messageDecoder = messageDecoder;
    }

    public ByteBuf encode(JTMessage message, Explain explain) {
        return messageEncoder.encode(message, explain);
    }

    public JTMessage decode(ByteBuf input, Explain explain) {
        return messageDecoder.decode(input, explain);
    }

    public ByteBuf encode(JTMessage message) {
        return messageEncoder.encode(message);
    }

    public JTMessage decode(ByteBuf input) {
        return messageDecoder.decode(input);
    }

    @Override
    public ByteBuf encode(JTMessage message, Session session) {
        ByteBuf output = messageEncoder.encode(message);
        encodeLog(session, message, output);
        return output;
    }

    @Override
    public JTMessage decode(ByteBuf input, Session session) {
        JTMessage message = messageDecoder.decode(input);
        if (message != null)
            message.setSession(session);
        decodeLog(session, message, input);
        return message;
    }

    /**
     *  应该是响应信息的打印 response
     */
    public void encodeLog(Session session, JTMessage message, ByteBuf output) {
        if (log.isInfoEnabled()) {
            log.info("{}\n>>>>>-{},hex[{}]  ", session, message, ByteBufUtil.hexDump(output));
        }

    }

    /**
     *  应该是接收信息的打印 request
     */
    public void decodeLog(Session session, JTMessage message, ByteBuf input) {
        if (log.isInfoEnabled()) {
            String hexString = ByteBufUtil.hexDump(input, 0, input.writerIndex());
            log.info("{}\n<<<<<-{},hex[{}]  ", session, message, hexString);
            Hex hex = new Hex();
            hex.setSession(session.toString());
            hex.setHex(hexString);
            hex.setMessage(message.toString());
            hexMapper.insert(hex);
        }

    }
}
```

###  2.添加数据库和数据库实体类

```sql
CREATE TABLE `hex` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `session` varchar(255) DEFAULT NULL,
  `message` varchar(500) DEFAULT NULL,
  `hex` varchar(500) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=63 DEFAULT CHARSET=utf8mb4;

```
```ini
spring.datasource.url=jdbc:mysql://localhost:3306/ssm
spring.datasource.username=ssm
spring.datasource.password=123456
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
```

```java
package org.yzh.pojo;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@Data
@AllArgsConstructor
@NoArgsConstructor
public class Hex {

    private String session;
    private String message;
    private String hex;
}
```

###  3.数据库Mapper

```java
package org.yzh.mapper;

import org.apache.ibatis.annotations.Insert;
import org.apache.ibatis.annotations.Mapper;
import org.yzh.pojo.Hex;

@Mapper
public interface HexMapper {

    // 查询用户信息
    @Insert("INSERT into hex (session, message, hex) values (#{session}, #{message}, #{hex})")
    public void insert(Hex hex);
}
```

###  4.pom文件


```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
    <modelVersion>4.0.0</modelVersion>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.7.5</version>
    </parent>

    <groupId>org.yzh</groupId>
    <artifactId>jtt808-demo</artifactId>
    <version>1.0.0-SNAPSHOT</version>
    <packaging>jar</packaging>

    <properties>
        <java.version>1.8</java.version>
        <resource.delimiter>@</resource.delimiter>
        <maven.compiler.source>${java.version}</maven.compiler.source>
        <maven.compiler.target>${java.version}</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
        <maven.test.skip>true</maven.test.skip>
    </properties>

    <url>https://gitee.com/yezhihao/jt808-server</url>
    <licenses>
        <license>
            <name>The Apache Software License, Version 2.0</name>
            <url>https://www.apache.org/licenses/LICENSE-2.0.txt</url>
        </license>
    </licenses>

    <dependencies>
        <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-boot-starter</artifactId>
            <version>3.0.0</version>
        </dependency>
        <dependency>
            <groupId>com.github.xiaoymin</groupId>
            <artifactId>knife4j-spring-ui</artifactId>
            <version>3.0.3</version>
        </dependency>
        <dependency>
            <groupId>com.google.code.gson</groupId>
            <artifactId>gson</artifactId>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
            <exclusions>
                <exclusion>
                    <groupId>org.springframework.boot</groupId>
                    <artifactId>spring-boot-starter-logging</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-webflux</artifactId>
            <exclusions>
                <exclusion>
                    <groupId>org.springframework.boot</groupId>
                    <artifactId>spring-boot-starter-reactor-netty</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-jetty</artifactId>
            <exclusions>
                <exclusion>
                    <groupId>org.apache.tomcat.embed</groupId>
                    <artifactId>tomcat-embed-el</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-log4j2</artifactId>
        </dependency>
        <dependency>
            <groupId>com.github.ben-manes.caffeine</groupId>
            <artifactId>caffeine</artifactId>
        </dependency>
        <dependency>
            <groupId>io.netty</groupId>
            <artifactId>netty-handler</artifactId>
        </dependency>

        <dependency>
            <groupId>io.github.yezhihao</groupId>
            <artifactId>protostar</artifactId>
            <version>3.0.4</version>
        </dependency>
        <dependency>
            <groupId>io.github.yezhihao</groupId>
            <artifactId>netmc</artifactId>
            <version>3.0.4</version>
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
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

    <repositories>
        <repository>
            <id>central</id>
            <name>Maven Central</name>
            <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
            <layout>default</layout>
            <releases>
                <enabled>true</enabled>
            </releases>
        </repository>
    </repositories>
</project>
```