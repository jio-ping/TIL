### 📌 중첩질의

: SELECT 문 내부에서 독립적으로 실행 가능한 또 다른 SELECT 문이 내포되어 있는 질의

- FROM 절

```sql
SELECT 컬럼1, 컬럼2, ..., 컬럼 n
FROM (SELECT 컬럼1, 컬럼2, ..., 컬럼 m
            FROM 테이블
                WHERE 조건  )
WHERE 조건
```

- WHERE 절

```sql
SELECT 컬럼1, 컬럼2, ..., 컬럼 n
FROM 테이블
WHERE 컬럼 i 연산자
        (SELECT 컬럼j
            FROM 테이블
                WHERE 조건  )
```

<details>
<summary>example-from절</summary>
학과별 교수의 평균 연봉이 70,000,000미만인 학과 중 가장 높은 평균 연봉을 출력

1. 학과별 평균

```sql
SELECT 소속학과, AVG(연봉) AS 평균연봉
FROM 교수
GROUP BY 소속학과
```

2. 학과별 평균이 70,000,000 미만인 학과 중 가장 높은 평균 연봉

```sql
SELECT MAX(d.평균연봉) AS 평균연봉
  FROM (SELECT 소속학과, AVG(연봉) AS 평균연봉
        FROM 교수
        GROUP BY 소속학과)AS d
  WHERE d.평균연봉 <7000000
```

</details>

<details>
<summary>example - where절</summary>
한화이글스 선수 중 라인업에 등록 되지 않은 선수 번호를 출력</br>

1. 한화 선수 번호

```sql
SELECT 선수번호
    FROM KBO
    WHERE 팀 ="한화이글스"
```

2. 등록되지 않은 선수의 번호 출력

```sql
SELECT A.선수번호
  FROM KBO AS A
  WHERE A.팀="한화이글스" AND
    NOT EXISTS (SELECT B.선수번호 FROM 라인업 B
                  WHERE A.선수번호 = B.선수번호)
```

</details>

### 📌 조인질의

: 테이블 간의 관련성을 이용해 두 개 이상의 테이블에서 데이터를 검색하는 질의 방법
</br> 조인조건은 WHERE절이 아닌 ON절에 기록

#### 📎 내부조인

: 두 개 이상의 테이블에서 <b>조인 조건을 만족하는 레코드만</b> 결합해 출력 결과에 포함</br>
➡️ 조인 결과에 정보의 손실이 발생

```sql
SELECT 컬럼1, 컬럼2, 컬럼,...
  FROM 테이블 1, INNER JOIN 테이블2,
  ON 조인조건
  [WHERE 조건]
```

<details>
<summary>exmple - 내부조인</summary>
철학과 소속 교수가 강의하는 과목에 대해 과목별 수강하는 학생수를 과목코드와 함께 출력

1. 교수와 과목테이블을 조인

```sql
SELECT *
  FROM 교수 INNER JOIN 과목
      ON 교수.교수번호 = 과목.교수번호
```

2. 교수와 과목테이블을 조인한 테이블에 수강테이블까지 조인

```sql
SELECT *
  FROM 교수 INNER JOIN 과목
    ON 교수.교수번호 = 과목.교수번호
      INNER JOIN 수강
        ON 과목.과목코드 = 수강.과목코드
```

</details>

#### 📎 자연조인

: 두 테이블에 동일한 이름의 커럼에 대해 값이 같은 레코드를 결합하는 내부 조인 </br>
➡️ 조인조건을 별도로 기술하지 않아도됨

```sql
SELECT 컬럼1, 컬럼2, ..., 컬럼n
  FROM 테이블 1 NATURAL JOIN 테이블 2
   [WHERE 조건]
```

#### 📎 외부조인

:조인조건에 맞지 않는 레코드도 질의의 결과로 포함시키는 질의

- 왼쪽 외부 조인(left outer join)</br>
  : 조건 만족하는 것은 출력, 조건만족하지 않은 왼쪽 테이블 레코드도 출력
- 오른쪽 외부 조인(right outer join)</br>
  : 조건 만족하는 것은 출력, 조건만족하지 않은 오른쪽 테이블 레코드도 출력
- 완전 외부 조인(full outer join)</br>
  : 왼쪽, 오른쪽 테이블 모두 조건에 만족하지 않아도 출력

```sql
SELECT 별칭1.컬럼1, 별칭2.컬럼1
FROM 테이블 1 AS 별칭
  LEFT | RIGHT [OUTER] JOIN
    테이블 2 AS 별칭
  ON 별칭1.컬럼i = 별칭2.컬럼j
  [WHERE 절]
```

