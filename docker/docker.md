https://docs.docker.com/get-started/

### images
* postgres
  * docker run --rm -it -p 5432:5432 -e POSTGRES_PASSWORD=pass -e POSTGRES_USER=yw-shin -e POSTGRES_DB=testdb --name postgres_test -d postgres
* mysql
  * docker run --rm -it -p 3306:3306 -e MYSQL_ROOT_PASSWORD=pass -e MYSQL_DATABASE=testdb -e MYSQL_USER=yw-shin -e MYSQL_PASSWORD=pass --name mysql_test -d mysql


### Docker 명령어
https://subicura.com/2017/01/19/docker-guide-for-beginners-2.html

* -d 백그라운드 모등
* -p 포트연결 -p 3306:3306
* -e 환경변수 설정
* -rm 프로세스 종료시 컨테이너 자동 제거
* -it 터미널 입력
* -link 컨테이너 연결
* -name  컨테이너 이름 설정
* -v 호스트와 컨테이너 디렉토리 연결 (마운트)


``` smali
 docker run ubuntu:16.04
 docker run --rm -it ubuntu:16.04 /bin/bash
 docker run -d -p 1234:6379 redis
 docker run -d -p 3306:3306                      -e MYSQL_ALLOW_EMPTY_PASSWORD=true
                    --name mysql mysql:8.0
 docker run -d -p 8080:80
		   --link mysql:mysql
		   -e WORDPRESS_DB_HOST=mysql
		   -e WORDPRESS_DB_NAME=wp
		   -e WORDPRESS_DB_USER=wp
		   -e WORDPRESS_DB_PASSWORD=wp
		   wordpress
 docker run -d -p 8888:8888 -p 6006:6006 
                     teamlab/pydata-tensorflow:0.1


 docker ps -a
 docker stop ${container}
 docker rm ${container}
 docker rm -v $(docker ps -a -q -f status=existed)
 docker images
 docker pull ubuntu:16.04
 docker rmi ${image}

 docker logs ${container}
 docker logs -tail 10 ${container}
 docker logs -f ${container}

 docker exec -it mysql /bin/bash
 docker exec -it mysql mysql -uroot

```
