## 데이터 조작언어 SELECT 문

### 📌 데이터의 정렬

: ORDER BY 사용 오름차순 :ASC, 내림차순 : DESC ➡️ 여러 개가 정렬 기준이 될 수 있음 ( , )로 나열

```sql

SELECT 팀명, 승률
FROM 팀랭킹
ORDER BY 승률 DESC

```

### 📌 특수연산자

| 종류    | 사용 예                  | 의미                                        |
| ------- | ------------------------ | ------------------------------------------- |
| BETWEEN | 컬럼명 BETWEEN V1 AND V2 | 컬럼값이 V1~V2 사이에 존재하는지 검사       |
| LIKE    | 컬럼명 LIKE '%V1%'       | V1이 문자열 내부에 존재하는지 검사          |
| IN      | 컬럼명 IN(V1,...,Vn)     | 컬럼값이 V1,...,Vn중 하나와 일치하는지 검사 |

```sql
-- 팀타율 0.3에서 0.4의 팀명,팀타율

SELECT 팀명, 팀타율
FROM KBOSTATS
WHERE 팀타율 BETWEEN 0.3 AND 0.4

-- 한화이글스, 엘지트윈스의 성이 문인 선수

SELECT 이름
FROM 선수
WHERE 이름 LIKE '문%' AND 팀명 IN ('한화이글스','엘지트윈스')

```

### 📌 집합 연산자

➡️ 집합 연산자 사용시 두 테이블이 같은 수의 열을 가져야 하며 도메인이 일치해야됨

- 합집합 : UNION, UNIONALL
- 교집합: INTERSECT
- 차집합: EXCEPT </br>

```sql
select 이름,G, GS,IP, E
from 한화이글스
where 선수명 = "3루수"
union
select 이름,G, GS,IP, E
from 삼성라이온즈
where POS = "3루수"
```

➡️ UNION(중복된 값 제거), UNIONALL(중복된 값 포함)

### 📌 숫자함수

| 함수         | 의미                                     |
| ------------ | ---------------------------------------- |
| ABS()        | 절대값 반환                              |
| SIN(), COS() | 삼각함수값 반환                          |
| CEILING()    | 주어진 값보다 크거나 같은 최소 정수 반환 |
| FLOOR()      | 주어진 값보다 작거나 같은 최대 정수 반환 |
| LN()         | 자연로그 값 반환                         |
| PI()         | 원주율(π) 값 반환                        |
| POWER()      | 지정된 수의 거듭제곱 값 반환             |
| RAND()       | 0과 1 사이의 난수 반환                   |
| ROUND()      | 지정된 소수점 자릿수로 반올림            |
| SQRT()       | 제곱근 반환                              |
| TRUNCATE()   | 지정된 소수점 자릿수까지 숫자 자름       |

### 📌 문자함수

| 함수          | 의미                       |
| ------------- | -------------------------- |
| CHAR_LENGTH() | 문자열의 문자 수 반환      |
| CONCAT()      | 두 개 이상의 문자열을 연결 |
| LOWER()       | 문자열을 소문자로 변환     |
| UPPER()       | 문자열을 대문자로 변환     |
| SUBSTRING()   | 문자열의 일부분 추출       |
| TRIM()        | 문자열 양쪽 공백 제거      |
| LTRIM()       | 문자열 왼쪽 공백 제거      |
| RTRIM()       | 문자열 오른쪽 공백 제거    |

```sql
-- 팀명: 대문자 승률: 소수점 3자리 승-패:하이픈 연결 승-패:절대값 차이 계산
SELECT
    팀명,
    UPPER(팀명) AS 대문자팀명,
    ROUND(승률, 3) AS 반올림승률,
    CONCAT(승, '-', 패) AS 승패기록,
    ABS(승 - 패) AS 승패차
FROM
    team_ranking
ORDER BY
    승률 DESC
```

### 📌 집계함수

: SELET 절 또는 HAVING절에 기술</br>

- COUNT : 컬럼에 있는 값들의 개수
- SUM : 컬럼에 있는 값들의 합
- AVG : 컬럼에 있는 값들의 평균
- MAX : 컬럼에서 가장 큰 값
- MIN : 컬럼에서 가장 작은 값

```sql

SELECT COUNT (선수id)
FROM 선수

```

### 📌 그룹질의

: 특정 기준으로 레코드를 그룹화하고 각 레코드 그룹에 대해 집계함수를 적용하는 질의</br>

- WHERE : 레코드에 대한 조건을 기술
- HAVING : 집계 결과 레코드에 대한 조건을 기술
  `SELECT 문 GROUP BY 컬럼 HAVING 조건`</br>
  ➡️ SELECT 절에 그룹의 기준과 집계 함수 이외의 컬럼은 포함 불가

```sql
SELECT * FROM 선수
GROUP BY 선수번호
HAVING ERA < 3.00
```
