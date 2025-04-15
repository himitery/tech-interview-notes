# Caching Strategies

## ✅ Redis를 캐시로 사용하는 이유

- 빠른 응답 속도 (ms 단위)
- 백엔드 DB 부하 감소
- 반복적인 요청에 대한 효율적 응답

Redis는 다양한 **캐시 전략**(Cache Strategy)을 구현할 수 있도록 유연한 구조를 제공합니다.

## ✅ 캐싱 전략 비교

| 전략 이름     | 개념 설명                                          | 장점                     | 단점                          |
| ------------- | -------------------------------------------------- | ------------------------ | ----------------------------- |
| Cache Aside   | DB에서 직접 데이터를 읽고, 캐시에 저장 (Lazy Load) | 구현 쉬움, TTL 설정 유연 | 캐시 미스 시 지연 발생        |
| Write Through | 쓰기 요청이 오면 캐시와 DB 모두에 쓰기             | 정합성 높음              | 쓰기 속도 느려질 수 있음      |
| Write Back    | 쓰기 요청은 캐시에만 하고, 주기적으로 DB에 반영    | 빠른 응답 속도           | 캐시 유실 시 데이터 손실 위험 |

## ✅ Cache Aside 예시 (가장 일반적인 패턴)

```python
# 조회 시
if cache.exists(key):
    return cache.get(key)
else:
    data = db.query(key)
    cache.set(key, data, ttl=60)
    return data

# 쓰기 시
update_db(key, new_value)
cache.delete(key)  # 캐시 무효화
```

> [!NOTE]
> 일반적으로 조회가 많은 서비스에 적합. TTL과 함께 사용하면 효과적

## ✅ Write Through 예시

```python
def write_through(key, value):
    db.write(key, value)
    cache.set(key, value)
```

- 쓰기 요청마다 캐시와 DB 동시 갱신
- 주로 키-값 변경이 잦은 시스템에서 사용

## ✅ Write Back 예시

```python
def write_back(key, value):
    cache.set(key, value)
    queue.append((key, value))  # 비동기 DB 저장

# 백그라운드 작업
while queue:
    key, value = queue.pop()
    db.write(key, value)
```

> [!WARNING]
> 장애 시 데이터 유실 위험이 있어 복구 전략이 필요

## ✅ 캐시 정합성 전략

- **TTL 기반 무효화**: 일정 시간 후 자동 삭제
- **쓰기 시 캐시 삭제**: 쓰기 연산 후 캐시 제거(Cache Aside)
- **버전 관리**: 데이터 버전 변경 시 캐시 갱신

## ✅ 실무 적용 팁

| 상황                              | 추천 전략              |
| --------------------------------- | ---------------------- |
| 조회가 빈번하고 쓰기가 적음       | Cache Aside            |
| 읽기와 쓰기가 균형 잡힌 경우      | Write Through          |
| 매우 빠른 쓰기 응답이 필요한 경우 | Write Back + 복구 전략 |

- TTL은 요청 주기와 정합성 요구 수준에 따라 조정
- 캐시 크기 초과 시 Eviction 정책도 고려해야 함 (LRU, LFU 등)
