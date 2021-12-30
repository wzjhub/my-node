# Eureka 注册中心



## 1.Eureka 简介

​		Spring Cloud Eureka是Spring Cloud Netflix 微服务件的一部分，基于 Netflix Eureka  做了 二次封装，主要负责实现微服务架构中的服务治理功能 Spring Cloud Eureka 是一个 基于 REST 的服务，并且提供了基于 Java 的客户端组件 能够非常方便地将服务注册到 Spring Cloud Eureka 中进行统一管理。服务治理在微服务架构中是必不可以少的部分。阿里开源的 Dubbo 框架就是针对于服务治理的。

​		服务治理必须要有个注册中心，除了用Eureka作为注册中心外，我们还可以使用 Consul、Etcd、Zookeeper 等来作为服务的注册中心。用过 Dubbo 的人应该清楚，Dubbo 中也有几种注册中心，比如基于 Zookeeper， 基于 Redis 的等，用得最 的还 Zookeeper方式 至于使用哪种方式都是可以的，注册中心无非就是管理所有服务的信息和状态。若用我们生活中的列子来说明的话，个人觉得12306网站比较合适：

​		首先，12306网站就好比一个注册中心，我们的顾客就好比调用的客户端， 当我坐火车时，会去 12306 网站上看有没有票，有票就可以购买，然后拿到火车的班次，时间等，最后出发。程序也是一样， 需要调用某个服务的时候，你会先去 Eureka 中去拉取服务列表，查看你调用的服务在不在其中，在的话就拿到服务地址、端口等信息，然后调用。

​		注册中 心带来的好处就是你不需要知道有多少提供方，你只需要关注注册中心即可，你不必关心有多少火车在运行，你只需要去 12306 网站上看有没有票可以买就可以了。

### 问：为什么 Eureka 比 Zookeeper 更适合作为注册中心呢？

​		主要是因为 Eureka 是基于 AP 原则构建的，而 ZooKeeper 是基于 CP 原则构建的 。在分布式系统领域有个著名的 CAP 定理， 即C为数据一致性； A为服务可用性；P为服务对网络分区故障的容错性。这三个特性在 任何分布式系统中都不能同时满足，最多同时满足两个。

