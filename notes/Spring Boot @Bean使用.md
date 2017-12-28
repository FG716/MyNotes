[back](https://github.com/FG716/MyNotes)
---

[原文](http://zetcode.com/articles/springbootbean/)

##Spring @Bean注解使用

Spring `@Bean`表示该方法产生的一个由Spring容器来管理的实体。它是方法级的注解。在Java配置（ `@Configuration` ）期间，该方法被执行并且它的返回值在 `BeanFactory` 中被注册为一个bean。

## Spring Boot @Bean 例子

```
$ tree
.
├── pom.xml
└── src
    └── main
        ├── java
        │   └── com
        │       └── zetcode
        │           ├── Application.java
        │           └── AppName.java
        └── resources
            ├── application.properties
            └── logback.xml
```

pom.xml

```xml
    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
    </properties>
    
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>1.5.2.RELEASE</version>
    </parent>    
    
    <dependencies>
        
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
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
```

AppName.java

```java
package com.zetcode;

public interface AppName {
    
    public String getName();    
}
```

application.properties

```properties
spring.main.banner-mode=off #关闭导航条
app.name=SpringBootBean     #自定义属性
```

logback.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <include resource="org/springframework/boot/logging/logback/base.xml" />
    <logger name="org.springframework" level="ERROR"/>
    <logger name="com.zetcode" level="ERROR"/>
</configuration>
```

Application.java

```java
package com.zetcode;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.Bean;

@SpringBootApplication
public class Application implements CommandLineRunner {

    @Autowired
    private AppName appName;

    @Bean
    public AppName getAppName(@Value("${app.name}") String appName) {

        return new AppName() {

            @Override
            public String getName() {
                return appName;
            }
        };
    }

    @Override
    public void run(String... args) throws Exception {
        
        System.out.println(appName.getName());
    }
    
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }    
}
```

运行
```shell
$ mvn spring-boot:run -q
SpringBootBean
```

`-q` 忽略maven相关信息
