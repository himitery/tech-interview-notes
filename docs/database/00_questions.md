# 면접 예상 질문

## ✅ 기본 개념

### 데이터베이스와 DBMS의 차이는?

- 데이터베이스는 데이터를 저장하는 집합
- DBMS는 데이터를 정의, 생성, 조작, 제어할 수 있도록 지원하는 소프트웨어

### RDBMS의 특징은?

- 테이블 기반 구조, 정규화, SQL 사용, 트랜잭션 지원 (ACID)

### NoSQL과 RDBMS의 차이점은?

- NoSQL은 스키마가 없고 수평 확장에 유리하며, 다양한 저장 방식 지원 (문서, 키-값, 그래프 등)
- RDBMS는 스키마 기반, 복잡한 쿼리 및 정합성 보장에 강함
- 최근에는 NoSQL에서도 트랜잭션(ACID)을 지원하거나 일관성 모드를 유연하게 선택할 수 있는 사례가 늘고 있음

### CAP 이론이란?

- Consistency, Availability, Partition Tolerance 세 가지 특성 중 두 가지만 만족 가능

### BASE란?

- Basically Available, Soft state, Eventual consistency
- NoSQL의 느슨한 일관성 모델

## ✅ 트랜잭션과 동시성 제어

### 트랜잭션이란?

- 데이터베이스에서 하나의 논리적인 작업 단위, 모두 성공하거나 모두 실패해야 함

### 트랜잭션의 ACID란?

- Atomicity: 모두 수행되거나 모두 실패
- Consistency: 일관성 있는 상태 유지
- Isolation: 동시 실행에도 결과 일관성 보장
- Durability: 장애 후에도 결과 유지

### 트랜잭션 격리 수준이란?

- READ UNCOMMITTED, READ COMMITTED, REPEATABLE READ, SERIALIZABLE
- 동시성 처리에 따른 데이터 일관성 수준을 조절

### 격리 수준별 문제점은?

- Dirty Read, Non-repeatable Read, Phantom Read

### Lock이란?

- 트랜잭션 간 충돌을 방지하기 위한 동기화 메커니즘
- Shared Lock (읽기), Exclusive Lock (쓰기)

### 낙관적 락 vs 비관적 락

- 낙관적: 충돌이 드물다고 가정하고, 커밋 전에 충돌 확인
- 비관적: 충돌이 자주 발생한다고 가정하고, 미리 락 걸기

## ✅ 인덱스

### 인덱스란?

- 데이터를 빠르게 조회하기 위한 보조 자료구조
- 검색 성능을 향상시키지만, 쓰기 성능과 저장 공간을 사용할 수 있음

### 인덱스의 자료구조는?

- B+Tree: 정렬 기반 범위 탐색에 유리
- Hash: 동등 비교 전용
- Inverted Index: 전문 검색용

### 클러스터드 vs 논클러스터드 인덱스

- 클러스터드: 실제 데이터가 인덱스 순서대로 저장됨
- 논클러스터드: 인덱스만 별도 저장, 실제 데이터 주소 참조

### 인덱스를 사용하지 못하는 경우는?

- WHERE 절에 함수, 연산자 사용 시
- LIKE 검색에 `%`로 시작하는 조건
- 중복이 많은 컬럼 또는 데이터 양이 작을 경우

### 커버링 인덱스란?

- 인덱스에 쿼리에서 필요한 모든 컬럼이 포함되어 있어 테이블 접근 없이 결과 반환 가능

## ✅ SQL 실행과 옵티마이저

### SELECT 쿼리 실행 순서는?

- FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY → LIMIT

### 옵티마이저란?

- 쿼리를 최적의 실행 계획으로 변환하는 DB 내부 컴포넌트
- 비용 기반 (CBO), 규칙 기반 (RBO) 방식 존재

### EXPLAIN은 어떤 역할을 하나요?

- SQL 실행 계획을 확인해 인덱스 사용 여부, 조인 방식 등을 분석할 수 있음

## ✅ 정규화와 설계

### 정규화란?

- 데이터 중복을 제거하고 이상 현상을 방지하기 위한 설계 기법
- 1NF ~ 5NF, BCNF까지 존재

### 반정규화란?

- 성능 개선을 위해 정규화를 일부 해제해 조인을 줄이고 조회 효율을 높이는 기법

### 이상 현상이란?

- 삽입, 삭제, 갱신 시 데이터 무결성 오류 발생

## ✅ 파티셔닝과 샤딩

### 파티셔닝이란?

- 하나의 테이블을 범위, 리스트, 해시 등의 기준으로 물리적으로 분할해 성능 향상

### 샤딩이란?

- 데이터베이스 자체를 여러 노드로 수평 분산하는 방식

## ✅ 전문 검색 (Full Text Index)

### LIKE와 Full Text Search의 차이점은?

- LIKE는 단순 패턴 매칭, 인덱스 효율 떨어짐
- Full Text는 역색인 기반으로 빠른 전문 검색 가능

### Inverted Index란?

- 단어별로 해당 단어가 포함된 문서 ID를 저장하는 구조

### MySQL Fulltext vs Elasticsearch

- MySQL: 내장 검색 엔진, 분석기 제한적
- Elasticsearch: 분산 처리, 고급 형태소 분석 가능

## ✅ 실무 및 최적화

### 슬로우 쿼리는 어떻게 분석하나요?

- slow query log, EXPLAIN, ANALYZE

### 조인 성능 향상 방법은?

- 적절한 인덱스, 조건 순서 최적화, 조인 순서 튜닝

### 정합성 유지를 위한 방법은?

- 외래 키 제약조건, 트랜잭션, CASCADE 옵션 활용

### 대용량 테이블 최적화는?

- 파티셔닝, 인덱스, 비정규화, 비동기 처리, 분산 DB 고려
