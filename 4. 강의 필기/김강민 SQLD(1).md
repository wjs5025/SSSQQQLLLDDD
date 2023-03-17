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
