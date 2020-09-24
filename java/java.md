### 병렬화 적용
* parallelStream()의 return instance는 multi-thread에서 병렬로 실행할 수 있는 map()이나 filter()와 같은 method며 내부적으로 thread pool에 의해 관리된다.
* stream()? parallelStream()? 어느 것을 사용할까?
  * lambda 표현식을 통시에 실행하고 싶을 때
  * race condition없이 독립적으로 실행 가능한가?
  * forEach() method에 대한 호출과 lambda 표현식을 통한 출력을 병렬화 하는 것은 맞지 않다.
  * map(), filter()는 병렬화하기에 적절하다.
  * 병렬은 task가 시간 소모적이고, collection이 매우 큰 경우에만 빞을 본다.
