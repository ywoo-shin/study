## ObjectMapper
* spring에서 new ObjectMapper()로 커스텀 싱글톤 객체 생성하면, jdk8이상부터 LocalDatetime 등에 의해 역직력화가 안 된다.
  * registerModule(new JavaTimeModule) 추가해줘야 함
  * WRITE_DATE_KEYS_AS_TIMESTAMPS은 dateTime을 String으로 변경함
  
```
public ObjectMapper unKnownFalseObjectMapper() {
    return new ObjectMapper().registerModule(new JavaTimeModule())
            .disable(SerializationFeature.WRITE_DATE_KEYS_AS_TIMESTAMPS)
            .disable(DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES);
}
```

## token
* seed = encode ('id' + 'timestamp' + 'randmon digit')
* token = encode ('seed' + 'ttl' + 'token-timeout' + 'uuid') 
