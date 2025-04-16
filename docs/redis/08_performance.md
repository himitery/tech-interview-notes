# Performance & Monitoring

## ✅ Redis가 빠른 이유

Redis는 메모리 기반 + 단일 스레드 구조 덕분에 매우 빠릅니다.

| 요소            | 설명                                           |
| --------------- | ---------------------------------------------- |
| In-memory 구조  | 모든 데이터를 메모리에 저장 → 디스크 I/O 없음  |
| 단일 스레드     | 락이나 컨텍스트 스위칭 없음 → 일관된 처리 속도 |
| Event loop 기반 | 효율적인 네트워크 처리 모델                    |
| Command 단순화  | 복잡한 쿼리 대신 단일 명령 중심 구조           |

## ✅ 성능 최적화 방법

### 1. Pipeline 사용

여러 명령을 한 번에 보내 네트워크 왕복(RTT)을 줄입니다.

```python
pipe = redis.pipeline()
pipe.set("key1", "val1")
pipe.set("key2", "val2")
pipe.execute()
```

> [!NOTE]
> RTT 줄이기에 효과적. 대량 처리 시 성능 향상

### 2. Lua Script

여러 명령을 하나의 Lua로 묶어 원자적 + 빠르게 실행 가능

```lua
EVAL "return redis.call('incrby', KEYS[1], ARGV[1])" 1 counter 5
```

> [!NOTE]
> 복잡한 로직을 Redis 내부에서 처리 → 왕복 최소화

### 3. 키 설계 전략

- 접두사(`user:1:profile`)로 정렬/검색 용이
- 너무 큰 키나 긴 리스트 피하기
- 하나의 키에 너무 많은 필드 저장 시 분할 고려

### 4. Eviction 정책

- 자주 쓰이는 키가 유지되도록 LRU/LFU 정책 설정
- `maxmemory-policy` 적절히 구성

## ✅ 성능 저하 원인 및 진단

| 원인             | 진단 방법                        | 해결 방안                    |
| ---------------- | -------------------------------- | ---------------------------- |
| 느린 명령어 실행 | `SLOWLOG`                        | Lua 스크립트/파이프라인 활용 |
| 메모리 파편화    | `mem_fragmentation_ratio` (INFO) | `activedefrag` 설정 고려     |
| 블로킹 명령 사용 | `CLIENT LIST`, `MONITOR`         | 비동기 명령, timeout 설정    |
| key 수 급증      | `INFO keyspace`, `DBSIZE`        | 키 TTL 설정, 키 정리 정책    |

## ✅ 모니터링 명령어

| 명령어           | 설명                         |
| ---------------- | ---------------------------- |
| `INFO`           | 전체적인 서버 상태 출력      |
| `MONITOR`        | 실시간 명령어 처리 로그 출력 |
| `SLOWLOG get`    | 느린 쿼리 기록 확인          |
| `LATENCY DOCTOR` | 레이턴시 이벤트 요약 진단    |
| `MEMORY STATS`   | 메모리 상세 사용 현황 확인   |

> [!NOTE]
> Prometheus + Grafana 연동을 통해 시각화 및 Alerting도 가능

## ✅ 실무 성능 관리 팁

- 캐시 구조라 해도 메모리 누수 감지 중요 (TTL + eviction 필수)
- `slowlog-log-slower-than` 설정으로 기준 초과 명령 기록
- 파이프라인/스크립트 적극 활용해 네트워크 왕복 최소화
- 단일 명령 처리지만, 백그라운드 작업(RDB, AOF) 성능에도 주의
