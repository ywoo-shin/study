## spring cloud
https://cloud.spring.io/spring-cloud-static/Greenwich.RELEASE/multi/multi_spring-cloud.html

## spring cloud case study
* https://www.slideshare.net/balladofgale/spring-camp-2018-11-spring-cloud-msa-1
* https://www.slideshare.net/balladofgale/11st-legacy-application-spring-cloud-microservices

### Zuul

### Hystrix (장애 전파 방지)
* Circuit Breaker
  * Circuit Open: `일정 시간` 동안 `일정 개수` 이상의 호출에서 `일정 비율의 에러`가 발생
  * Circuit Close: `일정 시간` 경과 후, 단 한 개의 요청에 대해 (`Half Open`), 성공
  ```properties
  hystrix.command.{commandKey}.metrics.rollingStats.timeInMilliseconds = 10
  hystrix.command.{commandKey}.circuitBreaker.requestVolumeThreshold = 20
  hystrix.command.{commandKey}.circuitBreaker.errorThresholdPercentage = 50
  hystrix.command.{commandKey}.circuitBreaker.sleepWindowInMilliseconds = 5
  ```
  * 한 process 내 commandKey 단위로 통계 산출
    * 동일한 boundary를 갖는 Hystrix는 동일한 commandKey로 grouping
  * `HystrixBadRequestException`
    * Circuit Open을 위한 통계 제외
  
* Fallback
  * Circuit Open 후, 원본 method 대신 실행되는 메소드 지정
    
* Timeout
  * Circuit Breaker(commandKey) 단위로 timeout 설정 
  ```properties
  hystrix.command.{commandKey}.execution.isolation.thread.timeoutInMilliseconds = 1
  ```    
    
* `Thread Isolation`


```java
@HystrixCommand(commandKey = "pay", fallbackMethod = "nextFallback")  
public String method() {  
}
```

```java
public class Sample extends HystrixCommand<String> {
  @Override
  protected String run() {
  }
}
```

### Eureka
* Dynamic Service Discovery (client/server)

### Ribbon
* Software Load Balancer
* HTTP 통신이 필요한 요소 내장
  * Zuul API GW
  * @LoadBalanced RestTemplate (RestTemplate가 Ribbon + Eureka 기능을 가지게 됨)
  * Feign - 선언적 HTTP client
  
* 설정 가능 요소
  * IRule
  * IPing
  * ServerList
  * ServerListFilter
  * ServerListUpdater
  * IClientConfig
  * ILoadBalancer
  
* Ribbon client의 Retry 기능 (spring-retry 존재 시)

```properties
<dependency>
    <groupId>org.springframework.retry</groupId>
    <artifactId>spring-retry</artifactId>
</dependency>
```

```yaml
pay:
  ribbon:
    MaxAutoRetries: 0
    MaxAutoRetriesNextServer: 1
    retryableStatusCodes: 500
```  
  
### Feign
```java
@FeignClient(name = "pay")
public interface FeignPayService {
    @GetMapping("/services/{serviceId}") 
    String getService(@PathVariable String serviceId);   
}
```

### Config 
* git 기반의 Config 관리 (client / server)

### Sleuth
* Distributed Tracing Solution
  * Trace ID(uuid)가 log에 출력됨

### Monitoring
* Zipkin
* Turbine
* Spring Boot Admin


