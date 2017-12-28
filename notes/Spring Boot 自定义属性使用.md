[back](https://github.com/FG716/MyNotes)
---
application.properties

```properties
app.name=demo
```

使用
```java
import org.springframework.beans.factory.annotation.Value;

@Value("${app.name}") 
private String appName
```

发现一个问题，@Value注解的属性不能在构造器中调用！！！否则会出错！！！(这是因为@Value不能用于 `static` 的变量)
