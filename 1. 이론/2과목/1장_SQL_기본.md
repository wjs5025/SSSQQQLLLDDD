# DB(데이터베이스)의 종류

- 관계형 DB / 계층형 DB / 네트워크형 DB

## 관계형 DB

- 릴레이션에 데이터를 저장하고 관리. / 릴레이션을 사용해서 집합 연산과 관계 연산 가능

## 계층형 DB

- 트리 형태의 자료구조에 데이터를 저장하고 관리 (1:M 관계를 표현)

## 네트워크형 DB

- 오너와 멤버 형태로 데이터를 저장 (1:M / N:M 표현 가능)

# 관계 연산 (선택, 투영, 결합, 나누기)

- 선택(SELECT) : 릴레이션 조건에 맞는 **행(튜플)**만을 조회
- 투영(PROJECT) : 릴레이션 조건에 맞는 **속성**만을 조회
- 결합(JOIN) : 여러 릴레이션의 공통된 속성으로 새로운 릴레이션 만들기
- 나누기(DIVIDE) : 기존 릴레이션에서 대상 릴레이션과 공통이 되는 행(튜플)을 제외하고 중복된 행도 제거

# 테이블 구조

- 기본 키 : 하나의 테이블에서 유일성과 최소성을 만족하면서 해당테이블을 대표하는 것.
- 행 : 하나의 테이블에 저장되는 값 (튜플, 레코드)
- 칼럼 : 어떤 데이터를 저장하기 위한 필드 (속성, 열, 컬럼)
- 외래키 : 다른 테이블의 기본 키를 참조하는 칼럼, 관계 연산 중 결합연산(JOIN)을 하기 위해 사용

# SQL의 종류

1. DDL (Data Definition Language) : 데이터 정의어 (CREATE, ALTER, DROP, RENAME) - DB의 구조를 정의
2. DML (Data Manipulation Language) : 데이터 조작어 (INSERT, UPDATE, DELETE, SELECT) - 테이블에서 데이터 입력, 수정, 삭제, 조회
3. DCL (Data Control Language) : 데이터 제어어 (GRANT, REVOKE) - DB 사용자에게 권한을 부여하거나 회수
4. TCL (Transaction Control Language) : 트랜잭션 제어어 (COMMIT, ROLLBACK) - 트랜잭션 제어.

> 트랜잭션 : DB의 작업 처리 단위

# 트랜잭션의 특성

- 원자성(Automicity) : 트랜잭션은 DB가 연산의 전부 또는 일부(All or Nothing) 실행 만이 있다.
- 일관성(Consistency) : 트랜잭션 실행 결과로 DB의 상태가 모순되지 않아야 한다.
- 고립성(Isolation) : 트랜잭션 실행 중 생성하는 **연산의 중간 결과는 다른 트랜잭션이 접근 불가**
- 영속성(Durability) : 트랜잭션이 그 실행을 성공적으로 완료하면, 그 결과는 영구적으로 보장되어야 한다.

# ERD 구성 요소

- 엔터티, 관계, 속성

# SQL 실행 순서 (파 인 실)

파싱 -> 인출 -> 실행

- 파싱 : SQL문의 문법을 확인하고 구문분석한다. 구문 분석한 SQL문은 Library Cache에 저장
- 실행 : 옵티마이저가 수립한 실행계획에 따라 SQL 실행
- 인출 : 데이터를 읽어서 전송

# 데이터 유형 ORACLE / SQL SERVER

- CHAR(s) : 고정 길이 문자열 정보 ('AA' = 'AA ')
- VARCHARS(s) : 가변길이 문자열 정보 ('AA' != 'AA ')
- NUMBER / NUMERIC : 정수, 실수 등 숫자 정보
- DATE / DATETIME : 날짜와 시각 정보

# DDL

- CREATE : 테이블 생성 / 테이블 생성 시 기본키, 왜래키, 기타 제약사항 등을 설정할 수 있다.
- ALTER : 테이블 변경 / 칼럼 추가, 변경, 삭제 / 기본키 설정하거나 외래키 설정
- DROP : 테이블 삭제 /데이터 구조뿐 아니라 저장된 데이터도 모두 삭제
- RENAME

# CREATE

```SQL
CREATE TABLE EMP (
    empno number(10),   -- 정수/실수형
    ename varchar2(20),   -- 가변 길이 문자형
    sal number(10,2) default 0, -- 소수점 둘째자리 까지 저장, 초기화값 0
    deptno varchar2(4) not null, -- not null 제약조건 추가 (무조건 값이 있어야 함)
    createdate date default sysdate, -- 날짜를 저장할 속성, 초기화 값은 현재 날짜(sysdate)
    constraint emp_pk primary key (empno) -- 기본키 설정
)
```

## 테이블 생성시 주의사항

