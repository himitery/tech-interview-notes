# Full Text Search & Inverted Index

## ✅ Full Text Search란?

- 문자열 전체를 대상으로 검색하는 기능
- 부분 일치, 자연어 검색, 유사도 기반 검색 등을 지원
- 일반적인 SQL의 `LIKE` 연산보다 훨씬 강력한 검색 기능 제공

## ✅ Inverted Index란?

- **단어를 키로, 해당 단어가 포함된 문서 ID 리스트를 값으로 가지는 구조**
- 도서관의 색인(Index) 구조와 유사
- 검색 엔진(예: Elasticsearch)에서 사용되는 핵심 자료구조

### 예시

| 문서 ID | 내용                   |
| ------- | ---------------------- |
| 1       | apple banana carrot    |
| 2       | banana durian eggplant |

→ Inverted Index 구조

```
apple    → [1]
banana   → [1, 2]
carrot   → [1]
durian   → [2]
eggplant → [2]
```

→ "banana" 검색 시 → [1, 2] 반환

## ✅ Inverted Index vs B+ Tree

| 항목          | Inverted Index               | B+ Tree Index                    |
| ------------- | ---------------------------- | -------------------------------- |
| 용도          | 전문 검색 (Full Text Search) | 정렬, 범위 검색 등 일반 인덱스용 |
| 검색 속도     | 단어 단위로 매우 빠름        | 키 단위 비교 → 상대적으로 느림   |
| 저장 구조     | 단어 → 문서 리스트 (역방향)  | 키 → 레코드 위치 (순방향)        |
| 정렬/범위검색 | 어려움                       | 매우 효율적                      |

## ✅ MATCH AGAINST (MySQL 예시)

```sql
SELECT * FROM articles
WHERE MATCH(title, body) AGAINST('database');
```

- `FULLTEXT INDEX`가 필요한 컬럼에서만 사용 가능
- Boolean Mode, Natural Language Mode 등 다양한 옵션 지원

## ✅ 실무에서의 고려사항

- 정규화된 데이터보다는 **비정형 텍스트** 검색에 효과적
- 결과 정확도 향상을 위한 **형태소 분석, 불용어 제거, 유사어 처리**가 중요함
- 검색 정확도/속도 튜닝이 중요한 요소이며, 필요시 별도의 검색 엔진 도입 고려 (ex. Elasticsearch)
- 동의어 사전(synonym dictionary)을 구축해 유사 검색 범위를 확장할 수 있음

## ✅ 검색 엔진 비교 (MySQL vs Elasticsearch)

| 항목        | MySQL FullText   | Elasticsearch                       |
| ----------- | ---------------- | ----------------------------------- |
| 색인 방식   | Inverted Index   | Inverted Index                      |
| 언어 분석기 | 기본 지원 한정적 | 다양한 언어 지원                    |
| 분산 처리   | 지원 안 됨       | 고성능 분산 검색 지원               |
| 실시간 검색 | 다소 느림        | 실시간 색인/검색 가능               |
| 복잡한 쿼리 | 어려움           | DSL(Query DSL)로 유연하게 작성 가능 |

## ✅ 면접 포인트

- Inverted Index가 어떻게 동작하는지 설명할 수 있어야 함
- 일반 인덱스(B+ Tree)와 어떤 차이가 있는지 비교할 수 있어야 함
- MySQL에서 FullText Search를 위한 구성 요소 (FULLTEXT INDEX, MATCH AGAINST 등)를 이해하고 있어야 함
