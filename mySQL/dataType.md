## MySQL의 자료형
### 1. MySQL 에서 제공하는 자료형 (Data Type)
 - MySQL 에서 제공하는 기본 자료형은 아래와 같다.
    - 숫자
    - 문자열
    - 날짜 및 시간
 - 테이블 생성시 필드의 자료형을 명시한다.

 ```
    create table test(name char(30), age int(3));
 ```

### 2. 숫자 타입 (Numeric Types)
 - 정수형 `int`
 - 고정 소수점 `decimal(M, D)`
    - M : 소수 부분을 포함한 총 자릿수, D : 소수 부분 자릿수

### 3. 문자열 타입
 - 고정길이 문자열 `char(M)`
    - M : 0 ~ 255
    - 설정한 크기보다 작은 길이의 문자열이 입력되면 나머지 공간을 여백으로 채워 모두 사용한다.
 - 가변길이 문자열 `varchar(M)`
    - M : 0 ~ 65,535
    - 실제 입력된 문자열의 길이만큼만 공간을 사용한다.

### 4. 날짜 타입
 - 날짜 `date`
    - (\'YYYY\-MM\-DD\')
 - 날짜와 시간 `datetime`
    - (\'YYYY\-MM\-DD HH\:MM\:SS\')
 - 타임스탬프 `timestamp`
    - (\'YYYY\-MM\-DD HH\:MM\:SS\')
    - 사용자가 별다른 입력갑을 주지 않으면, 데이터가 마지막으로 변경된 시간을 저장한다.
    - 데이터의 최종수정시각을 저장하고 확인하는데 사용된다.
 - 시간 `time`
    - ('HH:MM:SS')
 - 연도 `year`
    - 두자리 연도 year(2) 또는 네자리 연도 year(4) 를 사용할 수 있다.
    - 유효하지 않은 연도는 \'0000\' 으로 저장된다.


 * 참고링크 : [TCP SCHOOL.com](http://tcpschool.com/mysql/mysql_datatype_numeric)
