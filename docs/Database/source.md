# MySQL 파일 실행하기 (source)

## MySQL 파일 실행하기
  -  MySQL 에 접속한다.
  ```
    mysql -h 127.0.0.1 -u estellechoi -p
  ```

  - source 명령어로 원하는 .sql 파일을 실행한다.
  ```
    source /Users/youjin/sql-downloads/test.sql
  ```

  - 해당 파일에 syntax error 가 없다면 `Query OK` 문구와 함께 정상적으로 실행된다. 
