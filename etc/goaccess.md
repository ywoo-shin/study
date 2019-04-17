### goaccess
https://goaccess.io/

#### 1. GoAccess 다운
```properties
// 필수 유틸 다운
# sudo yum install ncurses-devel GeoIP-devel

$ wget http://tar.goaccess.io/goaccess-1.2.tar.gz
$ tar -xzvf goaccess-1.2.tar.gz
$ cd goaccess-1.2/
$ ./configure --enable-utf8 --enable-geoip=legacy
$ make

# sudo make install 
```

#### 2. nginx 로그 분석을 위해 log format 수정
```properties
# sudo vi /usr/local/etc/goaccess.conf
...
// 아래 정보 추가
date-format %d/%b/%Y
time-format %H:%M:%S
log-format [%d:%t %^][%T][%h][%s][%^ - %^] "%r"
```

#### 3. 터미널로 웹 서버 접속 로그 분석
```properties
$ goaccess -f ~/logs/nginx/nginx_access.log
```

#### 4. URL로 html 웹 서버 접속 로그 분석
```properties
$ goaccess ~/logs/nginx/nginx_access.log -o ~/nginx-report.html --real-time-html &
```