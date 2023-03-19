# ROLLUP 함수 사용 시기
- 그루핑 된 것을 최종 집계하기 위해 발달한 함수
- ROLLUP이 말아올리다. (분류된걸 말아올려서 합계를 낸다)

# 사용법
```SQL
SELECT [조회하고자 하는 부분]
FROM [테이블]
WHERE [테이블에서 GROUP BY 진행 전 필터링 조건]
GROUP BY ROLLUP (묶는 기준) -- 그룹별 결과 뿐 아니라 총 집계도 해줌.
HAVING [GROUP BY 결과 필터링할 조건]
```

- GROUP BY 절만 사용하면 나눠진 데이터에 대한 결과 조회
- ROLLUP까지 하면 그 나눠진거까지 묶어서 결과를 집계 해줌