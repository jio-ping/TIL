<details> 
<summary >
📕  용어 정의</summary>

<b>스키마(=database)</b>: 한 조직의 데이터 베이스 시스템에 운영에 필요한 테이블, 인덱스, 뷰 등의 데이터 베이스 객체 집합 </br></br>

<b>📍릴레이션</b>: 데이터를 구성하는 추상적인 개념. 집합론에 기반한 여러 속성을 가진 레코드의 집합으로 레코드의 순서가 없으며 중복된 레코드가 존재하지 않음 </br></br>

<b>📍테이블</b>:물리적으로 데이터를 저장하는 구체적인 개념. 레코드의 순서가 존재 가능하며 키 제약에 따라 중복된 레코드가 존재함 </br></br>

</details>

## SQL

: Structured Query Language(SQL)은 관계 대수에 기초해 RDBMS의 데이터관리를 위해 1970년대 초 IBM에서 설계

### 📌 특징

1. 비절차적(선언형)언어,필요한 데이터만 기술
2. 인간의 언어와 매우 유사하고 간단, 명료

### 📌 구성

- <b>데이터 정의 언어(DDL:Data Definition Language)</b></br>
  : 데이터베이스 내의 객체를 생성 및 삭제하고 그 구조를 조작하는 명령어의 집합. 데이터가 준수해야하는 제약조건을 기술</br></br>
- <b>데이터 조작 언어(DML:Data Manipulation Language)</b></br>
  : DDL에 의해 정의된 테이블에 데이터를 조작하는 명령어의 집합. 데이터에 대한 CRUD(생성,검색,삭제,수정) 명령을 포함
- <b>데이터 제어 언어(Data Control Language)</b></br>
  : DBMS의 동작, 접근 권한 등을 관리하는 SQL 명령어의 집합

## 데이터 정의 언어

: 데이터베이스 객체를 생성, 삭제 또는 구조를 수정하는 명령어의 집합

cf) <b>데이터 베이스의 객체</b>

- 데이터 저장:스키마(데이터베이스),테이블,인덱스,뷰
- 데이터 조작:트리거, 프로시저, 함수 등

### 📌 데이터 정의 명령어의 종류

#### 📎 CREATE : 객체 생성

- 스키마/ 데이터베이스 생성

```sql
CREATE SCHEMA 스키마이름
CREATE DATABASE 데이터 베이스 이름
```

| id             | 이름 | 직업   | 체온 |
| -------------- | ---- | ------ | ---- |
| 123456-123456  | 정원 | 바리톤 | 34.5 |
| 123456-1234557 | 꽁이 | 여행가 | 38   |

- 테이블 정의

```sql
CREATE TABLE 체온(
  id CHAR(13),
  이름 CHAR(30),
  이름 CHAR(10),
  체온 INT)
```

#### 📎 ALTER : 객체 수정

: CREATE 문에 의해 생성된 테이블에 컬럼을 추가, 수정(이름, 데이터 타입, 제약조건) 또는 삭제</br>
➡️ 컬럼 삭제 또는 컬럼의 데이터 타입 수정 시 데이터에 대한 소실이 발생하므로 많은 주의가 요구

- 체온 테이블에서 '직업' 컬럼 삭제

```sql
ALTER TABLE 체온 DROP COLUMN 직업
```

- 유저 테이블의 유저이름 컬럼을 VARCHAR(100)으로 변경

```sql
ALTER TABLE Users
MODIFY COLUMN Username VARCHAR(100);

```

#### 📎 DROP : 객체 삭제

```sql
DROP TABLE 체온
```

### 📌 데이터 타입의 개념

: 컬럼이 가질 수 있는 값의 범위, 즉 도메인을 결정

#### 📎 문자 데이터 타입

- CHAR(n): 최대길이가 n인 <b>고정길이</b>문자열</br></br>
- VARCHAR(n) : 최대길이가 n인 <b>가변길이</b>문자열</br></br>
- TEXT: 길이가 최대 2~4GB인 가변길이 문자열</br></br>
- CLOB(Character Large Object): 수백 MB, 수 GB의 데이터 저장을 위한 타입, 레코드 단위가 아닌 별도의 저장 공간을 부여하는 외부 저장 방식</br></br>
- ENUM : 유한개의 문자열 집합 중 하나의 값을 선택</br>
  ➡️ 효율적인 저장 및 처리를 위해 내부적으로 숫자로 저장 </br>
  ex. 혈액형: ENUM('A','B','O','AB')

<details>
<summary>고정 길이 vs 가변 길이</summary>
<img  width="400px" src="https://github.com/user-attachments/assets/fa5ac855-2459-4351-be08-7e7061bd1d36"></br>

➡️ 글자의 수가 일정하고 길이 변화가 크지 않으면 CHAR, 레코드 마다 글자 길이에 큰 편차가 있거나, 변화의 빈도가 크면 VARCHAR

</details>

#### 📎 숫자 데이터 타입

- 정수
  - TINYINT : 1바이트 정수(-128~127)
  - SMALLINT : 2바이트 정수(-32768~32767)
  - INT : 4바이트 정수 (약 -20억 ~ 20억)
  - BIGINT : 8바이트 정수(약 -9000경 ~ 9000경)
- 실수
  - 부동 소수형
    - FLOAT : 4바이트 크기 부동소수
    - FLOAT(P) : 소수점 이하 P개 자리 부동소수
    - DOUBLE : 8바이트 크기 부동소수
  - 고정 소수형
    - DECIMAL(M,N) : 전체 M자리, 소수접 이해 N 자리의 소수를 저장
    - NUMERIC : DECIMAL과 유사

#### 📎 날짜/시간 데이터 타입

- 날짜 데이터 타입
  - DATE : 'YYYY-MM-DD' 형식의 시간
  - YEAR: 'YYYY' 형식의 연도
- 시간 데이터 타입
  - TIME : 'HH:MI:SS' 형식의 시간
- 날짜 및 시간 데이터 타입
  - DATETIME : 'YYYY-MM-DD HH:MI:SS' 형식의 날짜 및 시간
  - TIMESTAMP : DATETIME과 유사 유닉스 시간 기반 1970~2038 년 표현 가능

### 📌 제약조건

:테이블에 존재하는 데이터를 무겨라고 세밀하게 관리하기 위한 목적으로 사용</br>

- PRIMARY KEY : 기본키 지정(UNIQUE + NOT NULL)
- FOREIGN KEY : 외래키 지정, 참조 컬럼 정의
- NOT NULL : NULL이 될 수 없는 컬럼에 지정
- UNIQUE : 동일한 컬럼 값을 가질 수 없음르 지정
- AUTO_INCREMENT: 레코드가 추가될 때 자동적으로 속성값이 1부터 1씩 증가되어 입력
- CHECK : 컬럼값이 특성 조건 준수 여부 지정

| StudentID | Name   | Email                   | Age | EnrollmentDate | DepartmentID |
| --------- | ------ | ----------------------- | --- | -------------- | ------------ |
| 1         | 구자욱 | moonlight@samsung.com   | 32  | 2025-03-20     | 101          |
| 2         | 노시환 | sihwantastic@hanhwa.com | 24  | 2025-03-19     | 102          |

```sql
CREATE TABLE Students (
    StudentID INT PRIMARY KEY,
    Name VARCHAR(50) NOT NULL,
    Email VARCHAR(100) UNIQUE,
    Age INT CHECK (Age >= 18),
    EnrollmentDate DATE DEFAULT CURRENT_DATE,
    DepartmentID INT,
    FOREIGN KEY (DepartmentID) REFERENCES Departments(DepartmentID)
);

```