- 테이블 이름은 중복될 수 없음.
- 한 테이블 내의 칼럼명은 중복될 수 없음.
- 각 칼럼은 , (콤마)로 구분. ; (세미콜론)으로 끝남
- 칼럼 뒤에 데이터 유형은 꼭 지정되어야 함
- 테이블 명과 칼럼명은 반드시 문자로 시작해야 함
- A-Z, a-z, 0-9, \_, $, #만 사용 가능.
- DATETIME 타입에는 별도로 크기를 지정하지 않는다.
- 기본키가 2개일 때 정의 예시
  constraint emp_pk primary key (empno, ename)

## 테이블 생성 시 CASCADE 사용.

- cascade는 참조 무결성을 유지하기 위한 제약 조건.
- 참조하는 부모 테이블의 키가 삭제될 경우 참조하는 자식 테이블에서의 해당 데이터가 같이 삭제된다.

```sql
constraint d_fk foreign key(deptno) dept (deptno)
    on delete cascade
```

## 제약 조건

- PRIMARY KEY(기본키) : 기본키 정의
- UNIQUE KEY(고유키) : 고유키 정의
- NOT NULL : NULL 입력 금지
- CHECK : 입력 값 범위 제한 / NULL 무시(NULL 가능)
- FOREIGN KEY(외래키) : 외래키 정의

> 테이블 구조 확인 : 오라클 -> DESC 테이블 / SQL server -> sp_help 'dbo.테이블'

# ALTER

```SQL
-- 칼럼 추가
ALTER TABLE emp ADD (age number(2) default 1);

-- 칼럼 변경
ALTER TABLE emp MODIFY (ename varchar(40) not null);

-- 칼럼 삭제
ALTER TABLE emp DROP COLUMN age;

-- 칼럼명 변경 (oracle / sql-server)
ALTER TABLE emp RENAME COLUMN ename TO newNM;
sp_rename 변경전칼럼, 새로운칼럼, 'CLOUMN'

-- 테이블 제약조건 삭제
ALTER TABLE emp DROP CONSTRAINT 조건명;

-- 테이블 제약 조건 추가
ALTER TABLE emp ADD CONSTRAINT 조건명 조건 (칼럼명);
```

# DROP

```sql
-- 테이블 삭제
DROP TABLE emp;

-- 참조된 제약사항까지 모두 삭제
DROP TABLE emp CASCADE CONSTRAINT;
```

# TRUNCATE

```sql
-- 테이블 데이터 삭제
TRUNCATE TABLE emp;
```

# DROP / TRUNCATE / DELETE 차이점

1. DROP : DDL / 테이블 정의 자체를 완전 삭제
2. TRUNCATE : DDL (일부 DML)/ 최초 생성된 초기 상태로 저장공간을 재사용 가능하도록 한다
3. DELETE : DML / 데이터만 삭제

> Dependent : 관계형 데이터베이스에서 자식 테이블의 FK 데이터 생성 시, 부모 테이블에 PK가 없는 경우, 자식테이블의 데이터 입력을 허용하지 않는 참조 동작

# 뷰(VIEW)

- 테이블로부터 유도된 **가상의 테이블**
- 실제 데이터를 가지고 있지 않고 **테이블을 참조해서 원하는 칼럼만 조회**가능
- **데이터 딕셔너리에 SQL문 형태로 저장**하고 실행 시에 참조된다.

# 뷰의 특징

- 참조한 **테이블이 변경되면 뷰도 변경**된다. (당연!)
- 특정 칼럼만 조회시켜서 **보안성을 향상**시킬 수 있다.
- 뷰의 검색은 참조한 테이블과 동일하게 할 수 있지만, 뷰에 대한 입력/수정/삭제에는 제약 발생.
- 한 번 생성된 뷰는 변경 못하고, 변경하려면 삭제 후 재생성해야함.
- ALTER문을 사용해서 뷰를 변경할 수 없다.

## 뷰 생성/조회/삭제

- 뷰 생성
  CREATE VIEW v_emp AS SELECT \* FROM emp;
- 뷰 조회
  SELECT \* FROM v_emp;
- 뷰 삭제
  DROP VIEW v_emp;

## 뷰의 장/단점

- 장점 : 특정 칼럼만 조회할 수 있기 때문에, 보안 기능 향상 / 데이터 관리 간단/ SELECT 문이 간단 / 하나의 테이블에 여러 개의 뷰를 생성할 수 있다.
- 단점 : 뷰는 독자적인 인덱스를 만들 수 없다. / 삽입,수정,삭제 연산에 제약이 있다. / 데이터 구조 변경 불가

# INSERT

- 테이블의 모든 칼럼에 삽입하는 경우 칼럼명 생략 가능.
- Auto Commit이 아니라면 Commit 을 해야한다.

```sql
INSERT INTO table (col1, col2) VALUES (value1, value2);
```
