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