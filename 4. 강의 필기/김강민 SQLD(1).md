> https://www.youtube.com/watch?v=PC3ypt_VGWI

# 연산순서

2 - 3 - 4 - 5 - 1 - 6
from - where - group by - having select - order by

# SQL 명령문 종류

1. DDL : CREATE, DROP, ALTER, MODIFY
2. DML : INSERT, UPDATE, SELECT, DELETE
3. TCL : COMMIT, ROLLBACK
4. DCL : GRANT REVOKE

# SELECT

- DISTINCT (집약) : 중복제거

```sql
DISTINCT (DEPTNO, MGR)
-- 은,
GROUP BY (DEPTNO,MGR)
-- 이랑 비슷하다.
```

# ALIAS

- SELECT 절에 쓰는경우
  - 생략가능 !
  - 컬럼명에 띄어쓰기( 직원 번호)
- FROM 절에 쓰는 경우
  - 생략불가능 ! (FROM AS ALIAS 사용불가)
  - AS사용 불가

# Concat 연산자

## concat() 함수

- 인수가 반드시 2개. (1개도 3개이상도 안됨)

## 기호

- : sql Server
- || : Oracle

# 논리연산자

- AND
- OR
- NOT
- 우선순위 : NAO (NOT AND OR)

# X BETWEEN A AND B, X IN (1,2,3)

- A <= X <= B
- X = 1 or X = 2 or X = 3

# LIKE

- \_ : 한글자
- % : 0이상의 글자
- \_L% : 두번째 글자가 L인 얘들 전부

## LIKE ESCAPE

- ename like 'A@\_A%' ESCAPE '@';
  : 앞 세글자가 A_A인 얘들 찾기

# ROWNUM / TOP

- rownum : oracle
- top : sql server

## rownum

- where 조건절에서, rownum = 1이면 포함
- rownum <= 10 // 10개 행을 가져옴.

```sql
select empno, sal
from emp
where rownum <= 3
order by sal;
```

**order by가 나중에 실행되기때문에, where에서 걸러진 놈들을 토대로 정렬 수행**

## top

select 절에서, top(n) <컬럼명>
특정 <컬럼명> 출력시, 상위 n개의 행을 가져옴.

# NULL \*\*\* (개많이나옴)

1. null의 정의

- 값의 부재, 모르는 값

2. null 연산

- 산술연산

  - null + 2 = null
  - null - 4 = null
  - null x null = null

- 비교연산

  - null = null = 알 수없음 (unknown)
  - null = 2 = 알 수없음 (unknown)
    -> where(<조건>)
    <조건>이 unknown인 경우, "False"

3. null의 정렬상 의미

- 오라클에서는 '무한대'로 해석. (오름차순하면 null 값 row가 나중에 등장)
- SQL Server 에서는 '음의(-) 무한대'로 해석. (오름차순하면 null 값 row가 처음에 등장)

4. null 함수

- 널 뛰기

  - NVL(값1, 값2) : 값1이 null이면 값2 리턴, 아니라면 값1 리턴
  - NVL2(값1, 값2, 값3 ...) : 값1이 null이면(isnull) 값3 리턴, 아니라면(is not null) 값2 리턴
  - isNull(값1, 값2) : 값1이 null이면 값2 리턴, 아니라면 값1 리턴 / NVL과 동일

- 같이 놀자!

  - NullIf(값1, 값2) : 두 값이 같으면 null, 다르면 값1

- 널 아닌 첫번째 값.
  - Coalesce(값1, 값2, 값3...) : null 아닌 첫번째 값을 리턴 (값3 앞으로 다 null이면 값3 리턴)

# 정렬 (order by)

## 정렬의 특성

- 가장 마지막에 실행.
- 성능이 느려질 가능성
- null 값 과의 관계 (오라클에선 무한대, sql server에서는 음의 무한대)

## 컬럼 번호 정렬

- 출력되는 컬럼의 수 보다 큰 값은 허용하지 않는다.

```sql
select deptno, ename
from table
order by 1 desc, 2 asc, 3 desc; -- 3 desc는 불가능 (2번이 최대)
```

## 인수 두개 정렬

- sal desc, ename asc
  : sal이 같으면, ename 오름차순.

## 출력되지 않는 컬럼으로도 정렬 가능

- 아래 문장이 가능함 ! (select절에서 sal이 없어도 sal 순으로 정렬가능)

```sql
select ename
from table
order by sal;
```

# 숫자함수

- round 자릿수 주의할 것.

  - round(138.94) = 139
  - round(138.94, 1) = 138.9
  - round(138.94, 2) = 138.94

- ceil / ceiling
  ceil 은 오라클, ceiling은 sql server

# 문자열 함수

- upper : 대문자로
- lower : 소문자로
- Lpad : LPAD(123,5,'0') = 00123 / 특정문자를 왼쪽에 채워서 문자열 길이를 맞출 때
- Rpad : RPAD(123,5,'0') = 12300 / 특정문자를 오른쪽에 채워서 문자열 길이를 맞출 때
- trim : trim(' NEW YORK ') = 'NEW YORK' / 문자열 양쪽 공백 제거
- Ltrim : ltrim(' NEW YORK ') = 'NEW YORK ' / 문자열 왼쪽 공백 제거 /
  - 두번째 인자로 option 문자열을 넣어주면, 왼쪽 끝의 반복적인 문자 제거
  - ltrim('AAABBCCC','A') = BBCCC
