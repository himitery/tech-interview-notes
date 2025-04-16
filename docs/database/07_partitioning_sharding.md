# Partitioning & Sharding

## ✅ Partitioning이란?

Partitioning은 하나의 테이블을 논리적으로 여러 개의 파티션으로 나누어 저장하는 방식이다. 데이터의 양이 많아졌을 때, **성능 향상, 관리 용이성, 유지보수 효율성** 등을 위해 사용된다.

## ✅ Partitioning의 종류

### 1. Range Partitioning

- 특정 범위에 따라 데이터를 분할
- 예: 날짜 기준으로 월별 파티션 구성

```sql
PARTITION BY RANGE (YEAR(order_date)) (
  PARTITION p2022 VALUES LESS THAN (2023),
  PARTITION p2023 VALUES LESS THAN (2024)
);
```

### 2. List Partitioning

- 명시된 값의 목록에 따라 데이터를 분할
- 예: 지역 또는 국가 코드로 분할

```sql
PARTITION BY LIST (region) (
  PARTITION asia VALUES IN ('KR', 'JP', 'CN'),
  PARTITION us VALUES IN ('US')
);
```

### 3. Hash Partitioning

- 해시 함수 결과에 따라 파티션 분산
- 균등하게 데이터 분산 가능

```sql
PARTITION BY HASH(user_id) PARTITIONS 4;
```

### 4. Key Partitioning

- 내부적으로 해시를 사용하되, 사용자 정의 키가 아닌 기본 키나 유니크 키를 기준으로 분할

## ✅ Partitioning의 장단점

### 장점

- 대용량 테이블 관리 용이
- 특정 파티션만 스캔해 쿼리 성능 향상
- 파티션 단위 백업/복원 가능

### 단점

- 파티션 키 기준의 쿼리 외에는 성능 이점이 없음
- 인덱스 관리가 복잡
- 파티션 키 변경 불가 (재분할 필요)

## ✅ Sharding이란?

Sharding은 데이터를 **물리적으로 서로 다른 데이터베이스 인스턴스나 서버에 분산 저장**하는 방식이다. 단일 데이터베이스의 한계를 극복하고, **확장성과 고가용성**을 확보하는 데 사용된다.

## ✅ Sharding 전략

### 1. Horizontal Sharding (수평 샤딩)

- 테이블의 행(Row)을 기준으로 분할
- 예: user_id에 따라 다른 DB에 분산 저장

```text
User DB1: user_id 1 ~ 10000
User DB2: user_id 10001 ~ 20000
```

### 2. Vertical Sharding (수직 샤딩)

- 테이블의 컬럼(Column) 또는 도메인 기준으로 분할
- 예: 주문 관련 테이블과 유저 테이블을 서로 다른 DB에 저장

## ✅ Sharding의 장단점

### 장점

- 데이터 용량이 증가해도 수평적 확장 가능
- 장애 발생 시 일부만 영향받음 (고가용성)

### 단점

- 조인 및 트랜잭션 처리 복잡
- 샤딩 키 설계가 매우 중요
- 샤드 간 데이터 이동 어려움 (리샤딩 이슈)

## ✅ Sharding 구현 방식

- **애플리케이션 레벨 샤딩**: 클라이언트에서 샤드 라우팅 처리
- **미들웨어 기반 샤딩**: 프록시 또는 DB 라우터가 자동으로 처리 (예: Vitess, ProxySQL)
- **DBMS 내장 샤딩**: 일부 DB는 기본적으로 샤딩 지원 (예: MongoDB, CitusDB)

## ✅ Partitioning vs Sharding

| 항목        | Partitioning             | Sharding                              |
| ----------- | ------------------------ | ------------------------------------- |
| 분할 단위   | 논리적 테이블 내부       | 데이터베이스 서버 또는 인스턴스       |
| 목적        | 관리 효율성, 성능 최적화 | 확장성, 대규모 트래픽 대응            |
| 구현 방식   | DB 내부 기능으로 구현    | 애플리케이션, 미들웨어 또는 DBMS 레벨 |
| 데이터 위치 | 동일 DB 서버 내          | 서로 다른 DB 서버                     |

## ✅ 면접 포인트

- Partitioning과 Sharding의 차이를 개념과 목적 중심으로 설명할 수 있어야 함
- Sharding에서 수평/수직 분할의 개념과 장단점을 알고 있어야 함
- 샤딩 키 설계 시 고려해야 할 사항과 리샤딩 이슈에 대해 설명할 수 있어야 함
- 샤딩 키가 잘못 설계되면 리샤딩이 필요하며, 이는 운영 환경에서 비용이 매우 크게 발생할 수 있음
