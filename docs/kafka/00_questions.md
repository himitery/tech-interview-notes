# 면접 예상 질문

## ✅ 기본 개념

### Kafka는 어떤 용도로 사용하나요?

- 대용량 실시간 로그 및 이벤트 데이터를 처리하는 분산 메시징 시스템
- MSA 간 통신, 실시간 데이터 스트리밍, 로그 수집 등에 사용

### Kafka는 어떤 구조로 동작하나요?

- Producer → Broker → Consumer 구조로 동작
- 메시지는 Topic에 저장되고, Topic은 여러 Partition으로 나뉘어 병렬 처리 가능

### Kafka의 주요 구성 요소는 무엇인가요?

- Producer: 메시지를 전송하는 주체
- Consumer: 메시지를 수신하는 주체
- Broker: 메시지를 저장하고 분산 처리
- Topic: 메시지가 저장되는 논리적 단위
- Zookeeper: 클러스터 상태 및 컨트롤러 관리

### Kafka의 장점은?

- 높은 처리량, 확장성, 내구성, 실시간 처리 성능
- 다양한 소비자 그룹 구성과 병렬 처리 가능

## ✅ 핵심 구성 요소

### Topic과 Partition은 무엇인가요?

- Topic: 메시지를 분류하는 논리적 단위
- Partition: 메시지를 병렬로 저장하고 처리하기 위한 물리적 단위

### Offset이란?

- 메시지의 위치를 나타내는 값으로 Consumer가 현재까지 읽은 위치를 의미
- 자동 또는 수동으로 commit 가능하며, Consumer Group 단위로 관리

### Consumer Group이란?

- 동일한 그룹에 속한 Consumer들이 메시지를 나누어 소비함
- 각 Partition은 하나의 Consumer에게만 할당됨

### Broker란?

- Kafka 클러스터 내에서 메시지를 저장하고 요청을 처리하는 서버 노드

### Replication이란?

- Partition 데이터를 여러 Broker에 복제하여 장애에 대비
- 리더와 팔로워 구조로 구성됨

## ✅ 메시지 처리 및 전달 보장

### 메시지 전송 보장 방식은?

- at most once: 유실 가능성 있음, 중복 없음
- at least once: 유실 없음, 중복 가능성 있음
- exactly once: 유실과 중복 모두 없음 (설정 필요)

### 메시지 유실은 언제 발생하나요?

- Consumer가 commit 전에 종료되었을 때
- ack 설정이 낮을 때
- 리더 변경 후 팔로워 sync가 되지 않은 경우

### acks 설정의 의미는?

- 0: 전송만 하고 확인 없음
- 1: 리더가 수신하면 성공 처리
- all: ISR의 모든 Replica가 수신해야 성공 처리

### Retention 정책이란?

- 메시지를 일정 시간 또는 용량만큼 유지하는 설정
- retention.ms 또는 retention.bytes로 관리

## ✅ 운영 및 성능

### 성능을 높이는 방법은?

- 배치 전송 (linger.ms, batch.size)
- 메시지 압축 (compression.type)
- 파티션 수 증가로 병렬 처리 향상

### Backpressure는 어떻게 대응하나요?

- Consumer 측 처리 속도 개선
- 파티션 수 조정 또는 Consumer 수 증가

### Consumer Lag이란?

- Consumer가 아직 처리하지 못한 메시지 수
- `kafka-consumer-groups` 명령어로 확인 가능

### 장애 발생 시 대응 방법은?

- Controller 장애 시 자동 선출 확인
- ISR 감소 시 알림 및 Broker 상태 점검
- Prometheus, Grafana 등으로 모니터링

## ✅ 실무 및 활용

### Kafka의 실무 활용 사례는?

- MSA 간 비동기 이벤트 브로커
- 로그 수집 및 검색 시스템 연동
- 실시간 알림 및 스트리밍 처리

### Kafka Connect와 Kafka Streams의 차이점은?

- Kafka Connect: 외부 시스템과 Kafka 간 연동 (데이터 수집/적재)
- Kafka Streams: Kafka 내부 스트림 처리 라이브러리

### Kafka 도입 시 고려한 점은?

- 메시지 유실 가능성
- Consumer Group 설계 및 파티셔닝 전략
- 클러스터 크기, 장애 복구, 모니터링 체계 구축

## ✅ 고급/기타

### Controller의 역할은?

- 리더 선출, 토픽 메타데이터 관리 등 클러스터의 중심 역할

### ZooKeeper는 왜 필요한가요?

- Kafka 2.x까지 클러스터 메타데이터와 컨트롤러 선출을 담당
- Kafka 3.x부터는 ZooKeeper 없는 KRaft 모드 지원

### ISR(In-Sync Replicas)이란?

- 리더와 동일한 데이터 상태를 유지하는 팔로워 집합
- 안정성 확보를 위한 필수 요소

### Kafka와 Flink/Spark Streaming 연동 경험이 있나요?

- Flink: 실시간 분석 후 데이터 저장
- Spark Streaming: 배치성 스트리밍 처리 구축
