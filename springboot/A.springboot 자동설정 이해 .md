### 1. 자동 설정 구현
https://docs.spring.io/spring-boot/docs/2.1.3.RELEASE/reference/html/boot-features-developing-auto-configuration.html


#### 1.1 자동설정 jar 생성

* 의존성 추가
```xml
<!-- 필요 library -->
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-autoconfigure</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-autoconfigure-processor</artifactId>
        <optional>true</optional>
    </dependency>
</dependencies>

<!-- 의존성 관리 -->
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-dependencies</artifactId>
            <version>2.0.3.RELEASE</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```

* @Configuration clsss
```java
package com.toast.springboot;

@Configuration
public class ToastAutoConfiguration {
    @Bean
    public Toast toast() {
    	return new Toast("so happy");
    }	
}
```
* src/main/resource/META-INF에 spring.factories 파일 생성
* spring.factories 안에 자동 설정 파일 추가
```
org.springframework.boot.autoconfigure.EnableAutoConfiguration = com.toast.springboot.ToastAutoConfiguration
```
 
* mvn install (jar 파일 생성되어 ~/.m2/repository에 생성되어 다른 프로젝트에서 사용할 수 있음)

---

#### 1.2 자동설정 이해
##### 자동 설정된 Bean을 사용하는 곳에서 Override가 필요할 경우
* @ConditionalOnMissingBean (Bean이 없을 경우에만, DI 하는 annotation) 

```java
package com.toast.springboot;

@Configuration
public class ToastAutoConfiguration {
    @Bean
    @ConditionalOnMissingBean
    public Toast toast() {
    	Toast toast = new Toast();
    	toast.setTile("so happy");
    	return toast;
    }	
}
```

##### Bean의 설정 값들을 Properties로 분리해 설정

```java
@ConfigurationProperties("toast")
public class ToastProperties {
	private String title;	
}

@Configuration
@EnableConfigurationProperties(ToastProperties.class)
public class ToastAutoConfiguration {
    @Bean
    @ConditionalOnMissingBean
    public Toast toast(ToastProperties properties) {
    	return new Toast(properties.getTitle);
    }	
}
```