<details>
<summary>
example - 외부조인</summary>

학생의 학생번호, 학생 이름과 그 학생이 수강신청한 과목의 과목코드와 신청시각을 출력하시오. 단, 수강신청을 하지 않은 학생도 결과에 포함시키고 과목코드를 기준으로 오름차순 정렬</br>
➡️ 학생테이블을 왼쪽외부조인

```sql
SELECT A.학생번호, A.학생이름, B.과목코드, B.신청시각
FROM 학생 AS A LEFT OUTER JOIN 수강 AS B
  ON A.학생번호 = B.학생번호
  ORDER BY B.과목코드 ASC
```

</details>

#### 📎 셀프조인

: 한 테이블이 자가 자신과 조인되는 질의.
➡️ 동일테이블에 대한 조인이므로 별칭의무적

```sql
SELECT 별칭1.컬럼1,별칭2.컬럼2
FROM 테이블1 AS 별칭1
  INNER | OUTER JOIN 테이블2 AS 별칭2
ON 조인조건
[WHERE 절]
```

<details>

<summary>example - 셀프조인</summary>
과목의 과목코드, 과목명 그리고 그 과목의 선수과목의 과목코드, 과목명을 모두 출력하시오. 단, 선수과목이 없는 과목도 결과에 포함

```sql
SELECT B.과목명, B.과목코드, A 과목명 AS 선수과목명, A.과목코드 AS 선수과목코드
FROM 과목 AS RIGHT OUTER JOIN 과목 AS B
ON A.과목코드 = B.선수과목코드
```

</details>

### 📌 뷰

: 데이터를 저장하고 있는 하나 이상의 테이블을 유도하여 생성하는 가상 테이블

#### 📎 뷰 사용시 이점

- 데이터 독립성 </br>
  : 원본테이블의 구조가 바뀌어도 뷰를 이용한 작업은 정의만 변경되어 응용프로그램에 영향이 없음
- 데이터 보안</br>
  : 사용자에게 원본테이블의 일부 컬럼에 대한 접근을 허용하여 보안효과를 향상
- 다양한 구조의 테이블 사용</br>
  : 사용자 요구사항에 맞는 테이블의 구조를 제공
- 작업의 단순화 </br>
  : 복잡한 질의문을 뷰로 단순화
- 데이터 무결성</br>
  : WITH CHECK OPTION을 이용해 뷰 생성에 위배되는 수정 작업 거부

#### 📎 뷰의 생성

: 생성되는 뷰의 구조는 SELECT 문의 결과로 결정

```sql
CREATE VIEW 뷰이름 AS
    (SELECT * FROM 테이블 [WITH 조건])
  [WITH CHECK OPTION]
```

<details><summary>example - 뷰 생성</summary>
철학과 소속의 학생정보와 학과이름 및 이수학점을 출력하는 철학과_학생 뷰를 생성

```sql
CREATE VIEW 철학과_학생 AS
(SELECT 학생.*, 전공.학과이름, 전공.이수학점
  FROM 학생 NATURAL JOIN 전공
  WHERE 전공.학과이름 = '철학과')

```

</details>

#### 📎 뷰의 수정,삭제,검색

- 뷰의 수정: 생성과 동일하게 새로운 SELECT 문의 결과로 변경
- 뷰의 삭제: 일반적인 데이터베이스 객체 삭제와 동일
- 뷰를 이용한 검색: 가상의 테이블이므로 데이터 조작은 테이블 조작과 동일하게 수행

```sql
-- 뷰 수정
ALTER VIEW 뷰이름(컬럼1,컬럼2,...컬럼n) AS
  (SELECT *
    FROM 테이블 [WHERE 조건])

-- 뷰삭제
DROP VIEW 뷰이름

-- 뷰를 이용한 검색
SELECT 컬럼1,..,컬럼2 FROM 뷰이름
WHERE 조건 AND 뷰 정의조건
```

#### 📎 뷰를 이용한 데이터 삽입

: 뷰에 대한 INSERT 문은 원본 테이블에서 실행</br>
INSERT 문 실행이 불가능한 이유

1. PK, NOT NULL 제약사항이 위배되는 경우
2. 원본테이블에 존재하는 컬럼이지만 뷰에서는 없는 컬럼에 삽입하는 경우
3. WITH CHECK OPTION이 적용된 뷰는 위배되는 사항은 없지만 뷰에 맞지 않는 조건일 경우 실행 불가능
4. 조인질의 또는 그룹 질의가 적용된 뷰는 데이터 삽입 및 수정이 불가능