- Rtrim : rtrim(' NEW YORK ') = ' NEW YORK' / 문자열 오른쪽 공백 제거
  - 두번째 인자로 option 문자열을 넣어주면, 오른쪽 끝의 반복적인 문자 제거
  - rtrim('AAABBCCC','C') = AAABB
- subStr : substr("문자열","시작위치","길이")
  - 시작위치는 인덱스가 아님. (1부터 시작)
  - substr("JEONINHYEOK",3,3) = ONI
- instr : INSTR 함수는 문자열에서 문자를 찾으면 문자의 시작 위치를 반환한다. 문자를 찾지 못하면 "0"을 반환한다.
  - instr([문자열], [찾을 문자값], [찾기 시작할 위치], [찾은 결과의 순번])
  ```sql
  SELECT INSTR('Oracle Database', 'Database') AS result1 -- 8
     , INSTR('Oracle Database', 'Server')   AS result2  -- 0
  FROM dual
  ```
  > https://youtu.be/PC3ypt_VGWI?t=1855

# 날짜함수

날짜 데이터의 형변환을 일으키는 함수

- to_char : TO_CHAR(SYSDATE, 'YYYYMMDD'), TO_CHAR(SYSDATE, 'YYYY/MM/DD')과 같이 사용
- 위와 같이 사용하면 20200723, 2020
- to_date : TO_DATE('20160901151212','YYYYMMDDHH24MISS'), TO_DATE('2016','YYYY')

  - 위와 같이 사용하면 2016-09-01 오후3:12:12와 같은 날짜로 표기됨.

- sysdate : 오라클에서 현재시간 출력해주는 함수
- getdate : sql server에서 현재시간 출력해주는 함수

- 날짜데이터 + 100 하면 => 100일 이후로 인식. (날짜연산은 기본적으로 day로 인식한다.)

# DECODE / CASE

## DECODE

```sql
DECODE(gender, "M", "남자", "F", "여자", "기타")
```

gender가 M이면 남자, F면 여자, 둘다 아니면 기타

- 마지막 파라미터(else) 부분은 생략 가능. 해당 조건이 없으면 null 처리

## CASE WHEN

```sql
  CASE
    WHEN THEN -- 조건1
    WHEN THEN -- 조건2
  ELSE
  END
```

- ELSE가 있으면, 모든 조건 (조건1,조건2) 만족하지 않으면 else에 있는거 출력
- ELSE 가 없는 경우, 모든 조건(조건1, 조건2) 만족하지 않으면 null 출력

# 집계 함수 (매우 중요)

## null과의 관계

- sum() : null 값을 제외하고 더하기.
- count(컬럼) : null 값을 제외한 컬럼 개수를 센다.
- count(\*) : null에 상관없이 전체 컬럼 개수를 센다.

```
  A    B     C    A+B+C
null  null   1     null
3      2     2      7
null   2     3     null

에서, sum(A) = null / sum(B) = 7 / sum(C) = null
vs
sum(A+B+C)

의 차이 기억해놓기.
```

# GROUP BY (집약기능)

- WHERE 다음에 실행.
- 그룹수준의 정보를 바꾼다.

> https://youtu.be/PC3ypt_VGWI?t=2211

# natural join / using

- 중복된 컬럼이 하나로 출력됨.
- 중복되 컬럼이 제일 앞에 등장함.

## natural join

- ALIAS 사용가능 !

## using

- alias 사용 불가능

# left outer join

- A left outer join

  - left 조인이면 오른쪽에 (+) 붙인다.
  - 오른쪽이 뚱뚱해진다.
    = A col 1 = b col1 (+)
  -

- A right outer join

  - right 조인이면 왼쪽에 (+) 붙인다.
  - 왼쪽이 뚱뚱해진다.
    = A col 1 (+) = b col 1

## 조인순서

From A, B, C에서,
AB 먼저 조인 후 그 결과(하나의 테이블)와 C를 다시 조인

# 서브쿼리

[절별 서브쿼리 들어감 여부]
SELECT - 단일행 서브쿼리 중 하나인 Scalar 들어감
FROM - Inline View 들어감 (메인쿼리의 컬럼사용가능 !)
WHERE - 거의 모든 서브쿼리가 다들어감 (중첩서브쿼리)
GROUPBY - 서브쿼리 안들어감
HAVING - 거의 모든 서브쿼리가 다들어감 (중첩서브쿼리)
ORDER BY - Scalar 서브쿼리

## 스칼라 서브쿼리(Scalar Subquery)

- SELECT 절에서 사용하는 쿼리
- 스칼라 서브쿼리는 항행,, 한 칼럼만 반환하는 서브쿼리를 말함.
- 컬럼을 사용할 수 있는 대부분의 곳에서 사용가능.
- 단일행 서브쿼리라서, 2개이상의 결과가 반환되면 오류발생

