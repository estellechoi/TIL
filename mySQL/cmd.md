## Command 로 MySQL 데이터베이스 백업/복원하기

 #### 데이터베이스 백업하기
 - 백업 명령어 : mysqldump
 ```
  mysqldump -uroot -p1234 database > filename.sql

 - MySQL 포트가 여러개이고 특정 테이블만 백업하는 경우
  mysqldump -uroot -p1234 --port=3307 database table > filename.sql
 ```

 - PATH 설정되어있는 경우 : 바로 명령어 실행
 - PATH 설정되어있지 않은 경우 : 백업실행파일 위치한 디렉토리로 이동
 ```
  - 백업실행파일(mysqldump.exe) 위치한 디렉토리로 이동
   cd C:\Program Files\MySQL\MySQL Server 5.7\bin

  - 명령어 사용 예시
  - mall 데이터베이스의 product 테이블 백업 (test.sql 파일로 저장)
   mysqldump -uroot -p1234 --port=3307 mall product > d:/test.sql

  - D드라이브로 이동 (백업 성공여부를 확인하자 !)
   d:

  - 전체 목록 조회
   dir

  - test.sql 파일 조회
   dir test.sql
 ```

 #### 데이터베이스 복원하기 (가져오기)
 - 복원 명령어 : mysql
 ```
  mysql -uroot -p1234 --port=3307 database < table.sql
```
