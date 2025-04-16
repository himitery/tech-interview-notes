# Troubleshooting

## ✅ 메시지 유실 문제

### 증상

- Producer는 전송했지만 Consumer가 메시지를 받지 못함
- 메시지가 Retention 기간 전에 삭제됨

### 원인 및 조치

- Producer의 `acks=0` 설정 → `acks=all` 또는 `acks=1`로 변경
- Consumer가 offset을 자동 commit했지만 처리 전에 장애 발생 → manual commit 고려
- Retention 정책이 너무 짧게 설정되어 있음 → `retention.ms` 재설정 필요

## ✅ Consumer Lag 증가

### 증상

- Consumer가 최신 메시지를 따라가지 못함
- 모니터링 지표에서 Lag 지속적으로 증가

### 원인 및 조치

- Consumer 처리 속도 < 메시지 생성 속도 → Consumer 개수/성능 확장
- Rebalancing 잦음 → session.timeout.ms, max.poll.interval.ms 조정
- 메시지 처리 병목 (DB 쓰기, 외부 API 호출 등) → 비동기 처리 고려

## ✅ Under Replicated Partitions 발생

### 증상

- 일부 Partition이 ISR에서 이탈
- `under_replicated_partitions` 지표가 0 이상 유지됨

### 원인 및 조치

- Follower 브로커가 리더로부터 데이터 복제를 못 따라감 → 네트워크 상태 확인
- 브로커 재시작 또는 과부하 → 리소스 사용량 확인
- `min.insync.replicas` 설정과 충돌 시 쓰기 실패 가능

## ✅ Controller 반복 변경

### 증상

- Controller가 지속적으로 교체됨
- 메타데이터 처리 지연, Rebalancing 발생

### 원인 및 조치

- Zookeeper 연결 불안정
- 특정 Broker의 네트워크/디스크/GC 문제 → 로그 확인 및 자원 점검

## ✅ 토픽 설정 반영 안됨

### 증상

- `retention.ms`, `segment.bytes` 등 설정 변경 후에도 반영되지 않음

### 원인 및 조치

- CLI에서 설정 후 반영 확인 필요 → `kafka-topics.sh --describe`
- 설정이 Partition 단위가 아닌 Topic 단위인지 확인

## ✅ 메시지 순서 꼬임

### 증상

- 동일 키 메시지가 순서대로 처리되지 않음

### 원인 및 조치

- Key 없이 전송 → 라운드로빈으로 파티션 분산됨 → 키 지정 필요
- Consumer 병렬 처리 중 순서가 꼬임 → 순차 처리 보장 구조 필요 (예: 파티션 단위로 처리)

## ✅ Kafka 자체 성능 저하

### 증상

- TPS 저하, request latency 증가

### 원인 및 조치

- 디스크 I/O 병목 → 디스크 스토리지 성능 확인 (SSD 권장)
- GC 병목 → JVM Heap 크기 조정 또는 GC 방식 변경
- 너무 많은 토픽/파티션 수 → 관리 오버헤드 발생
