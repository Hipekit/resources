# 服务注册与发现

Eureka用于服务的注册于发现，分为Eureka Server和Eureka Client两部分，Eureka Servier用作服务注册中心，Eureka Client则作为服务的提供者向Eureka Server注册服务。

## 创建服务注册中心

创建一个Springboot项目，加入eureka server的依赖，pom文件相关依赖如下：

```xml
<properties>
    <java.version>1.8</java.version>
    <spring-cloud.version>Greenwich.SR2</spring-cloud.version>
</properties>
<dependencies>
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
    </dependency>
</dependencies>
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-dependencies</artifactId>
            <version>${spring-cloud.version}</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```

application.yml配置

```yml
#服务名称
spring:
  application:
    name: eureka-server
    
#eureka server端口号
server:
  port: 8090
  
# eureka server地址
eureka:
  instance:
    homename: localhost
    #指定register-with-eureka和fetch-registry为false表名这是eureka server
  client:
    register-with-eureka: false
    fetch-registry: false
```

- `fetch-registry:` 检索服务选项，当设置为True(默认值)时，会进行服务检索,注册中心不负责检索服务。
- `register-with-eureka:` 服务注册中心也会将自己作为客户端来尝试注册自己,为true（默认）时自动生效



通过在启动类上添加`@EnableEurekaServer`注册启动一个服务中心

```java
@EnableEurekaServer
@SpringBootApplication
public class SpirngCloudEurekaApplication {

    public static void main(String[] args) {
        SpringApplication.run(SpirngCloudEurekaApplication.class, args);
    }

}
```

访问yml中配置的路径http://localhost:8090

![](.\img\Eureka服务注册中心.jpg)



## 服务提供方

创建一个Springboot项目，加入eureka client的依赖，pom文件相关依赖如下：

```xml
<properties>
        <java.version>1.8</java.version>
        <spring-cloud.version>Greenwich.SR2</spring-cloud.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>

    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>${spring-cloud.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
```

application.yml配置

```yml
#服务名称
spring:
  application:
    name: eureka-client
    
#服务端口
server:
  port: 9001
  
#服务注册中心url，向注册中心注册服务
eureka:
  client:
    serviceUrl:
      defaultZone: http://localhost:8090/eureka
```

创建服务接口

```java
@RestController
public class DcController {
    @Autowired
    public DiscoveryClient discoveryClient;

    @GetMapping("/dc")
    public String dc(){
        String services = "Services：" + discoveryClient.getServices();
        System.out.println(services);
        return services;
    }
}
```

通过`@EnableDiscoveryClient`开启服务发现

```java
@EnableDiscoveryClient
@SpringBootApplication
public class EurekaClientApplication {

    public static void main(String[] args) {
        SpringApplication.run(EurekaClientApplication.class, args);
    }

}
```

* `@EnableDiscoveryClient`：基于spring-cloud-commons依赖，相当于一个公共的服务发现；
* `@EnableEurekaClient`：基于spring-cloud-netflix依赖，只能为eureka作用；



## 服务消费方

在微服务架构中，业务都会被拆分成一个独立的服务，服务与服务的通讯是基于 http restful 的。Spring cloud 有两种服务调用方式，一种是 ribbon + restTemplate，另一种是 feign。

### Ribbon

Spring Cloud Ribbon是基于Netflix Ribbon实现的一套客户端负载均衡的工具。它是一个基于HTTP和TCP的客户端负载均衡器。它可以通过在客户端中配置ribbonServerList来设置服务端列表去轮询访问以达到均衡负载的作用。

***

创建消费者，eureka-comsumer，pom文件相关内容

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-ribbon</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
```

***

application.yml配置文件

```yml
spring:
  application:
    name: client-ribbon
    
server:
  port: 9005

eureka:
  client:
    serviceUrl:
      defaultZone: http://localhost:8090/eureka
```

***

创建RestTemplate配置文件

```java
@Configuration
public class RestConfig {

    @Bean
    @LoadBalanced  //开启负载均衡
    public RestTemplate restTemplate(){
        return new RestTemplate();
    }
}
```

***

创建service服务，调用服务接口

```java
@Service
public class RibbonService {
    @Autowired
    private RestTemplate restTemplate;

    public String getMessage(){
        return restTemplate.getForObject("http://eureka-client/dc", String.class);
    }
}
```

* `eureka-client`为服务方的spring.application.name，Ribbon会根据服务名选项服务实例

***

创建contrller

```java
@RestController
public class RibbonController {

    @Autowired
    private RibbonService ribbonService;

    @GetMapping("/ribbon")
    public String getMessage(){
        return ribbonService.getMessage();
    }
}
```

***

启动类添加`@EnableDiscoveryClient`，开启服务发现

```java
@EnableDiscoveryClient
@SpringBootApplication
public class RestApplication {
    public static void main(String[] args) {
        SpringApplication.run(RestApplication.class, args);
    }
}
```

注意：可以通过修改服务方的端口，再次启动一个新的服务，以此观察负载均衡的效果



### Feign

Feign 是一个声明式的伪 Http 客户端，它使得写 Http 客户端变得更简单。使用 Feign，只需要创建一个接口并注解。它具有可插拔的注解特性，可使用 Feign 注解和 JAX-RS 注解。Feign 支持可插拔的编码器和解码器。Feign 默认集成了 Ribbon，并和 Eureka 结合，默认实现了负载均衡的效果

- Feign 采用的是基于接口的注解
- Feign 整合了 ribbon

***

创建消费方，comsumer-feign，pom文件相关内容

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-openfeign</artifactId>
</dependency>
```

***

application.yml

```properties
spring.application.name=comsumer-feign
server.port=1001
eureka.client.serviceUrl.defaultZone=http://localhost:8090/eureka
```

***

使用`@FeignClient`标注接口

```java
//eureka-client为服务提供方的名称
@FeignClient("eureka-client")
public interface MessageProvider {

    //dc为eureka-client方法的路径
    @GetMapping("/dc")
    public String message();
}
```

提供一个controller类调用接口

```java
@RestController
public class MessageController {

    @Autowired
    private MessageProvider messageProvider;

    @GetMapping("/message")
    public String message() {
        return messageProvider.message();
    }
}

```

启动类Application

```java
@EnableFeignClients
@EnableDiscoveryClient
@SpringBootApplication
public class ComsumerFeignApplication {

    public static void main(String[] args) {
        SpringApplication.run(ComsumerFeignApplication.class, args);
    }

}
```

* `@EnableFeignClients`：开启扫描Spring Cloud Feign客户端的功能
* 