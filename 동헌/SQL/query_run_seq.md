# SQL 쿼리 실행 순서

### SQL 쿼리가 실행되는 순서가 존재하는데 이를 알고 쿼리를 짠다면 좀 더 효율적인 쿼리문을 작성할 수 있다.

## 실행순서
> FROM - WHERE - GROUP BY - HAVING - SELECT - ORDER BY

### `FROM`
쿼리의 가장 처음 실행되는 것은 FROM 절이다. FROM 절을 통해서 테이블의 결과를 가져온다.
즉, INDEX를 사용하지 않는다는 가정하에 WHERE 절보다 먼저 실행되기 때문에 WHERE 절이나 SELECT 절에서 일부 행이나 열을 제거하여 출력한다고 해도 가장 처음에 테이블의 모든 데이터를 가져온다.

### `WHERE`
2번쨰로 실행되는 WHERE 절에서는 FROM에서 읽어온 테이블에서 조건에 맞는 결과만 갖도록 데이터를 간추린다.

### `GROUP BY`
GROUP BY 절에서 WHERE 조건으로 간추린 데이터를 선택한 칼럼으로 GROUPING 작업을 한 결과를 갖고 있다. GROUP BY 절을 사용하게 되면 해당 칼럼으로 집계함수를 사용할 수 있다.

### `HAVING`
HAVING 절과 WHERE 절과 혼돈이 오기 쉬운데 WHERE 절에 내용을 HAVING에서도 사용할 수 있다.
하지만 HAVING 에서 일반 조건을 처리하게 된다면 WHERE에서 처리하는 것 보다 비용이 훨씬 많이 소비된다.

### `WHERE` vs `HAVING`
실행 순서를 알면 좋은 쿼리를 짤 수 있는지는 여기서 나온다!!

### 조건
PRICE > 10000 이라는 조건을 WHERE 과 HAVING에서 처리해보자

`HAVING`에서 해당 조건을 처리할 경우에는 FROM에서 테이블 데이터를 가져오고 WHERE에서 데이터를 거르지 않고 GROUP BY에서 GROUPING을 진행한다 그 후에 PRICE > 10000 조건에 맞게 데이터를 추출한다.

`WHERE`에서는 전체 데이터를 가져온 다음 데이터를 한번 WHERE 절에서 간추린다. 그 후에 GROUP BY에서 GROUPING을 진행하게 된다.

즉 위와 같이 `WHERE`에서 처리할 수 있는 조건은 WHERE에서 처리하는 방식이 효율적이다.

### `SELECT`
SELECT에서 어떤 컬럼에 데이터를 출력해줄지 선택하여 출력한다.

### `ORDER BY`
마지막으로 행의 순서를 지정하여 정렬해준다.

## ALIAS 사용
``` SQL
SELECT a
    ,b AS NAME
FROM ex
WHERE a > 100
ORDER BY NAME;
```
``` SQL
SELECT a
    ,b AS JOB
    ,c AS NAME
FROM ex
WHERE JOB = 'abc'
ORDER BY NAME;
```

## ROWNUM 사용
