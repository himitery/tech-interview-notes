# Use Cases

Redis는 단순한 캐시 서버를 넘어서 다양한 실무 상황에서 핵심 인프라로 활용됩니다.
이 문서에서는 대표적인 Redis 활용 사례들을 구조와 함께 설명합니다.

## ✅ 캐시 (Cache Aside 패턴)

- 가장 일반적인 사용 방식
- DB 부하 감소 및 응답 속도 향상을 위해 사용

```plaintext
Client → Redis (조회 시도)
   ↓        ↓ (miss)
  Cache hit   DB → Redis 저장 후 반환
```

- 전략: Cache Aside + TTL + allkeys-lru eviction

## ✅ 세션 저장소 (Session Store)

- 로그인 세션, JWT 로그아웃 처리 등에 사용
- TTL 기반으로 자동 만료 관리 가능

```plaintext
Login 시 → Redis에 세션 저장 (EX 3600)
API 호출 시 → Redis에서 세션 검증
```

- 전략: Key = session:{token}, TTL = 유효기간

## ✅ 실시간 랭킹 시스템 (Sorted Set)

- 점수 기반 정렬이 필요한 서비스에 최적화

```plaintext
ZADD rank 100 "user1"
ZADD rank 80 "user2"
ZREVRANGE rank 0 9 WITHSCORES
```

- 실시간으로 유저 랭킹, 게임 점수 등 처리 가능

## ✅ 분산 락 (SET NX PX + Lua)

- 주문 처리, 재고 감소 등 동시성 충돌 방지

```plaintext
SET lock:order123 "token" NX PX 3000
```

- 해제는 Lua로 보장된 토큰일 때만 수행 (atomic)

## ✅ 실시간 알림 시스템 (Pub/Sub)

- 실시간 채팅, 알림 등에 사용

```plaintext
PUBLISH channel "New message!"
SUBSCRIBE channel
```

- 단점: 메시지 유실 가능 → 중요 데이터는 Stream 사용 고려

## ✅ 메시지 큐 / 로그 처리 (Stream)

- 영속성 있는 메시지 처리 필요할 때 사용

```plaintext
XADD logs * level INFO message "Something happened"
XREAD STREAMS logs 0
```

- Kafka보다 가볍고 설정 간단, 소비자 그룹으로 병렬 처리 가능

## ✅ 카운터 / 지표 집계 (String or Hash)

- 실시간 방문자 수, 좋아요 수 등에 사용

```plaintext
INCR page:viewcount
HINCRBY post:1:metrics likes 1
```

- TTL과 함께 사용해 날짜별 집계 가능

## ✅ 출석 체크 / 비트 플래그 (Bitmap)

- 특정 유저의 특정 날 출석 여부 확인

```plaintext
SETBIT attendance:user1 5 1  # 6일차 출석
GETBIT attendance:user1 5
```

- 공간 효율성 매우 높음 (수백만 유저에도 적합)

## ✅ 유니크 카운트 (HyperLogLog)

- 대용량 유저 수 추정, UV(Unique Visitor) 등에 사용

```plaintext
PFADD visitors user1
PFCOUNT visitors
```

- 약간의 오차는 있으나 메모리 절약 효과 큼
