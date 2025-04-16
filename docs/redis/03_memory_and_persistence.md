# Memory & Persistence

## ✅ Redis의 메모리 구조

Redis는 모든 데이터를 **메모리**에 저장하여 초고속 처리를 지향합니다. 하지만 단순 메모리 저장소가 아닌, 데이터를 **디스크에 영속적으로 보관할 수 있는 기능**(AOF/RDB)도 함께 제공합니다.

## ✅ maxmemory 설정

- Redis는 기본적으로 메모리 사용량 제한이 없습니다.
- `maxmemory` 설정을 통해 최대 사용 가능 메모리를 제한할 수 있으며, 초과 시 **Eviction 정책**에 따라 데이터를 제거합니다.

```bash
maxmemory 256mb
```

> [!NOTE]
> 운영 중에는 `CONFIG SET maxmemory`로 동적으로 설정 가능

## ✅ Eviction 정책 (메모리 초과 시 동작)

| 정책 이름       | 설명                                             |
| --------------- | ------------------------------------------------ |
| noeviction      | 제거 없이 에러 반환                              |
| allkeys-lru     | 전체 키 중 가장 오래 사용되지 않은 키 제거       |
| volatile-lru    | TTL이 있는 키 중 가장 오래 사용되지 않은 키 제거 |
| allkeys-random  | 전체 키 중 무작위 제거                           |
| volatile-random | TTL이 있는 키 중 무작위 제거                     |
| volatile-ttl    | TTL이 가장 빨리 만료되는 키 제거                 |

```bash
maxmemory-policy allkeys-lru
```

> [!NOTE]
> `allkeys-lru`가 가장 많이 사용되며, 캐시 서버에 적합

## ✅ TTL(Time To Live)

- Redis는 각 키에 대해 TTL을 설정할 수 있습니다.
- TTL이 지난 키는 Lazy 또는 Active 방식으로 삭제됩니다.

```bash
SET session:user1 abc EX 60  # 60초 후 만료
TTL session:user1            # 남은 TTL 확인
```

| 삭제 방식 | 설명                                                |
| --------- | --------------------------------------------------- |
| Lazy      | 해당 키에 접근 시 만료 여부 확인 후 삭제            |
| Active    | 백그라운드에서 주기적으로 샘플링하여 만료된 키 삭제 |

## ✅ Redis의 영속성(Persistence) 방식

Redis는 메모리 기반이지만, 장애 복구와 데이터 보존을 위해 두 가지 영속성 방식을 제공합니다:

### RDB (Snapshot 방식)

- 일정 간격으로 메모리 상태를 디스크에 저장 (Binary)
- 빠른 복구, 적은 디스크 사용량
- 단점: 중간에 데이터 유실 가능성 있음

```bash
save 900 1   # 900초 동안 1건 이상 변경 시 저장
save 60 1000 # 60초 동안 1000건 이상 변경 시 저장
```

### AOF (Append Only File)

- 모든 쓰기 명령을 로그로 저장 (텍스트 기반)
- 서버 재시작 시 AOF 로그를 재실행하여 상태 복원
- 더 높은 데이터 정확성, 하지만 디스크 사용량 많고 느림

```bash
aof-enabled yes
appendfsync everysec   # 1초 단위로 디스크에 flush
```

### RDB + AOF 혼합 사용

- Redis 4.0부터 AOF가 RDB 스냅샷을 먼저 기록하고 이후 append 방식으로 전환하는 hybrid 모드 지원

```bash
appendonly yes
appendfilename "appendonly.aof"
save 60 1000
```

> [!NOTE]
> 일반적으로 **복구 속도 중시 → RDB**, **데이터 정확성 중시 → AOF**, **실무에서는 병행 사용**

## ✅ 운영 중 고려사항

- 디스크 용량과 IOPS 고려 (AOF의 경우 특히 중요)
- AOF 파일 크기 증가 → `BGREWRITEAOF` 명령어로 압축 가능
- RDB 저장 주기는 시스템 성능 영향 고려하여 신중히 설정
- 주요 모니터링 항목:
  - `used_memory`
  - `mem_fragmentation_ratio`
  - `rdb_last_save_time`
  - `aof_current_size`
