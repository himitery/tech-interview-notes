# Data Structures

## ✅ Redis가 지원하는 자료구조

Redis는 단순한 Key-Value 저장소를 넘어, 다양한 고수준 자료구조를 지원합니다.  
각 자료구조는 시간복잡도와 특성이 다르며, **상황에 따라 적절한 구조를 선택하는 것이 성능과 효율성에 매우 중요**합니다.

## ✅ 자료구조 요약

| 자료구조        | 주요 명령어                        | 사용 예시                  | 특징 및 시간복잡도  |
| --------------- | ---------------------------------- | -------------------------- | ------------------- |
| **String**      | `SET`, `GET`, `INCR`, `APPEND`     | 기본 캐시, 카운터 등       | O(1)                |
| **List**        | `LPUSH`, `RPUSH`, `LPOP`, `LRANGE` | 작업 큐, 채팅 메시지       | O(1) or O(n)        |
| **Set**         | `SADD`, `SREM`, `SISMEMBER`        | 태그, 유니크 리스트        | 평균 O(1)           |
| **Hash**        | `HSET`, `HGET`, `HDEL`             | 객체 저장 (예: 유저 정보)  | O(1)                |
| **Sorted Set**  | `ZADD`, `ZRANGE`, `ZINCRBY`        | 랭킹, 점수 기반 정렬       | O(log n)            |
| **Bitmap**      | `SETBIT`, `GETBIT`, `BITCOUNT`     | 출석 체크, 플래그 저장     | 비트 단위 처리      |
| **HyperLogLog** | `PFADD`, `PFCOUNT`                 | 유니크 카운팅(대용량 추정) | O(1) (근사치 제공)  |
| **Stream**      | `XADD`, `XREADGROUP`, `XACK`       | 로그 수집, 메시지 큐       | 메시지 기반 처리    |
| **Geo**         | `GEOADD`, `GEORADIUS`              | 위치 기반 서비스           | 위치 기반 정렬/검색 |

## ✅ 자료구조별 상세 설명

### 1. String

- 기본적인 Key-Value 저장 방식
- 숫자, 문자열 모두 저장 가능
- 간단한 카운팅, 플래그, JSON 문자열 저장 등에 활용

```bash
SET user:1:name "Alice"
GET user:1:name
INCR visit_count
```

### 2. List

- 양쪽에서 삽입/삭제 가능한 연결 리스트
- 주로 작업 큐(Queue), 채팅 메시지 저장 등에 사용

```bash
LPUSH queue "task1"
RPUSH queue "task2"
LPOP queue
```

### 3. Set

- 중복 없는 값들의 집합
- 태그 관리, 유저 아이디 저장 등 유니크 값 관리에 적합

```bash
SADD tags "java"
SADD tags "spring"
SISMEMBER tags "java"
```

### 4. Hash

- 필드-값 쌍을 가지는 해시 테이블 (작은 JSON 객체처럼 사용)
- 유저 정보, 설정값 저장 등 객체 단위 저장에 유리

```bash
HSET user:1 name "Alice"
HSET user:1 age 30
HGET user:1 name
```

### 5. Sorted Set (ZSet)

- 점수(score)를 기준으로 정렬된 Set
- 실시간 랭킹 시스템, 우선순위 큐 등에 사용

```bash
ZADD rank 100 "user1"
ZADD rank 200 "user2"
ZRANGE rank 0 -1 WITHSCORES
```

### 6. Bitmap

- 비트를 직접 조작하여 상태 저장 (출석, 상태 체크 등)
- 비트 단위이므로 공간 효율이 높음

```bash
SETBIT user:1:attendance 1 1
GETBIT user:1:attendance 1
```

### 7. HyperLogLog

- 정확한 수가 아닌 **근사치(unique count)** 를 제공
- 수백만 유저를 대상으로 유니크 방문자 수 추정 가능

```bash
PFADD visitors user1 user2 user3
PFCOUNT visitors
```

### 8. Stream

- 시간순 메시지 저장 구조
- Pub/Sub보다 안정적이고 영속적인 메시지 소비 지원

```bash
XADD mystream * user "Alice" message "Hello"
XREAD COUNT 2 STREAMS mystream 0
```

### 9. Geo

- 위도/경도 좌표 저장 및 반경 검색 가능
- 배달, 택시 등 위치 기반 서비스에서 활용

```bash
GEOADD locations 127.0 37.5 "store1"
GEORADIUS locations 127.0 37.5 5 km
```

## ✅ 실무에서의 구조 선택 팁

| 상황                 | 추천 자료구조 | 이유                                  |
| -------------------- | ------------- | ------------------------------------- |
| 게시글 좋아요 카운트 | String        | 단순 증가/감소 연산                   |
| 실시간 대기열 처리   | List          | FIFO 구조, 작업 순서 보장             |
| 유저 정보 저장       | Hash          | 필드 단위 접근, 유저 객체 구조와 유사 |
| 태그 관리            | Set           | 중복 없는 집합                        |
| 사용자 랭킹          | Sorted Set    | 점수 기반 정렬                        |
| 하루 출석 체크       | Bitmap        | 비트 연산으로 공간 절약               |
| UV 측정 (대용량)     | HyperLogLog   | 메모리 절약 + 근사치 계산             |
| 실시간 로그 수집     | Stream        | 안정적 메시지 저장 + 소비             |
