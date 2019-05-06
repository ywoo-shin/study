## spring cloud
https://cloud.spring.io/spring-cloud-static/Greenwich.RELEASE/multi/multi_spring-cloud.html

## spring cloud case study
https://www.slideshare.net/balladofgale/spring-camp-2018-11-spring-cloud-msa-1

### Hystrix (장애 전파 방지)
* Circuit Breaker
  * Circuit Open: `일정 시간` 동안 `일정 개수` 이상의 호출에서 `일정 비율의 에러`가 발생
  * Circuit Close: `일정 시간` 경과 후, 단 한 개의 요청에 대해 (`Half Open`), 성공
  ```properties
  metrics.rollingStats.timeInMilliseconds = 10
  circuitBreaker.requestVolumeThreshold = 20
  circuitBreaker.errorThresholdPercentage = 50
  circuitBreaker.sleepWindowInMilliseconds = 5
  ```

* Fallback

* Thread Isolation

* Timeout

```java
@HystrixCommand
public String method() {  
}
or
public class Sample extends HystrixCommand<String> {
  @Override
  protected String run() {
  }
}
```

### Eureka

### Ribbon

### Zuul

### Config

### Sleuth
