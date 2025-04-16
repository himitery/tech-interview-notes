# Kafka Integration with Spring Boot

## ✅ Spring Kafka 개요

Spring Kafka는 Kafka 클라이언트를 추상화하여 Spring 애플리케이션에 쉽게 통합할 수 있도록 지원하는 라이브러리다. 설정 중심의 개발 방식을 제공하며, Producer/Consumer를 어노테이션 기반으로 구현할 수 있다.

```java
@KafkaListener(topics = "orders", groupId = "order-group")
public void listen(String message) {
    // 메시지 처리 로직
}
```

## ✅ Consumer Thread 구조

Spring Kafka에서 Consumer는 내부적으로 **스레드 풀 기반**으로 동작한다.

- Kafka Listener Container는 하나 이상의 스레드를 생성하여 파티션을 처리한다.
- **기본적으로 Listener 하나당 하나의 스레드**가 생성된다.
- health check나 리밸런싱 감시 등을 위한 내부 관리 스레드도 항상 존재한다.

### 특징

- 컨슈머가 Topic에 바인딩되면 고정된 스레드가 생성되어 해당 파티션을 지속적으로 폴링함
- 컨슈머 수가 늘어나면 스레드도 1:1로 증가 (멀티 파티션 병렬 처리 가능)
- 각 컨슈머는 고유의 스레드에서 메시지를 처리하므로 Thread-safe 해야 함

## ✅ 병렬 처리 구조

`@KafkaListener`에 `concurrency` 옵션을 부여하면 하나의 Topic에 대해 여러 Consumer 인스턴스를 생성할 수 있다.

```java
@KafkaListener(topics = "orders", concurrency = "3", groupId = "order-group")
public void process(String message) {
    // 병렬 처리 로직
}
```

- 이 경우 각 인스턴스는 별도의 스레드에서 독립적으로 실행된다.
- 파티션 수보다 많은 concurrency를 설정해도 효과가 없으며, 파티션 수 이하로 설정하는 것이 일반적이다.

## ✅ 주의할 점

- Listener는 기본적으로 **동기 처리** 방식이며, 메시지 처리 중 예외가 발생하면 Retry 또는 DLT 설정이 필요함
- 메시지 처리 로직은 **스레드 안전(Thread-safe)** 해야 하며, 외부 자원 접근 시 주의 필요

## ✅ 실무 팁

- `KafkaListenerContainerFactory`를 커스터마이징하여 스레드 수, 에러 핸들링 전략, ack 모드를 조정할 수 있음
- Kafka Lag이 지속될 경우, 파티션 수 및 concurrency 수 재점검
- health check 및 리밸런싱 스레드는 컨슈머 수와 관계없이 항상 존재하므로, 최소한의 리소스를 보장함
