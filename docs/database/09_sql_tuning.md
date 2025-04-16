# SQL Tuning

## ✅ SQL 튜닝이란?

SQL 튜닝은 데이터베이스 쿼리의 성능을 개선하여 응답 시간을 단축하고, 시스템 자원을 효율적으로 사용하는 작업이다. 실행 계획 분석, 쿼리 구조 개선(Query Rewriting), 인덱스 최적화, 데이터베이스 설정 조정 등이 포함된다.

## ✅ 주요 목표

- 실행 시간 단축: 불필요한 연산을 줄이고 효율적인 데이터 접근을 구현
- 자원 최적화: CPU, 메모리, 디스크 I/O 등 자원 소비를 줄인다
- 시스템 부하 완화: 동시 접속 환경에서도 안정적인 성능을 유지한다

## ✅ SQL 튜닝 절차 및 예시

### 1. 문제 파악

- 느린 쿼리 탐지: DB 로그나 모니터링 툴을 통해 성능이 저하된 쿼리를 찾는다

```sql
SELECT * FROM orders WHERE order_date >= '2023-01-01';
```

- 자주 호출되는 쿼리 목록화: 빈도 높은 쿼리를 파악하고 우선순위를 설정한다

### 2. 실행 계획 분석

- EXPLAIN 명령어를 활용하여 쿼리 실행 계획을 분석하고 인덱스 사용 여부, 접근 방식, 예상 비용 등을 확인한다

```sql
EXPLAIN SELECT * FROM orders WHERE customer_id = 123;
```

- 분석 항목:
  - 사용된 인덱스 (key)
  - 접근 방식 (type)
  - 예상 읽기 건수 (rows)
  - 추가 정보 (extra)

### 3. 쿼리 최적화

- 불필요한 연산 제거: 서브쿼리, 중복 연산, SELECT \* 등 제거

```sql
-- Before
SELECT * FROM orders WHERE customer_id = 123;

-- After
SELECT order_id, order_date, total_amount FROM orders WHERE customer_id = 123;
```

- 조인 최적화: 불필요한 조인을 제거하고 조인 순서를 조정한다
- 쿼리 리라이팅: 동일한 결과를 더 효율적인 구조로 재작성한다

### 4. 인덱스 최적화

- 자주 사용하는 WHERE, JOIN, ORDER BY 컬럼에 인덱스를 생성한다

```sql
CREATE INDEX idx_orders_customer_id ON orders(customer_id);
```

- 커버링 인덱스 적용: 쿼리에서 필요한 모든 컬럼이 인덱스에 포함되도록 설계한다

```sql
CREATE INDEX idx_orders_covering ON orders(customer_id, order_date, total_amount);
```

- 불필요하거나 중복된 인덱스는 제거한다

### 5. 데이터베이스 설정 조정

- 버퍼 및 캐시 크기 설정: 메모리 캐시 활용을 최적화해 디스크 I/O를 줄인다
- 통계 정보 갱신: 옵티마이저가 최신 정보를 기준으로 계획을 수립하도록 한다

```sql
ANALYZE TABLE orders;
```

### 6. 모니터링 및 테스트

- 부하 테스트: 실제와 유사한 환경에서 테스트를 진행한다
- 실행 계획 재확인: 튜닝 후 EXPLAIN 결과를 재검토한다
- 문서화: 튜닝 전후의 성능 비교와 변경 사항을 기록한다

## ✅ 실무 팁

- 점진적 개선: 전체 쿼리를 한 번에 고치기보다 자주 사용하는 쿼리부터 순차적으로 튜닝한다
- 모니터링 도구 사용: APM, slow query log, Performance Schema 등 다양한 도구를 활용한다
- 테스트 중요성: 튜닝 결과가 실제 서비스에 미치는 영향을 충분히 검증한다
- SQL Plan Management(SPM)나 Optimizer Hints 등을 활용해 특정 쿼리 실행 계획을 직접 제어할 수도 있음
