@Scheduled 注解的方法必须是无参数的，且必须要有 cron、fixedRate、fixDelay等属性之一

fixedRate：在调用之间以毫秒为单位执行带有固定时间段的注释方法。

fixedRateString: 字符串格式

fixDelay:在最后一次调用结束和下一次调用结束之间以毫秒为单位执行带注释的方法。

fixDelayString：字符串格式

可以使用 [@Scheduled（cron =“...”）表达式](https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/scheduling/support/CronSequenceGenerator.html) 来实现更复杂的任务调度。

一些cron pattern:

- "0 0 * * * *" = the top of every hour of every day.
- "*/10 * * * * *" = every ten seconds.
- "0 0 8-10 * * *" = 8, 9 and 10 o'clock of every day.
- "0 0 6,19 * * *" = 6:00 AM and 7:00 PM every day.
- "0 0/30 8-10 * * *" = 8:00, 8:30, 9:00, 9:30, 10:00 and 10:30 every day.
- "0 0 9-17 * * MON-FRI" = on the hour nine-to-five weekdays
- "0 0 0 25 12 ?" = every Christmas Day at midnight

demo

```
├─src
│  ├─main
│  │  └─java
│  │      └─com
│  └─test
│      └─java
```

`pom.xml`

```xml
  <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>1.5.7.RELEASE</version>
    </parent>

    <properties>
        <java.version>1.8</java.version>
    </properties>

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

`src/main/java/com/ScheduledTasks.java`

```java
package com;

import java.text.SimpleDateFormat;
import java.util.Date;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.scheduling.annotation.Scheduled;
import org.springframework.stereotype.Component;

@Component
public class ScheduledTasks {

	private static final Logger log = LoggerFactory.getLogger(ScheduledTasks.class);

	private static final SimpleDateFormat dateForamt = new SimpleDateFormat("HH:mm:ss");

	@Scheduled(fixedRateString = "5000") // 等同于fixedRate = 5000
	public void reportCurrentTime() {
		log.info("The time is now {}", dateForamt.format(new Date()));
	}
}

```

`src/main/java/com/Application.java`

```java
package com;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.scheduling.annotation.EnableScheduling;

@SpringBootApplication
@EnableScheduling
// @EnableScheduling确保创建后台任务执行程序。没有它，就不会执行任何计划。
public class Application {

	public static void main(String[] args) {
		SpringApplication.run(Application.class, args);
	}
}

```

