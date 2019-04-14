### Demo环境
- gradle：5.3.1
- Spring Boot ：2.1.4
- Spring Cloud：Greenwich.SR1

### Demo应用
- admin： 作为SBA管理端
- clientDemo： 作为SBA客户端，也就是被监控的微服务实例
- Eureka： 注册中心

### 集成逻辑
1. 启动注册中心
2. 把admin作为微服务实例注册到Eureka
3. 将clientDemo注册到Eureka

### 启动注册中心
- [Running-Eureka-Service](https://cloud.spring.io/spring-cloud-static/Greenwich.RELEASE/single/spring-cloud.html#spring-cloud-running-eureka-server)
- 因为暂时不用考虑注册中心高可用，单点部署一个即可 [Standalone-Mode](https://cloud.spring.io/spring-cloud-static/Greenwich.RELEASE/single/spring-cloud.html#spring-cloud-eureka-server-standalone-mode)

### 启动Spring Boot Admin Server
- AdminApplication.java

```
@SpringBootApplication
@EnableAdminServer
@EnableDiscoveryClient
public class AdminApplication {

    public static void main(String[] args) {
        SpringApplication.run(AdminApplication.class, args);
    }


}

```
- application.properties
  ```
    eureka.client.service-url.defaultZone=http://localhost:4999/eureka/
  ```

运行成功时，可以在注册中心看到SBA-Admin-Server 已经注册成功

### 启动Spring boot admin client
需要额外注意的是，在Spring boot 2.0 中，spring-boot-actuator中的endpoint，需要在配置文件中暴露出来，才能被admin-Server 所读取。
- application.properties

```
 management.endpoints.web.exposure.include=*
```

服务启动后，将会被添加到注册中心，admin-Server 则会通过Eureka，获取每个实例的监控信息，从而进行管理




### 参考链接
[spring-boot-admin in github](https://github.com/codecentric/spring-boot-admin)
