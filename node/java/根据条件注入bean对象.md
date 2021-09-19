

```java
public class MyCondition implements Condition {
	@Override
	public boolean matches(ConditionContext context, AnnotatedTypeMetadata metadata) {
        try {
            InetAddress addr = InetAddress.getLocalHost();
            String host = addr.getHostAddress();
            if(host.equals("111.111.111.111")){
                return true;
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
        return false; 
		// return context.getEnvironment().getProperty("os.name").contains("Linux");
	}
}
```



```
@Configuration
@Conditional(MyCondition.class)
public class ConditionConfig {
	
	@Bean
	public ListService windowsListService() {
		return new WindowsListServiceImpl();
	}
	
	@Bean
	public ListService linuxListService() {
		return new LinuxListServiceImpl();
	}
}
```

