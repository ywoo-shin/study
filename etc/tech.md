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


## Token
* seed = encode ('id' + 'timestamp' + 'randmon digit')
* token = encode ('seed' + 'ttl' + 'token-timeout' + 'uuid') 


## How to get HttpServletRequest from HttpRequest
```
RequestAttributes requestAttributes = RequestContextHolder.getRequestAttributes();
HttpServletRequest httpServletRequest = null;
if (requestAttributes instanceof NativeWebRequest) {
    httpServletRequest = (HttpServletRequest) ((NativeWebRequest) requestAttributes).getNativeRequest();

} else if (requestAttributes != null) {
    httpServletRequest = ((ServletRequestAttributes)requestAttributes).getRequest();
}
```

## How to convert String to LocalDateTime at ModelMapper
```
ModelMapper modelMapper = new ModelMapper();
modelMapper.getConfiguration().setMatchingStrategy(MatchingStrategies.STRICT);

Converter<String, LocalDateTime> stringToDate = new AbstractConverter<String, LocalDateTime>() {
	@Override
	protected LocalDateTime convert(String source) {
		DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyyMMddHHmmss");
		LocalDateTime localDateTime = LocalDateTime.parse(source, formatter);
		return localDateTime;
	}
};
modelMapper.addConverter(stringToDate);
return modelMapper;
```

