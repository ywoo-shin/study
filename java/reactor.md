## 1. Introduction to Reactive Programming

## 2. Flux And Mono
### Flux
* It can emit 0 to n <T> elements (onNext event)
  * onComplete
  * onError
```java
Flux.fromIterable(getSomeLongList())
    .delayElements(Duration.ofMillis(100))
    .doOnNext(serviceA::someObserver)
    .map(d -> d * 2)
    .take(3)
    .onErrorResumeWith(errorHandler::fallback)
    .doAfterTerminate(serviceM::incrementTerminate)
    .subscribe(System.out::println);
```  
### Mono
* It can emit at most 1 <T> element
  * onComplete
  * onError
```java
Mono.just(1)
    .map(integer -> "foo" + integer)
    .or(Mono.delay(Duration.ofMillis(100)))
    .subscribe(System.out::println)
```  
  
## 3. StepVerifier
```java
StepVerifier.withVirtualTime(() -> Mono.delay(Duration.ofHours(3)))
            .expectSubscription()
            .expectNoEvent(Duration.ofHours(2))
            .thenAwait(Duration.ofHours(1))
            .expectNextCount(1)
            .expectComplete()
            .verify();
```

## 4. Transform
* flux.flatMap()
* flux.flatMapSequential()
* flux.concatMap() 		

## 5. Merge
* mergeWith()
* concatWith(): if we want to keep the order of sources
* Flux.concat(Mono1, Mono2)  

## 6. Request
### backpressure (volume control)
* Subscriber가 Publisher에게 처리 가능한 data의 양을 알려줌으로써 Publisher가 data를 생성하는 속도를 제한하는 feedback mechanism.


