> https://www.youtube.com/watch?v=PC3ypt_VGWI

> https://youtu.be/PC3ypt_VGWI?t=330

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
