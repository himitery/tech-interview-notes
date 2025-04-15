# Troubleshooting

Redis를 운영하면서 겪을 수 있는 주요 문제 상황과 그에 대한 원인 분석 및 해결 방안을 정리합니다.

## ✅ 캐시 미스가 너무 자주 발생해요

### 🔍 원인

- TTL이 너무 짧음
- 캐시 키 설정이 일관되지 않음 (접두사 누락 등)
- `maxmemory-policy`가 aggressive하게 설정됨 (eviction이 자주 발생)

### 🛠️ 해결

- TTL 적절히 조정 (기본적으로 수 분 이상)
- 키 네이밍 규칙 일관화 (예: `user:{id}:profile`)
- `allkeys-lru` 대신 `volatile-lru` 등 보수적 정책 고려

## ✅ Redis가 너무 자주 재시작돼요 (OOM 등)

### 🔍 원인

- 메모리 초과 (Out Of Memory)
- 너무 많은 key, 너무 큰 value 저장

### 🛠️ 해결

- `maxmemory` 설정 확인 및 조정
- key/value 크기 제한 → 큰 value는 외부 저장소 사용
- `MEMORY USAGE`, `INFO` 명령으로 용량 분석

## ✅ 특정 명령이 느려요 (latency spike)

### 🔍 원인

- 느린 명령어 (예: `ZRANGE`, `LRANGE`, `SUNION` 대량 데이터)
- RDB 저장 중 일시적 성능 저하
- AOF rewrite 등 백그라운드 작업 영향

### 🛠️ 해결

- `SLOWLOG` 확인 및 최적화
- 필요 시 Lua 스크립트로 연산 이동
- 백그라운드 작업은 부하 적은 시간에 실행 (예: CRON)

## ✅ key가 너무 많아서 조회가 느려요

### 🔍 원인

- Redis는 keyspace 검색에 O(n) 비용이 발생할 수 있음
- 대량의 키를 단건 검색으로 처리할 경우 성능 저하

### 🛠️ 해결

- `SCAN` 명령어 사용 (비차단 반복자 방식)
- 필요한 키만 정해진 패턴으로 필터링 (`SCAN 0 MATCH prefix:*`)
- 대량 키 관리 시 TTL/eviction 적극 활용

## ✅ 복제(Redis Replication)가 느려요

### 🔍 원인

- 네트워크 지연
- Master 노드의 성능 부족 (slow command 다수)

### 🛠️ 해결

- 슬레이브의 `offset`, `repl_backlog_size` 확인
- `INFO replication`으로 동기 상태 모니터링
- Master 노드에 무거운 명령 줄이기 (slowlog 활용)

## ✅ 클러스터에서 MOVED 에러가 떠요

### 🔍 원인

- 클라이언트가 올바른 노드로 라우팅하지 못함
- 클러스터 모드 비호환 클라이언트 사용

### 🛠️ 해결

- Cluster-aware 클라이언트 사용 (`ioredis`, `Lettuce` 등)
- MOVED 응답 수신 시 자동 재요청 기능 확인
