https://docs.docker.com/get-started/

### images
* postgres
  * docker run --rm -it -p 5432:5432 -e POSTGRES_PASSWORD=pass -e POSTGRES_USER=yw-shin -e POSTGRES_DB=testdb --name postgres_test -d postgres
* mysql
  * docker run --rm -it -p 3306:3306 -e MYSQL_ROOT_PASSWORD=pass -e MYSQL_DATABASE=testdb -e MYSQL_USER=yw-shin -e MYSQL_PASSWORD=pass --name mysql_test -d mysql

