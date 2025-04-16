# Transaction & Lua & Distributed Lock

## ✅ 트랜잭션(MULTI / EXEC)

Redis는 **MULTI / EXEC 명령어를 통해 간단한 트랜잭션 처리**를 지원합니다. 여러 명령을 하나의 블록으로 묶어 **순차적으로 실행**하지만, 완전한 롤백은 지원하지 않습니다.

```bash
MULTI
SET key1 "value1"
INCR counter
EXEC
```

### 특징

- EXEC 이전까지는 명령을 큐에 넣기만 함
- EXEC 실행 시 큐에 있는 명령을 순서대로 실행
- 중간에 실패한 명령이 있어도 나머지는 실행됨 (부분 실패 가능)

## ✅ WATCH 명령으로 낙관적 락 구현

WATCH를 사용하면 **트랜잭션 실행 전 키 변경 여부를 감지하여 충돌을 방지**할 수 있습니다.

```bash
WATCH balance
MULTI
DECR balance
EXEC
```

- EXEC 전 `balance`가 변경되면 → 트랜잭션 취소됨 (EXEC는 null 반환)
- 낙관적 락(Optimistic Lock) 구현 가능

> [!WARNING]
> WATCH는 MULTI보다 먼저 호출해야 하며, 트랜잭션 이후 자동으로 UNWATCH됨

## ✅ Lua Script로 원자성 보장

복잡한 연산을 트랜잭션처럼 **원자적으로 실행**하려면 Lua 스크립트를 사용할 수 있습니다.

```bash
EVAL "return redis.call('incrby', KEYS[1], ARGV[1])" 1 counter 10
```

- 모든 명령은 단일 명령처럼 실행됨 (중간에 다른 명령이 끼어들 수 없음)
- WATCH 없이도 원자성 보장

### Lua의 활용 예시

- GET → 검사 → SET (ex. 존재 여부 확인 후 삽입)
- 여러 키 갱신, 조건 분기 처리 등
- 분산락 구현 (SETNX + TTL)

> ✅ 스크립트는 `redis.call`을 통해 내부 명령어 사용

## ✅ 분산 락 (Distributed Lock)

Redis는 분산 환경에서 락을 구현할 수 있도록 기본 명령을 제공합니다.

### 기본 방식: `SET NX PX`

```bash
SET lock:order123 "lock_token" NX PX 3000
```

- `NX`: 키가 존재하지 않을 때만 설정
- `PX`: TTL (ms 단위) 설정
- 성공 시 락 획득, 실패 시 락 보유 중임을 의미

### 락 해제 시 주의사항

- **반드시 자신이 설정한 락만 해제해야 함**
- Lua 스크립트를 통해 해제 조건 확인 후 삭제해야 안전

```lua
EVAL "if redis.call('get', KEYS[1]) == ARGV[1] then return redis.call('del', KEYS[1]) else return 0 end" 1 lock:order123 lock_token
```

## ✅ Redlock 알고리즘

Redis 공식 문서에 소개된 **분산 락의 안전한 구현 방식**입니다.

- 5개 이상의 Redis 노드에 동시에 락 요청
- 최소 과반수(N/2 + 1) 성공 시 락 획득으로 간주
- 시간 보정, 유효시간 내 실행, 단일 해시 키 등 조건 고려 필요

> [!WARNING]
> Redlock은 안정성 논란이 있었지만, 대부분의 단일 데이터센터 환경에서는 충분히 실용적임

## ✅ 실무에서의 선택 기준

| 상황                        | 추천 방법        |
| --------------------------- | ---------------- |
| 단순 트랜잭션 처리          | MULTI / EXEC     |
| 조건부 실행, 충돌 방지 필요 | WATCH + MULTI    |
| 원자적 연산 + 복잡한 로직   | Lua Script       |
| 멀티 인스턴스 락 구현 필요  | Redlock 알고리즘 |
