# 면접 예상 질문

## ✅ 기본 개념

### Redis는 어떤 용도로 사용하나요?

- 인메모리 기반의 Key-Value 저장소
- 캐시, 세션 저장소, 분산 락, 메시지 큐, 실시간 랭킹 등 다양한 용도로 활용

### Redis는 메모리 기반인데, 데이터는 어떻게 유지되나요?

- AOF(Append Only File): 모든 쓰기 명령을 로그로 저장
- RDB(Snapshot): 주기적으로 메모리 상태를 덤프
- AOF + RDB 혼합 사용 가능

### Redis에서 지원하는 자료구조는 어떤 게 있고, 각각 어떤 상황에 쓰이나요?

- `String`: 일반 Key-Value
- `List`: 대기열, 스택
- `Set`: 중복 없는 데이터 (태그 등)
- `Sorted Set`: 점수 기반 랭킹
- `Hash`: 객체 저장 (사용자 프로필)
- `Stream`: 로그, 메시지 큐

### Redis는 단일 스레드인데, 어떻게 고성능을 낼 수 있나요?

- 메모리 기반 + Non-blocking I/O 기반 Event Loop 구조
- 락이 없고 컨텍스트 스위칭이 없어 처리 속도가 빠름

### Redis는 단일 스레드인데 왜 트랜잭션과 락이 필요한가요?

- 명령 자체는 직렬 처리되지만, 여러 명령을 하나의 논리 단위로 묶을 때 트랜잭션 필요
- 여러 클라이언트 또는 분산 환경에서의 동시성 제어를 위해 락 필요

## ✅ 캐시 사용 관련

### Redis를 캐시로 쓸 때 어떤 방식으로 동기화를 하나요?

- `Cache Aside`: Lazy loading, 필요 시 DB 조회 후 캐시 저장
- `Write Through`: 쓰기 요청 시 캐시와 DB 동시에 갱신
- `Write Back`: 캐시에만 저장 후 주기적으로 DB 반영

### 캐시 무효화(Cache Invalidation)는 어떻게 처리하나요?

- TTL 설정
- 데이터 변경 시 `DEL`로 삭제
- 트리거 기반 연동

### TTL 설정은 어떤 기준으로 잡았나요?

- 변경 빈도 + 사용 패턴 기반
- 자주 바뀌는 데이터는 짧게, 변동 적은 데이터는 길게

### 캐시 히트율이 낮다면 어떤 점을 먼저 점검하나요?

- 캐시 키 전략
- TTL 적절성
- Redis에 데이터가 제대로 들어가는지
- Redis 응답 속도 문제 없는지

### Redis 캐시와 DB 간의 데이터 정합성을 어떻게 유지했나요?

- 캐시 삭제 → DB 쓰기 순서 보장
- fallback 로직 구성
- TTL 기반으로 stale data 방지

## ✅ 동시성 / 트랜잭션 / 일관성

### Redis는 여러 요청이 동시에 들어올 때 순서를 어떻게 보장하나요?

- Redis는 단일 스레드로 처리 순서는 보장하지만,
- 여러 커넥션에서의 요청 도달 순서는 보장되지 않음 (Race Condition)

### Redis에서 분산 락은 어떻게 구현하나요?

- `SET key value NX PX 3000` 방식
- Redlock 알고리즘: 여러 노드에서 과반수 락 획득 시 성공

### MULTI / EXEC 트랜잭션은 어떤 상황에서 사용하나요?

- 원자적 처리 필요 시
- 예: 포인트 차감 후 로그 기록 등

### Lua 스크립트를 사용하는 이유는 무엇인가요?

- 서버 단에서 여러 명령을 원자적으로 처리
- 예: `GET → 검사 → SET`을 Lua로 묶어 동시성 문제 해결

## ✅ 운영 및 장애 대응

### Redis에서 메모리가 부족해지면 어떻게 되나요?

- 설정된 eviction 정책에 따라 데이터 제거 또는 에러 발생

### Redis eviction 정책에는 어떤 게 있고, 언제 어떤 걸 사용해야 하나요?

- `noeviction`, `allkeys-lru`, `volatile-lru`, `allkeys-random` 등
- `allkeys-lru`가 일반적으로 많이 사용됨

### Redis Sentinel과 Cluster의 차이는 무엇인가요?

- Sentinel: Master-Slave 감시, 장애 조치
- Cluster: 데이터 샤딩 + 고가용성, 대용량 대응

### Redis 장애 발생 시 어떻게 대응했나요?

- Sentinel 또는 Cluster를 통한 자동 장애 조치
- AOF/RDB 백업에서 복구
- `INFO`, `MONITOR`, `slowlog` 등으로 원인 파악

### Redis의 모니터링은 어떤 방식으로 하나요?

- `INFO`, `slowlog`, `keyspace hits/misses`
- Prometheus + Grafana를 통한 시각화

## ✅ 고급 기능 및 구조

### Redis Pub/Sub과 Kafka의 차이를 설명해주세요.

| 항목        | Redis Pub/Sub | Kafka               |
| ----------- | ------------- | ------------------- |
| 영속성      | ❌ 없음       | ✅ 있음             |
| 순서 보장   | ❌ 불확실     | ✅ 파티션 단위 보장 |
| 소비자 그룹 | ❌ 없음       | ✅ 있음             |
| 유실 가능성 | 높음          | 낮음                |

- Redis: 실시간 알림
- Kafka: 로그 기반, 대용량 처리

### Redis Stream은 어떤 상황에 적합한가요?

- 소비자가 메시지를 놓치지 않고 읽어야 할 때
- Kafka 대체로 사용 가능
- 소비자 그룹 및 메시지 유지 필요 시

### Redis에서 LRU, LFU 캐시 정책 차이를 설명해주세요?

- **LRU**: 가장 오랫동안 사용되지 않은 항목 제거
- **LFU**: 가장 적게 사용된 항목 제거

### Redis Cluster에서 키가 어떤 방식으로 샤딩되나요?

- 키를 0~16383 해시 슬롯으로 분할
- 각 노드가 특정 슬롯 범위 담당
- `{}` 사용 시 동일 슬롯으로 고정 가능

### Redis의 persistence 방식(AOF, RDB)은 각각 언제 사용하면 좋을까요?

- AOF: 복구 정확성 중요할 때 (로그 기반)
- RDB: 빠른 백업/복원이 중요할 때 (스냅샷)
- 둘을 함께 쓰는 경우도 많음

## ✅ 기타

### Sorted Set을 사용한 랭킹 시스템은 어떻게 구현하셨나요?

- `ZADD`, `ZRANGE`, `ZREVRANGE`, `ZINCRBY` 사용
- 유저 ID를 키로, 점수를 score로 저장
- TTL과 랭킹 초기화 로직 구현

### Redis에 쌓는 데이터가 많아졌을 때 메모리 이슈는 어떻게 해결하셨나요?

- TTL 설정
- 메모리 초과 정책 설정 (`maxmemory`, `eviction`)
- 불필요한 데이터 주기적 정리
- 대용량 데이터는 Redis 외부로 이동

### Redis를 대체할 수 있는 대안 기술을 고려해본 적 있나요?

- 캐시: Memcached
- 메시징: Kafka, RabbitMQ
- 락/컨센서스: Zookeeper, Etcd
- 세션 저장: DB, JWT 기반
