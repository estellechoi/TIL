## Command 로 MySQL 접근하기
 #### sql 파일 가져오기
 - mysql -uroot -p1234 --port=3307 database < table.sql


 #### 데이터베이스 백업하기
 - mysqldump -uroot -p1234 --port=3307 backupDB table > 저장파일명
 - PATH 설정 안돼있는 경우 : 백업 실행파일(mysqldump.exe) 위치한 디렉토리로 이동
 ```MySQL
   cd C:\Program Files\MySQL\MySQL Server 5.7\bin

   mysqldump -uroot -p1234 --port=3307 mall_test product > d:/test.sql

   d:

   dir test.sql
 ```

 
