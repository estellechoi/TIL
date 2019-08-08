## MySQL 쿼리 문법

### Notes
* mysql -u 아이디 -p 비번
* bin (실행파일 디렉토리)

---

## SQL Structure
* MySQL > DB > my DB > Table > Field > Record (values)
* 작업의 종류  : insert(입력), update(수정), delete(삭제), select(조회)


## Datatype(자료형)
* int
* char(문자수)
* text
* ...varchar -> 입력된 문자수 만큼만 메모리 차지

---

## MySQL Console(root) command
> Enter password: apmsetup


### Basic commands
* ; or \g → send cmd to mysql.
* help or \h → help
* \c → clear the input statement.
* \q → quit (sql 종료)


### Commands

> "db"/"table"/"field" 는 관리자가 입력하는 DB/Table/Field의 이름

* status; DB서버 상태 조회
* grant all privileges on *.* to user@localhost identified by '1234' with grant option; DB 사용자 추가
* use sql; select * from user; 사용자 권한 조회

* show databases; DB 현황 조회
* create database "db"; DB 생성
* drop database "db"; DB 삭제
* use "db"; 해당 DB에서 작업하겠다 선언

* show tables; table 현황 조회
* create table "table"("field" "datatype"(text limit),"field" "datatype"(text limit)); table 생성

       [ex] create table member(name char(10), address char(20), age int);


       ☆ 각 레코드에 고유번호 부여하기
          sql-> create table "table"(id int auto_increment primary key);
       ☆ null 값 허용하지 않기 (회원가입 등에서 사용)
          sql-> create table "table"(name char(10) not null);

       [ex] create table member(id int auto_increment primary key, name char(10) not null, age int not null);

* drop table "table"; 테이블 삭제
* desc "table"; 해당 테이블 구성 보기 (description)
* alter table "table" add column "field" "datatype"; 테이블 필드 추가

       [ex] alter table member add column phone char;

* alter table "table" change "ex-field" "field" "datatype"; 테이블 필드 수정
* create table "table"(select * from "ex-table"); 테이블 복사 (ex-table → table)

##### 레코드 관련
* select * from "table"; 해당 테이블의 모든 레코드 조회 (* means all)
* select "field", "field" from "table"; 선택된 필드의 레코드만 조회
* select * from "table" where "field"="record"; 조건에 해당하는 레코드만 조회
* select * from "table" where "field"="record" and "field"="record"; 2 개 조건을 충족하는 레코드만 조회
* select * from "table" order by "field 1" desc, "field 2" asc; 필드1 내림차순 조회, 필드1 값이 같은 경우 필드2 오름차순

       [ex] select*from member where name="Estelle";
       [ex] select*from member where id=3;
       [ex] select*from member order by age desc; 나이순 조회
       [ex] select*from member where id%2; id가 짝수인 레코드만 조회
       [ex] select*from member where name like "%a%"; name 값에 a 가 포함되는 레코드만 조회
       [ex] select*from member where name like "a%"; name 값이 a로 시작하는 레코드만 조회
       [ex] select*from member where name like "%a%" and age>25;

* insert into "table"("field", "field") values("문자", 숫자); 레코드 추가
* update "table" set "field"='new record', "field"='new record' where "field"='record'; 해당 조건 하에서 필드의 레코드 수정

       [ex] update member set name='Estelle Choi', address='Seoul' where name='Estelle';
       [ex] update member set age=age+1; member 테이블 age 필드의 모든 레코드 값을 1씩 증가
       [ex] update member set city=city+" revised";

* delete from "table"; 테이블 레코드 전체 삭제
* delete from "table" where "field"='record'; 조건에 해당하는 레코드만 삭제

##### 인덱스와 뷰
* create index "in_field" on "table"("field"); 인덱스 생성
* create view "view_table"("field", "field") as select "field", "field" from "table"; 뷰 생성
* drop view "view_table"; 뷰 삭제
