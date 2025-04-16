# Query Execution

## ✅ SQL 실행 순서

SQL 쿼리는 작성한 순서대로 실행되지 않으며, 실제 실행 순서는 다음과 같음:

1. **FROM / JOIN**: 테이블 선택 및 조인 수행
2. **WHERE**: 조건 필터링
3. **GROUP BY**: 그룹화 수행
4. **HAVING**: 그룹 결과 조건 필터링
5. **SELECT**: 컬럼 선택 및 표현식 계산
6. **DISTINCT**: 중복 제거
7. **ORDER BY**: 정렬 수행
8. **LIMIT**: 결과 개수 제한

```sql
SELECT name, COUNT(*)
FROM employees
WHERE department = 'dev'
GROUP BY name
HAVING COUNT(*) > 1
ORDER BY name
LIMIT 10;
```

이 쿼리는 내부적으로 `FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY → LIMIT` 순서로 처리됨.

## ✅ 실행 계획 확인

데이터베이스가 쿼리를 어떻게 실행하는지 확인하기 위해 `EXPLAIN` 또는 `EXPLAIN ANALYZE`를 사용함.

### MySQL

```sql
EXPLAIN SELECT * FROM employees WHERE department = 'dev';
EXPLAIN ANALYZE SELECT * FROM employees WHERE department = 'dev';
```

### PostgreSQL

```sql
EXPLAIN SELECT * FROM employees;
EXPLAIN ANALYZE SELECT * FROM employees;
```

### 주요 항목 설명

- **type**: 접근 방식 (ALL, index, range 등)
- **key**: 사용된 인덱스
- **rows**: 스캔한 예상 row 수
- **Extra**: 추가 정보 (e.g. Using where, Using index)

## ✅ 실행 계획 예시 (MySQL)

```text
id | select_type | table | type | possible_keys | key  | rows | Extra
1  | SIMPLE      | emp   | ref  | dept_idx      | dept_idx | 10 | Using where
```

## ✅ 쿼리 최적화를 위한 팁

- WHERE 조건절에는 인덱스를 적용할 수 있도록 작성
- SELECT 절에는 필요한 컬럼만 지정 (SELECT \*은 피하기)
- ORDER BY, GROUP BY에는 인덱스를 활용할 수 있는 컬럼 지정
- LIMIT과 OFFSET을 함께 사용할 경우 성능 고려 필요
- GROUP BY나 ORDER BY에 사용되는 컬럼도 인덱스로 구성하면 성능을 높일 수 있음

## ✅ 면접 포인트

- SQL 실행 순서를 설명할 수 있어야 함
- 실행 계획을 통해 쿼리 동작을 해석할 수 있어야 함
- 인덱스가 WHERE, JOIN, GROUP BY에서 어떻게 작동하는지 이해해야 함
