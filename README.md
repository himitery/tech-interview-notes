![image](images/image.png)

기술 면접을 준비하며 정리한 개념들을 주제별로 모아, 실무와 이론을 자연스럽게 연결해 이해할 수 있도록 구성했어요.

## Index

### ⭐️ Network

1. [Questions](./docs/network/00_questions.md) : 네트워크 면접에서 자주 나오는 질문과 개념을 정리했어요.
2. [Overview](./docs/network/01_overview.md) : 네트워크의 정의와 필요성을 간략하게 설명했어요.
3. [OSI 7 Layer](./docs/network/02_osi_model.md) : OSI 7계층의 구조와 각 계층의 역할을 정리했어요.
4. [TCP/IP Layer](./docs/network/03_tcp_ip.md) : 실제 인터넷에서 사용되는 TCP/IP 4계층 모델을 설명했어요.
5. [HTTP](./docs/network/04_http.md) : 요청/응답 구조와 HTTP 버전별 차이점을 다뤘어요.
6. [TLS](./docs/network/05_tls.md) : HTTPS를 구성하는 TLS 프로토콜의 흐름과 기능을 설명했어요.
7. [Transport Layer (TCP/UDP)](./docs/network/06_transport.md) : TCP와 UDP의 차이, handshake, 상태 다이어그램 등을 정리했어요.
8. [DNS](./docs/network/07_dns.md) : 도메인을 IP 주소로 변환하는 DNS의 구조와 질의 과정을 설명했어요.
9. [Network Tools](./docs/network/08_network_tools.md) : ping, traceroute, dig 등 실무에서 자주 쓰는 도구들을 정리했어요.
10. [Summary](./docs/network/09_summary.md) : 네트워크 개념을 한눈에 복습할 수 있도록 요약했어요.

### ⭐️ Operating System

1. [Questions](./docs/os/00_questions.md) : 면접에서 자주 나오는 운영체제 질문들을 정리했어요.
2. [Overview](./docs/os/01_overview.md) : 운영체제의 정의와 핵심 역할을 설명했어요.
3. [Process & Thread](./docs/os/02_process_and_thread.md) : 프로세스/스레드 구조, 문맥 교환 등을 정리했어요.
4. [Memory Management](./docs/os/03_memory_management.md) : 가상 메모리, 페이징, 스와핑 등을 정리했어요.
5. [CPU Scheduling](./docs/os/04_cpu_scheduling.md) : 주요 스케줄링 알고리즘과 성능 지표를 정리했어요.
6. [Synchronization](./docs/os/05_synchronization.md) : 임계 구역, 세마포어, 데드락을 자세히 설명했어요.
7. [File & I/O](./docs/os/06_file_and_io.md) : 파일 시스템 구조와 I/O 처리 방식을 다뤘어요.
8. [Virtualization](./docs/os/07_virtualization.md) : 하이퍼바이저와 컨테이너 기반 가상화를 비교했어요.
9. [Security & Protection](./docs/os/08_security_and_protection.md) : 인증, 접근 제어, 보호 기법 등을 설명했어요.
10. [Summary](./docs/os/09_summary.md) : 운영체제 전체 내용을 핵심만 모아 요약했어요.

### 💎 Kafka

1. [Questions](./docs/kafka/00_questions.md) : Kafka 면접에서 자주 나오는 질문과 핵심 내용을 정리했어요.
2. [Overview](./docs/kafka/01_overview.md) : Kafka의 개념, 구조, 주요 특징을 설명했어요.
3. [Core Components](./docs/kafka/02_core_components.md) : Producer, Consumer, Broker 등 핵심 구성 요소를 다뤘어요.
4. [Message Flow](./docs/kafka/03_message_flow.md) : 메시지 저장 구조와 전달 보장 방식에 대해 설명했어요.
5. [Operations](./docs/kafka/04_operations.md) : Replication, Retention, 장애 대응, 모니터링 전략을 다뤘어요.
6. [Usecases](./docs/kafka/05_usecases.md) : Kafka의 실무 적용 사례를 다양한 패턴으로 정리했어요.
7. [Troubleshooting](./docs/kafka/06_troubleshooting.md) : Kafka 운영 중 자주 발생하는 문제와 해결 방법을 정리했어요.
8. [Spring Integration](./docs/kafka/07_spring_integration.md) : Spring Boot에서 Kafka를 연동할 때의 구조와 실무 팁을 정리했어요.

### 💎 Redis

1. [Questions](./docs/redis/00_questions.md) : 면접에서 자주 나오는 질문과 답변을 다뤘어요.
2. [Overview](./docs/redis/01_overview.md) : Redis의 개념, 특징, 장단점, Memcached와의 비교를 다뤘어요.
3. [Data Structure](./docs/redis/02_data_structures.md) : 지원하는 자료구조와 사용 예시를 다뤘어요.
4. [Memory & Persistence](./docs/redis/03_memory_and_persistence.md) : RDB, AOF, TTL, Eviction 정책을 다뤘어요.
5. [Pub/Sub & Stream](./docs/redis/04_pubsub_stream.md) : 메시징 구조와 Kafka와의 비교를 다뤘어요.
6. [Transaction & Lua & Distributed Lock](./docs/redis/05_transaction_lua_lock.md) : 트랜잭션, Lua, 분산 락을 다뤘어요.
7. [Cluster & Sentinel](./docs/redis/06_cluster_and_sentinel.md) : 고가용성 구성과 샤딩 전략을 다뤘어요.
8. [Cache Strategies](./docs/redis/07_cache_strategies.md) : Cache Aside, Write Through, Write Back 전략을 다뤘어요.
9. [Performance](./docs/redis/08_performance.md) : Pipeline, Lua 최적화, slowlog 등 성능 관련 내용을 다뤘어요.
10. [Usecases](./docs/redis/09_usecases.md) : 세션, 랭킹, 메시지 큐 등 실무 사례를 다뤘어요.
11. [Troubleshooting](./docs/redis/10_troubleshooting.md) : Redis 운영 중 발생할 수 있는 문제와 해결 방법을 다뤘어요.

## Contribute

새로운 내용을 추가하거나, 기존 내용을 수정하고 싶다면 언제든지 PR을 보내주세요!

👉 [PR 작성하러 가기](https://github.com/himitery/tech-interview-notes/compare)
