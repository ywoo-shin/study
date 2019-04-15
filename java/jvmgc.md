# JVM Flag

## `CMS` 컬렉터
```
-XX:+UseConcMarkSweepGC
-XX:+UseParNewGC -XX:+CMSParallelRemarkEnabled
-XX:+CMSConcurrentMTEnabled
-XX:CMSInitiatingOccupancyFraction=85 (default 68)
```

flag | 설명
--- | ---
UseConcMarkSweepGC | CMS 컬렉터 활성화
UseParNewGC | Minor GC에서 Parallel Collector 활성화
CMSParallelRemarkEnabled | (`UseParNewGC`와 같이 사용), Remark phase를 parallel 수행 
CMSConcurrentMTEnabled | 다중 GC thread로 parallel  작동하게 함
CMSInitiatingOccupancyFraction | CMS GC 주기를 언제 시작할지, (old generation이 85%가 차면... old gc 실행)

## `G1` 컬렉터 (jdk 8 이상)

tuning g1gc: https://www.youtube.com/watch?v=dx6V5HD14D0

http://www.oracle.com/technetwork/articles/java/g1gc-1984535.html

```
-XX:+UseG1GC 
-XX:ParallelGCThreads=n
-XX:ConcGCThreads=n
-XX:MaxGCPauseMillis=200(default)
-XX:InitiatingHeapOccupancyPercent=35 (default 45)
```

flag | 설명
--- | ---
UseG1GC | G1 gc 활성화
MaxGCPauseMillis | GC로 인한 최대 중단시간을 명시
InitiatingHeapOccupancyPercent | old heap 쪽 region을 언제부터 mark할 것인가.. (full gc 회피)
ParallelGCThreads | STW Worker thread 수를 지정 (사용 가능한 process core 수)
ConcGCThreads | Parallel marking thread의 수를 지정, ParallelGCThread에서 지정한 값의 1/4을 정의
UseTLAB | Thread local allocation buffer 활성화

## 공통
```
-Xms1g -Xmx1g  // heap size는 최대 2g로... (동일하게 설정하는 것을 권장함)
```

## GC log
```
-verbose:gc 
-Xloggc:logs/gc.$(date '+%Y%m%d_%H%M%S').log 
-XX:+PrintGCDetails 
-XX:+PrintGCTimeStamps 
-XX:+PrintGCDateStamps 
-XX:+PrintHeapAtGC 
-XX:+PrintClassHistogram 
-XX:+PrintGCApplicationStoppedTime 
-XX:+UseGCLogFileRotation -XX:NumberOfGCLogFiles=10 -XX:GCLogFileSize=10M
-XX:+PrintClassHistogramAfterFullGC -XX:+PrintClassHistogramBeforeFullGC
```

flag | 설명
--- | ---
PrintGCDetails | GC 수행 상세 정보를 출력
PrintGCTimeStamps | GC 발생 시간 정보를 출력
PrintGCDateStamps | `2018-01-01T22:24:52.770+0900` 식으로 gc 날짜 출력
PrintHeapAtGC | GC 발생 전후의 Heap에 대한 정보를 상세하게 기록
PrintClassHistogram | 클래스별로 점유하고 있는 메모리 히스토그램 출력
PrintGCApplicationStoppedTime | GC로 인해 Application이 정지된 시간 정보를 기록
UseGCLogFileRotation | gc 파일 갯수 분리 설정
NumberOfGCLogFiles | gc 파일 유지 갯수
GCLogFileSize | gc 파일 사이즈
PrintClassHistogramAfterFullGC | full gc 전 메모리 히스토그램 출력
PrintClassHistogramBeforeFullGC | full gc 후 메모리 히스토그램 출력

## Head dump
http://what-when-how.com/Tutorial/topic-684cn3k/Java-Performance-The-Definitive-Guide-233.html
```
-XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=logs
-XX:+HeapDumpBeforeFullGC -XX:+HeapDumpAfterFullGC
```
flag | 설명
--- | ---
HeapDumpOnOutOfMemoryError | OOM 시, heap dump (.prof) 자동 남김
HeapDumpPath | heap dump 경로
HeapDumpBeforeFullGC | full gc 전 heap dump
HeapDumpAfterFullGC | full gc 후 heap dump

# 유용한 도구
* GC 분석 도구
    * [IBM garbage analyzer](https://www.ibm.com/developerworks/community/groups/service/html/communityview?communityUuid=22d56091-3a7b-4497-b36e-634b51838e11) (java -Xms2g -Xmx2g -jar **.jar)
    * [HPJmeter](https://h20392.www2.hpe.com/portal/swdepot/displayProductInfo.do?productNumber=HPJMETERSW)  (java -Xms1g -Xmx1g -jar **.jar)
* 메모리 분석 도구 (OOME)
    * [Eclipse MAT](https://www.eclipse.org/mat/)
        * http://jagadesh4java.blogspot.com/2013/06/eclipse-memory-analyzer.html
* Heap 분석 도구
    * [IBM heap analyzer](https://www.ibm.com/developerworks/community/groups/service/html/communityview?communityUuid=4544bafe-c7a2-455f-9d43-eb866ea60091) (java -Xms2g -Xmx2g -jar **.jar)
* Load Test
    * [Gatling](https://gatling.io/) (**.scalar)
    * [Apache Jmeter](http://jmeter.apache.org/)  (swing)
    * [nGrinder](http://ngrinder.nhnent.com/) (for nhnent)
* JDK
    * jmap
    ```properties
    jmap -dump:format=b,file=heapdump1.hprof <pid>
    ```
    * jstak
    * gcutil