```sql
SELECT PLAYER_NAME 선수명, HEIGHT 키,
       ROUND( (SELECT AVG(HEIGHT)
                 FROM PLAYER X
                WHERE X.TEAM_ID = P.TEAM_ID), 3) 팀평균키
FROM PLAYER P
```

# 서브쿼리 예시

- 서브쿼리는, 한개의 메인테이블의 ROW를 볼 때,
  모든 ROW를 대상으로 서브쿼리를 다돈다.
  그렇게 A.COL1 = B.COL1 인경우를 다 찾아내는 것.

```sql
SELECT
FROM A
WHERE (
  SELECT
  FROM B
  WHERE A.COL1 = B.COL1
)
```

# IN, ANY/SOME, ALL, EXISTS

## IN

- 조건절에서 사용하고, 다수의 비교값과 비교하여 비교값 중 하나라도 같은 값이 있다면 TRUE

## ANY

- 다수의 비교값 중 한개라도 만족하면 TRUE
- IN과의 차이는 비교연산자를 사용한다는 점.

```SQL
SELECT * FROM WHERE SAL = ANY(950,3000,1250)
```

## SOME (ANY와 동일)

## ALL

- 전체 값을 비교하여 모두 만족하면 TRUE

```SQL
SELECT * FROM emp WHERE sal>ALL(950, 3000, 1250)
```

## EXISTS / NOT EXISTS

- 대부분 IN을 이용하는 것보다 EXISTS를 사용하는 것이 쿼리 성능면에서 장점을 가진다.

- EXISTS : 서브 쿼리가 적어도 하나의 행을 돌려주면 TRUE가 된다. ('1', 'X', 'A' 등 다 서브쿼리에서 가져올 수 있는데, 그냥 있기만하면 TRUE)
- NOT EXISTS : 서브 쿼리가 적어도 하나의 행을 돌려주지 않으면 TRUE가 된다.

# 집합 연산자

## UNION

- 정렬작업 있음, 조금 느리다

## INTERSECT

- 정렬작업 있음, 조금 느리다

## MINUS (EXCEPT)

- 정렬작업 있음, 조금 느리다

## UNION ALL

- 중복 데이터 존재, 정렬작업 없음, 빠르다

## UNION VS UNION ALL

UNION ALL 이 빠르다.

# DDL

DROP ALTER CREATE TRUNCATE

## TRUNCATE vs DROP

DROP는 건물 철거 (구조 자체를 없애서 모두 없애버림)
TRUNCATE는 입주민 퇴거 (구조는 남아있고 레코드만 삭제)

## TRUNCATE vs DELETE

- DDL vs DML
- TRUNCATE는 롤백 불가능
- DELETE는 롤백 가능

# DML

- INSERT UPDATE DELETE SELECT MERGE
- INSERT UPDATE DELETE는 대부분 TCL(COMMIT ROLLBACK과 연관지어서 나옴)
- MERGE(신유형)

# 제약조건 \*\*\*

PK : UNIQUE + NOT NULL / 대표성을 띄기 때문에 하나만 존재함
UNIQUE
NOT NULL

# DCL

- GRANT REVOKE
- ROLE 특징
  - 명령어는 아니고 오브젝트인데,

```SQL
-- WITH GRANT OPTION
GRANT SELECT ON A.T_TABLE TO B WITH GRANT OPTION;
```

```SQL
REVOKE SELECT ON my_table
FROM jsm CASCADE;
-- CASCADE : jsm이 권한을 준 모든 대상을 가리킴
-- 즉, jsm의 권한과 연결된 대상의 권한을 전부 취소
```

# VIEW

- 독편보
- 독립성 : 기존 테이블 구조가 변경되어도 뷰를 사용하는 응용 프로그램은 변경하지 않아도 된다.
- 편리성 : 복잡한 질의를 뷰로 만들면, 관련 질의를 단순하게 작성할 수 있다.
- 보안성 : 숨기고 싶은 데이터가 존재한다면, 뷰 생성 시 해 당 컬럼을 빼고 만들어서 보안성 확보

```SQL
CREATE VIEW AS SELECT문
```

# 그룹함수

- ROLL UP : ROLLUP (A,B) != ROLLUP(B,A)
- CUBE : CUBE(A,B) = CUBE(B,A)
- GROUPINGSETS :
- GROUPING

## 문제 예시

표를 주고 ROLLUP CUBE GROUPINGSETS 인지 판단하는 문제

1. NULL 다 찾기
2. 총합행이 있는지 찾기
   - X : GROUPINGSETS
   - O : ROLLUP, CUBE (양쪽으로 결과가 나오면 CUBE(행의 수가 많아보여요), 한쪽으로만 결과가 나오면 ROLLUP ()(행의수가 적어보여요))

# TCL

- COMMIT ROLLBACK

- 오라클은 AUTO COMMIT OFF가 기본값. (SQL SERVER는 ON)
- DDL 커밋기능 없애는 명령어 (SQL SERVER)

```SQL
AUTO CUMMIT OFF
AND BEGIN TRANSACTION
```