​		Zookeeper有一个Leader ，而且在Leader 无法使用的时候通过Paxos ( ZAB ）算法选举出一个新的Leader ，这个 Leader的任务就是保证写数据的时候只向这个Leader写入，Leader会同步信息到其他节点，通过这个操作就可以保证数据的一致性。总而言之，想要保证 AP 就要用Eureka，想要保证 CP 就要用 Zookeeper Dubbo 中大部分都是基于 Zookeeper作为注册中心的，Spring Cloud 当然首选 Eureka。



## 2.使用 Eureka 编写注册中心服务

### 1.创建项目，添加依赖

首先创建一个 Maven项目，在pom中配Eureka的依赖信息。

```html
    <!-- Spring Boot -->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>1.5.4.RELEASE</version>
        <relativePath />
    </parent>

    <dependencies>
    <!--eureka-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-eureka-server</artifactId>
        </dependency>
    </dependencies>
    
    <!--spring Cloud-->
    <dependencyManagement>
        <dependencies>
            <!--一个依赖管理器的pom文件,是对cloud的依赖管理,如：spring-cloud-starter-config、spring-cloud-starter-netflix-eureka-server -->
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>Dalston.SR4</version>
                <type>pom</type>
                  <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
```

### 2.创建启动类

创建一个启动类EurekaServerApplication，跟我们之前使用的SpringBoot几乎完全一样，只是多了一个＠EnableEureka Server注解，表示开启Eureka Server。

```java
@EnableEurekaServer //EurekaServer 服务器端的启动类, 接收其他微服务注册进来
@SpringBootApplication
public class EurekaServerApplication {
    public static void main(String[] args) {
        SpringApplication.run(EurekaServerApplication.class, args);
    }
}
```

### 3.编写配置文件

在src/main/resources下面创建一个application.yml（application. properties）属性文件，编写配置文件。

```
server:
  port: 8888                #配置端口为8888

eureka:
  instance:
    hostname: localhost     #eureka服务端的实例名称. 由于目前只有一个eureka, 因此只需写一个localhost
  client:
    register-with-eureka: false     #该微服务是否将自身信息暴漏给EurekaServer。
    fetch-registry: false           #是否可以从EurekaServer中查找服务。
    service-url:
      #单机的设置  ${eureka.instance.hostname} 取出eureka服务实例名称   ${server.port} 取出端口
      defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/       #设置与Eureka Server交互的地址查询服务和注册服务都需要依赖这个地址（单机）。
  server:
    enable-self-preservation: false           #是否开启自我保护
    eviction-interval-timer-in-ms: 60000      #服务注册表清理间隔
security:
  basic:
    enabled: true      #开启认证
  user:
    name: admin        #用户名
    password: 123456   #密码
```

### 4.启动注册中心

接下来就可以直接运行EurekaServerApplication就可以启动我们的注册中心服务了。

![1576829108633](C:\Users\Y小狗的信仰。\AppData\Roaming\Typora\typora-user-images\1576829108633.png)



## 3.编写服务提供者

### 1.创建项目注册到 Eureka

注册中心已经创建并且启动好了，接下来我们实现将一个服务提供者注册到Eureka中，并提供一个接口给其他服务调用。

#### （1）创建启动类

```java
@SpringBootApplication
@EnableDiscoveryClient  //表示当前服务是一个Eureka的客户端
public class TestProviderApplication {
    public static void main(String[] args) {
        SpringApplication.run(TestProviderApplication.class,args);
    }
}
```

启动类的方法与之前没有多大区别，就是注解变了，换成＠EnableDiscoveryClient，这个表示当前服务是Eureka的客户端。

#### （2）编写配置文件

在src/main/resources下面创建一个application.yml（application. properties）属性文件，编写配置文件。

```java
spring:
  application:
    name: test-provider
server:
  port: 8081
eureka:
  client:
   register-with-eureka: true  #该微服务是否将自身信息暴漏给EurekaServer，如果微服务提供者true。
   fetch-registry: false         #是否可以从EurekaServer中查找服务，如果微服务调用者true。
   service-url:
    defaultZone: http://localhost:8888/eureka/   #注册中心地址
```

#### （3）检查服务注册情况

执行启动服务检查我们的服务是否注册成功。回到我们之前打开Eureka Web控制台，刷新页面，就可以看到新注册的服务信息了。

![1576829813593](C:\Users\Y小狗的信仰。\AppData\Roaming\Typora\typora-user-images\1576829813593.png)

### 2.编写提供接口

#### （1）创建Controller，提供接口

创建一个Controller，提供一个接口给其他服务查询

```java
@RestController
@RequestMapping("/provider")
public class ProviderController {

    @GetMapping("/hello")
    public String hello(){
        return "hello！！！";
    }
}
```

#### （2）重启服务，访问接口

重启服务，通过访问 http://localhost:8081/provider/hello 如果能看到我们返回的hello字符串，就证明接口提供成功了。

![1576830288903](C:\Users\Y小狗的信仰。\AppData\Roaming\Typora\typora-user-images\1576830288903.png)



## 4.编写服务消费者

### 1.直接调用接口

#### （1）创建启动类

```java
@SpringBootApplication
@EnableDiscoveryClient  //表示当前服务是一个Eureka的客户端
public class TestConsumerApplication {
    public static void main(String[] args) {
        SpringApplication.run(TestConsumerApplication.class,args);
    }
}
```



#### （2）编写配置文件

```java
spring:
  application:
    name: test-consumer    #指定服务名
server:
  port: 8082               #服务端口
eureka:
  client:
   register-with-eureka: false  #该微服务是否将自身信息暴漏给EurekaServer，如果微服务提供者true。
   fetch-registry: true         #是否可以从EurekaServer中查找服务，如果微服务调用者true。
   service-url:
    defaultZone: http://localhost:8888/eureka/   #注册中心地址

```

#### （3）RestTemplate

RestTemplate是Spring提供的用于访问Rest服务的客户端， RestTemplate 提供了多种便捷访问远程Http服务的方法，能够大大提高客户端的编写效率。我们通过配置RestTemplate来调用接口。

```java
@Configuration
public class BeanConfiguration {
    @Bean
    public RestTemplate getRestTemplate() {
        return new RestTemplate();
    }
}
```

#### （4）创建接口

创建接口，在接口中调用 provider/hello 接口。

```java
@RestController
@RequestMapping("/consumer")
public class ConsumerController {

    @Autowired
    private RestTemplate restTemplate;

    @GetMapping("/callHello")
    public String callHello(){
        return restTemplate.getForObject("http://localhost:8081/provider/hello",String.class);
    }
}
```

启动消费者服务，通过访问 http://localhost:8081/provider/hello 如果能看到我们返回的hello字符串，就证明接口提供成功了。

### 2.通过Eureka来消费接口

上面我们是直接通过服务接口的地址来调用的，这样和我们之前的做法就是一样的了，完全没有用到 Eureka 带给我们的便利。既然用了注册中 ，那么客户端调用的时候肯定是不需要关心有多少个服务提供接口，下面我来改造之前的调用代码。

#### （1）改造RestTemplate 配置

首先改造 RestTemplate 配置，添加@LoadBalanced注解，这个注解会自动构造LoadBalancerClient接口的实现类并注册到Spring容器中。

```java
@Configuration
public class BeanConfiguration {
    @Bean
    @LoadBalanced     //开启负载均衡的功能
    public RestTemplate getRestTemplate() {
        return new RestTemplate();
    }
}
```

#### （2）改造调用代码

不再直接写固定地址， 是写成服务的名称，这个名称也就是我们注册到 Eureka 中的名称，是属性文件中的spring.application.name。

```java
@RestController
@RequestMapping("/consumer")
public class ConsumerController {

    @Autowired
    private RestTemplate restTemplate;

    @GetMapping("/callHello")
    public String callHello(){
        return restTemplate.getForObject("http://test-provider/provider/hello",String.class);
    }
}
```

#### （3）重启服务，访问接口

启动消费者服务，通过访问 http://localhost:8082/consumer/callHello 如果能看到我们返回的hello字符串，就证明接口提供成功了。

![1576832041488](C:\Users\Y小狗的信仰。\AppData\Roaming\Typora\typora-user-images\1576832041488.png)



## 5.开启 Eureka 认证

Eureka 自带了一个 Web 的管理页面，方便我们查询注册到上面的实例信息，但是有一个问题：如果在实际使用中，注册中心地址有公网IP 的话，必然能直接访问到，这样是不安全的 所以我们需要对Eureka进行改造，加上权限认证来保证安全性。

我们需要改造我们的EurekaServer，通过集成Spring-Security来进行安全认证。

### 1.pom.xml 中添加Spring Security的依赖包

```java
<dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-security</artifactId>
        </dependency>
    </dependencies>
```

### 2.在application.yml中加上认证的配置信息

```java
security:
  basic:
    enabled: true      #开启认证
  user:
    name: admin        #用户名
    password: 123456   #密码
```

重新启动注册中心，访问http: //localhost:8888/ ，此时浏览器会提示你输入用户名和密码，输入正确后才能继续访问 Eureka提供的管理页面。

![1576832613488](C:\Users\Y小狗的信仰。\AppData\Roaming\Typora\typora-user-images\1576832613488.png)

在 Eureka 开启认证后，客户端注册的配置也要加上认证的用户名和密码信息。

服务提供者：

```java
spring:
  application:
    name: test-provider
server:
  port: 8081
eureka:
  client:
   register-with-eureka: true  #该微服务是否将自身信息暴漏给EurekaServer，如果微服务提供者true。
   fetch-registry: false         #是否可以从EurekaServer中查找服务，如果微服务调用者true。
   service-url:
    defaultZone: http://admin:123456@localhost:8888/eureka/   #注册中心地址
```

服务消费者：

```java
spring:
  application:
    name: test-consumer    #指定服务名
server:
  port: 8082               #服务端口
eureka:
  client:
   register-with-eureka: false  #该微服务是否将自身信息暴漏给EurekaServer，如果微服务提供者true。
   fetch-registry: true         #是否可以从EurekaServer中查找服务，如果微服务调用者true。
   service-url:
    defaultZone: http://localhost:8888/eureka/   #注册中心地址
```



## 6.Eureka 高可用搭建

### 1.高可用原理

前面我们搭建的注册中心只适合本地开发使用，在生产环境中必须搭建一个集群来保证高可用。Eureka的集群搭建方法很简单：每一台Eureka只需要在配置中指定另外多个Eureka的地址就可以实现个集群的搭建了。

下面我们以3个节点为例来说明搭建方式，假设我们有EurekaServerApplication，EurekaServerApplication2，EurekaServerApplication3三台机器，需要做的就是：

（1）将EurekaServerApplication注册到EurekaServerApplication2，EurekaServerApplication3上面

（2）将EurekaServerApplication2注册到EurekaServerApplication，EurekaServerApplication3上面

（3）将EurekaServerApplication3注册到EurekaServerApplication，EurekaServerApplication2上面

### 2.搭建步骤

2.1.1 修改配置文件application.yml

```java
server:
  port: 8887 #配置端口为8887
spring:
  application:
    name: eureka-server   #注册中心应用名称
eureka:
  server:
    enableSelfPreservation: false     #是否开启自我保护
  client:
    fetch-registry: true          #一定要设置为true或者不写，否则会出现unavailable-replicas
    register-with-eureka: true    #一定要设置为true或者不写，否则会出现unavailable-replicas
    service-url:
      defaultZone: http://admin:123456@127.0.0.1:8888/eureka/,http://admin:123456@127.0.0.1:8889/eureka/
security:
  basic:
    enabled: true      #开启认证
  user:
    name: admin        #用户名
    password: 123456   #密码
```

2.1.2  启动EurekaServerApplication

2.1.3 访问localhost:8887，登录注册中心查看。

![1578625645405](C:\Users\Y小狗的信仰。\AppData\Roaming\Typora\typora-user-images\1578625645405.png)

2.2.1 修改配置文件application.yml

```java
server:
  port: 8888 #配置端口为8888
spring:
  application:
    name: eureka-server   #注册中心应用名称
eureka:
  server:
    enableSelfPreservation: false     #是否开启自我保护
  client:
    fetch-registry: true          #一定要设置为true或者不写，否则会出现unavailable-replicas
    register-with-eureka: true    #一定要设置为true或者不写，否则会出现unavailable-replicas
    service-url:
      defaultZone: http://admin:123456@127.0.0.1:8887/eureka/,http://admin:123456@127.0.0.1:8889/eureka/
security:
  basic:
    enabled: true      #开启认证
  user:
    name: admin        #用户名
    password: 123456   #密码
```

2.2.2  启动EurekaServerApplication2

2.2.3 访问localhost:8888，登录注册中心查看。

![1578625820421](C:\Users\Y小狗的信仰。\AppData\Roaming\Typora\typora-user-images\1578625820421.png)

2.3.1 修改配置文件application.yml

```java
server:
  port: 8889 #配置端口为8889
spring:
  application:
    name: eureka-server   #注册中心应用名称
eureka:
  server:
    enableSelfPreservation: false     #是否开启自我保护
  client:
    fetch-registry: true          #一定要设置为true或者不写，否则会出现unavailable-replicas
    register-with-eureka: true    #一定要设置为true或者不写，否则会出现unavailable-replicas
    service-url:
      defaultZone: http://admin:123456@127.0.0.1:8887/eureka/,http://admin:123456@127.0.0.1:8888/eureka/
security:
  basic:
    enabled: true      #开启认证
  user:
    name: admin        #用户名
    password: 123456   #密码
```

2.2.2  启动EurekaServerApplication3

2.2.3 访问localhost:8889，登录注册中心查看。

![1578626643533](C:\Users\Y小狗的信仰。\AppData\Roaming\Typora\typora-user-images\1578626643533.png)

​		经过上述步骤，我们就将三个注册中心相互注册了，无论谁挂掉了，应用都能继续使用存活的这个注册中心。之前在客户端中我们通过配置 eureka.clients service-url.defaultZone来指定对应的注册中心，当我们的注册中心有多个节点后，那就需要修改eureka.clients service-url.defaultZone配置为多个节点的地址，多个地址用英文逗号隔开即可：

服务提供方：

```java
spring:
  application:
    name: test-provider
server:
  port: 8081
eureka:
  client:
   register-with-eureka: true  #该微服务是否将自身信息暴漏给EurekaServer，如果微服务提供者true。
   fetch-registry: false         #是否可以从EurekaServer中查找服务，如果微服务调用者true。
   service-url:
    defaultZone: http://admin:123456@127.0.0.1:8887/eureka/,http://admin:123456@127.0.0.1:8888/eureka/,http://admin:123456@127.0.0.1:8889/eureka/
```

服务消费方：

```java
spring:
  application:
    name: test-consumer #指定服务名
server:
  port: 8082  #服务端口
eureka:
  client:
   register-with-eureka: false  #该微服务是否将自身信息暴漏给EurekaServer，如果微服务提供者true。
   fetch-registry: true         #是否可以从EurekaServer中查找服务，如果微服务调用者true。
   service-url:
     defaultZone: http://admin:123456@127.0.0.1:8887/eureka/,http://admin:123456@127.0.0.1:8888/eureka/,http://admin:123456@127.0.0.1:8889/eureka/   #注册中心地址
```



## 7.常用配置讲解

### 1.关闭自我保护

​		一旦进入保护模式，Eureka Server 将会尝试保护其服务的注册表中的信息，不再删除服务注册表中的数据，当网络故障恢复后，该 Eureka Server 节点会自动退出保护模式。

![1578626643533](C:\Users\Y小狗的信仰。\AppData\Roaming\Typora\typora-user-images\1578626643533.png)

### 2.快速移除已经失效的服务信息

​		在实际开发 ，我们可能会不停地重启服务，由于ureka有自己的保护机制，故节点下线后，服务信息还是一直存在于 Eureka中。我们可以通过增加一些配置让移除的速度更快一点，当然只在开发环境下使用，生产不推荐使用。

首先在我们的 application.yml中增加两个配置，分别是关闭自我保护和清理间隔：

```java
eureka.server.enable-self-preservation=false 
eureka.server.eviction-interval-timer-in-ms=5OOO
```

然后在具体的客户端服务中配置下面的内容：

```java
eureka.client.healthcheck.enabled=true
eureka.instance.lease-renewal-interval-in-seconds=5
eureka.instance.lease-expiration-duration-in-seconds=5
```

eureka.client.healthcheck.enbled 用于开启健康检查， 需要 pom.ml 中引入 actuator 的依赖：

```java
<dependency> 
<groupId>org.springframework.boot</groupId>
<artifactId>sping-boot-stater-actuator</artifactId>
</dependency>
```

其中：

eureka.instance.lease-renewal-interval-in-seconds 表示 Eureka Client 发送💗跳给 server端的频率。

eureka.instance.lease-expiration-duration-in-seconds 表示 Eureka Server 至上一次收到 client 的心跳之后，等待下一次心跳的超时时间，在这个时间内若没收到下一次心跳，则移除该 Instance。



## 8.扩展使用

### 1.Eureka REST API 

​		Eureka 作为注册中心，其本质是存储了每个客户端的注册信息， Ribbon 在转发的时候会获注册中心的服务列表， 然后根据对应的路由规则来选择一个服务给 Feign 进行调用。利用 Eureka 提供的 API 我们可以获取某个服务的实例信息：

获取某个服务的注册信息，可以直接 GET 请求：http://localhost:8888/eureka/apps/test-provider。

其中，test-provider是应用名称，也就是 spring.application.name。在浏览器中数据的显示格式默认是 XML格式的：

![1578640613801](C:\Users\Y小狗的信仰。\AppData\Roaming\Typora\typora-user-images\1578640613801.png)

如果想返回 Json 数据的格式，可以用一些接口测试工具来请求， 比如 Postman ，在请求头中添加下面两行代码即可：

```java
Content-Type:application/json 
Accept:application/json
```

如果 Eureka 开启了认证，记得添加认证信息，用户名和密码必须是 Base64 编码过的Authorization:Basic 用户名：密码。Postman 直接支持了 Basic 认证 ，将选项从 Headers 换到 Authorization，选择认证方式为 Basic Auth 就可以填写用户信息了：

![1578641420689](C:\Users\Y小狗的信仰。\AppData\Roaming\Typora\typora-user-images\1578641420689.png)

填写之后，直接发起请求就可以了。我们可以切换到 Headers 选项中，可以看到请求头中已经多了一个 Authorization：

![1578641315183](C:\Users\Y小狗的信仰。\AppData\Roaming\Typora\typora-user-images\1578641315183.png)

### 2.元数据使用

​		Eureka 的元数据有两种类型，分别是框架定好了的标准元数据和用户自定义元数据。标准元数据指的是主机名、 IP 地址 端口号、状态页和健康检查等信息，这些信息都会被发布在服务注册表中，用于服务之间的调用自定义元数据可以使用 eureka.instance. metadataMap 进行配置。

​		自定义元数据说得通俗点就是自定义配置，我们可以为每个 Eureka Client 定义一些属于自己的配置，这个配置不会影响 Eureka 的功能。自定义元数据可以用来做一些扩展信息，比如灰度发布之类的功能，可以用元数据来存储灰度发布的状态数据， Ribbon 转发的时候就可以根据服务的元数据来做一些处理。当不需要灰度发布的时候可以调用 Eureka 供的 REST API 将元数据清除掉。

下面我们来自定义一个简单的元数据，在属性文件中配置如下：

```java
eureka.instance.metadataMap.TELLHOW=666
```

上述代码定义了一个 key为 TELLHOW 的配置， value是666 重启服务，然后通过Eureka提供的 REST API 查看刚刚配 元数据是否已经存在于 ureka 中：

![1578643425135](C:\Users\Y小狗的信仰。\AppData\Roaming\Typora\typora-user-images\1578643425135.png)

### 3.EurekaClient 使用

​		当我们的项目中集成了 Eureka 之后，可以通过 EurekaClient 来获取一些我们想的数据，比如上节讲的元数据我们就可以直接通过 EurekaClient 来获取, 不用去调用 Eureka 提供的 REST API。

```java
public class serviceUrl {
    @Autowired
    private EurekaClient eurekaClient;
    @GetMapping("/infos")
    public Object serviceUrl() {
        return eurekaClient.getInstancesByVipAddress("eureka-server",false);
    }
}
```

通过 PostMan 来调用接口看看有没有返回我们想要的数据:

![1578643425135](C:\Users\Y小狗的信仰。\AppData\Roaming\Typora\typora-user-images\1578643425135.png)

​		通过 EurekaClient 获取的数据跟我们自己利用 Eureka 提供的 API取的数据是一样的， 从使用角度来说这个比较方便，除了使用 EurekaClient 还可以使用DiscoveryClient，这个不是Feign 自带的，是 Spring Cloud 重新封装的， 类的路径为 org.springframework.cloud.client.discovery.DiscoeryClient。

```JAVA
    @Autowired
    private EurekaClient eurekaClient;
    @GetMapping("/infos")
    public Object serviceUrl() {
        return discoveryClient.getInstances("eureka-server");
    }
```

### 4.健康检查

​		默认情况下，Eureka客户端是使用心跳和服务端通信来判断客户端是否存活， Spring Boot Actuator提供了／health 端点，该端点可展示应用程序的健康信息， 可以将健康信息传递给 Eureka 服务端。通过配置如下内容开启健康检查：

```java
eureka.client.healthcheck.enabled=true
```

注意！！！：eureka.client.healthcheck.enabled=true 只有配置在applicaion.yml中才能生效。

Eureka 默认健康检查的页面 health ，这是 Spring Boot Actuator 提供的默认终端点。上面代码中的 healthCheckUrl 是健康检查的地址，如果我们的项目中用到 context-path，那么这个地址就是错误的了，解决这个问题的办法就是自己配置 healthCheckUrl地址。

首先我们在属性文件中 一个 context-path 来模拟这个问题：

```java
context-path: /pro
```

然后我们再次查看实例的信息：

当我们的访问地址中已经加上了 context-path 时，若healthCheckUrl还是以前的地址，则通过这个地址去访问健康检查页肯定是不存在的，所以我们需要自己去配置这个地址：

```javav
health-check-url-path: ${server.context-path}/health
```

然后我们再看信息，就会发现已经变成了我们配置的地址，并加上了 context-path，如图：

![1578647295054](C:\Users\Y小狗的信仰。\AppData\Roaming\Typora\typora-user-images\1578647295054.png)

