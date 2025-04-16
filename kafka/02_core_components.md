# Core Components

## ✅ Producer

Producer는 Kafka로 데이터를 전송하는 주체로, 애플리케이션 또는 서비스에서 발생한 이벤트를 Topic에 발행한다.
전송할 메시지를 어떤 Topic의 어떤 Partition에 보낼지 결정할 수 있으며, 기본적으로 Round-Robin 방식이나 Key 기반 파티셔닝을 지원한다.

### 주요 설정

- `acks`: 전송 확인 방식 (0, 1, all)
- `key.serializer`, `value.serializer`: 메시지를 직렬화하는 방식
- `retries`, `linger.ms`, `batch.size`: 성능 및 안정성 설정

## ✅ Consumer

Consumer는 Kafka에서 메시지를 구독(consume)하는 클라이언트로, Topic의 Partition으로부터 데이터를 읽는다.
여러 Consumer가 하나의 그룹(Consumer Group)으로 묶이면, 파티션 단위로 메시지를 나누어 읽을 수 있다.

### 주요 개념

- **Offset**: Consumer가 읽은 메시지의 위치
- **Auto Commit** vs **Manual Commit**: 오프셋을 자동으로 저장할지 수동으로 제어할지 여부
- **Rebalancing**: Consumer Group의 멤버가 변경되었을 때 Partition을 재할당하는 과정

## ✅ Topic & Partition

- **Topic**: 메시지를 분류하는 논리적 단위이며, 다수의 Producer와 Consumer가 하나의 Topic을 통해 통신할 수 있다.
- **Partition**: Topic을 나눈 물리적 단위로, 병렬 처리와 스케일 아웃을 가능하게 한다.
  각 Partition은 메시지를 순차적으로 저장하고, 각 메시지는 고유 Offset을 가진다.

## ✅ Broker

Broker는 Kafka 서버 인스턴스로, 메시지를 저장하고 클라이언트 요청을 처리한다. 하나의 Kafka 클러스터는 여러 Broker로 구성되며, 각 Broker는 특정 Partition의 리더 또는 팔로워가 될 수 있다.

### 기능

- 메시지 저장 및 전달
- Producer/Consumer 요청 처리
- 리더 선출 및 메타데이터 관리 (Controller 역할 포함 가능)

## ✅ Zookeeper (Kafka 2.x까지)

Zookeeper는 Kafka 클러스터의 메타데이터와 상태를 관리하는 외부 시스템이다. Kafka 3.x부터는 Zookeeper 없는 구조(KRaft)도 도입되었지만, 기존에는 컨트롤러 선출과 브로커 상태 동기화 등에 필수적이었다.

### 역할

- Controller 선출
- Broker 상태 감시
- 토픽/파티션 메타데이터 저장

## ✅ Controller

Kafka 클러스터 내에서 리더 파티션 선출, 파티션 재할당 등의 클러스터 관리 작업을 담당하는 특별한 Broker.
Zookeeper가 존재하는 경우, ZK가 Controller를 선출한다. 하나의 클러스터에는 항상 하나의 Controller가 존재해야 한다.

### 기능

- Partition 리더 선출
- ISR 관리
- 클러스터 메타데이터 업데이트
