# [일관성 있는 웹 서비스 인터페이스 설계를 위한 REST API 디자인 규칙](http://www.yes24.com/Product/Goods/17945500)

## 1장. REST 소개
### 1.6 REST API 설계
* REST API 설계하면서 자주 떠오르는 질문들 ...
  * Q. URI 경로 path segment는 언제 복수로 써야 하는가?
  * Q. resource의 상태를 업데이트하려면, 어떤 method를 사용해야 하는가?
  * Q. CRUD가 아닌 연산을 어떻게 URL에 매핑하는가?
  * Q. 특정한 시나리오에 가장 적합한 HTTP 응답은 무엇인가?
  * Q. resource 상태 표현의 버전은 어떻게 관리할 수 있는가?
  * Q. JSON에 포함된 hyperlink는 어떻게 구조화하는가?

## 2장. URI 식별자 설계
### 2.2 URI 형태
```
scheme "://" authority "/" path ["?" query] ["#" fragment]
```
* 슬래시 구분자(/)는 계층 관계를 나타내는 데 사용한다. 
* URI 마지막 문자로 슬래시(/)를 포함하지 않는다.
* 하이픈(-)은 URI 가독성을 높이는 데 사용한다.
* 밑줄(_)은 URI에 사용하지 않는다.
* URI경로에는 소문자가 적합하다
* 파일 확장자는 URI에 포함시키지 않는다.

### 2.5 리소스 원형

* document
  * 객체 인스턴스나 데이터베이스 레코드와 유사한 단일 개념
```
http://api.soccer.restapi.org/leagues/seattle
http://api.soccer.restapi.org/leagues/seattle/teams/trebuchet
http://api.soccer.restapi.org/leagues/seattle/teams/trebuchet/players/mike
```

* collection
  * 서버에서 관리하는 디렉터리라는 리소스
```
 http://api.soccer.restapi.org/leagues
 http://api.soccer.restapi.org/leagues/seattle/teams
 http://api.soccer.restapi.org/leagues/seattle/teams/trebuchet/players
```

* controller
  * CRUD라고 알려진 표준적인 메서드와는 논리적으로 매핑되지 않는 application 고유의 행동
  * 일반적으로 controller 이름은 URI경로의 제일 마지막 부분에 표시되며, 계층적으로 뒤따르는 자식 resource는 없음.

```
// client가 사용자에게 경고를 재전송하게 하는 controller
POST /alerts/245743/resend
```

* store


### 2.6 URI 경로 디자인
* document 이름으로는 단수 명사를 사용해야 한다.
```
http://api.soccer.restapi.org/leagues/seattle/teams/trebuchet/players/claudio
```

* collection 이름으로는 복수 명사를 사용해야 한다.
```
http://api.soccer.restapi.org/leagues/seattle/teams/trebuchet/players
```

* controller 이름으로는 동사나 동사구를 사용해야 한다.
```
http://api.college.restapi.org/students/morgan/register
http://api.example.restapi.org/lists/4324/dedupe
http://api.ognom.restapi.org/dbs/reindex
http://api.build.restapi.org/qa/nightly/runTestSuite
```

* 경로 부분 중 변하는 부분은 유일한 값으로 대체한다. (use pathvariable)
```
http://api.soccer.restapi.org/leagues/{league-id}/teams/{team-id}/players/{player-id}
```

* CRUD 기능을 나태는 것은 URI에 사용하지 않는다.
  * 반드시 지양해야 하는 패턴 (CRUD 기능을 수행하는 내용은 URI에 노출되면 안된다.)
  ```
  GET /deleteUser?id=1234
  GET /deleteUser/1234
  DELETE /deleteUser/1234
  POST /users/1234/delete
  ```

### 2-7. URI Query 디자인
* URI쿼리 부분으로 컬렉션이나 스토어를 필터링할 수 있다.
```
GET /users
GET /users?role=admin
```

* URI쿼리는 컬렉션이나 스토어의 결과를 페이지로 구분하여 나타내는데 사용해야 한다.
```
GET /users?pageSize=25&pageStartIndex=50 (50페이지부터 최대 75페이지까지만)
```

* URI 쿼리로 client의 page나 filtering의 요구 사항에 대응할 수 없다면, controller를 생각해봐야 한다.
```
// 맞춤 범위 형태나 특별한 소팅 순서 등이 RequestBody에 기술 가능
POST /users/search
```

## 3장. HTTP를 이용한 인터랙션 설계
### 3.2 요청 메소드
* GET, POST를 사용하여 다른 요청 method를 처리해서는 안된다.
* GET은 resource의 상태 표현을 얻는 데 사용해야 한다.
* 응답 header를 가져올 때는 반드시 HEAD를 사용해야 한다.
* PUT은 변경 가능한 resource를 갱신하는 데 사용해야 한다.
* POST는 collection에 새로운 resource를 만드는 데 사용해야 한다.
* POST는 controller를 실행하는 데 사용해야 한다.
* DELETE는 그 부모에서 resource를 삭제하는 데 사용해야 한다.
* OPTIONS 메서드는 리소스의 사용 가능한 인터랙션을 기술한 메타데이터를 가져오는 데 사용해야 한다

### 3.3 응답 상태 코드

HTTP STATUS | 설명
--- | ---
200 ("OK") | 일반적인 요청 성공을 나타내는 데 사용해야 한다.<br>응답 바디에 에러를 전송하는 데 사용해서는 안 된다. 
201 ("Created") | 성공적으로 리소스를 생성했을 때 사용해야 한다 
202 ("Accepted") | 비동기 처리가 성공적으로 시작되었음을 알릴 때 사용해야 한다 
204 ("No Content") | 응답 바디에 의도적으로 아무것도 포함하지 않을 때 사용한다 
301 ("Moved Permanently") | 리소스를 이동시켰을 때 사용한다 
303 ("See Other") | 다른 URI를 참조하라고 알려줄 때 사용한다 
304 ("Not Modified") | 대역폭을 절약할 때 사용한다 
307 ("Temporary Redirect") | 클라이언트가 다른 URI로 요청을 다시 보내게 할 때 사용해야 한다 
400 ("Bad Request") | 일반적인 요청 실패에 사용해야 한다 
401 ("Unauthorized") | 클라이언트 인증에 문제가 있을 때 사용해야 한다 
403 ("Forbidden") | 인증 상태에 상관없이 액세스를 금지할 때 사용해야 한다 
404 ("Not Found") | 요청 URI에 해당하는 리소스가 없을 때 사용해야 한다 
405 ("Method Not Allowed") | HTTP 메서드가 지원되지 않을 때 사용해야 한다 
406 ("Not Acceptable") | 요청된 리소스 미디어 타입을 제공하지 못할 때 사용해야 한다
409 ("Conflict") | 리소스 상태에 위반되는 행위를 했을 때 사용해야 한다 
412 ("Precondition Failed") | 조건부 연산을 지원할 때 사용한다 
415 ("Unsupported Media Type") | 요청의 페이로드에 있는 미디어 타입이 처리되지 못했을 때 사용해야 한다 
500 ("Internal Server Error") | API가 잘못 작동할 때 사용해야 한다 
 
## 4장. 메타데이터 디자인

## 5장. 표현 디자인

## 6장. 클라이언트 영역

