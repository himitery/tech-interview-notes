# Database Overview

## ✅ 데이터베이스란?

- 데이터를 체계적으로 저장하고 관리하는 시스템
- 다수의 사용자와 프로그램이 데이터를 공유하고 사용할 수 있도록 지원
- 파일 시스템과 달리 중복 최소화, 무결성 보장, 고급 질의 기능을 제공함

## ✅ DBMS(Database Management System)

- 데이터베이스를 정의, 생성, 조작, 제어할 수 있는 소프트웨어
- 사용자와 데이터베이스 사이에서 중재자 역할 수행
- 대표적인 DBMS: MySQL, PostgreSQL, Oracle, MS SQL Server 등

## ✅ RDBMS(Relational DBMS)

- 데이터를 행(Row)과 열(Column)로 구성된 테이블 형태로 저장
- SQL을 사용해 데이터를 조작
- 관계(Join)를 통해 데이터 간 연관성 표현
- 정규화를 통해 데이터의 중복을 최소화하고 무결성을 유지함

## ✅ SQL(Structured Query Language)

- 관계형 데이터베이스에서 데이터를 정의하고 조작하기 위한 표준 언어
- 주요 구문: SELECT, INSERT, UPDATE, DELETE, CREATE, DROP 등

## ✅ 데이터베이스 설계 핵심 개념

- **스키마(Schema)**: 데이터베이스 구조를 정의하는 틀 (테이블, 뷰, 인덱스 등)
- **정규화(Normalization)**: 데이터 중복 제거, 이상현상 방지를 위한 설계 기법
- **반정규화(Denormalization)**: 성능 개선을 위해 정규화를 일부 해제하는 설계
- **ER 모델**: 개체(Entity), 속성(Attribute), 관계(Relationship)을 시각적으로 표현

## ✅ 트랜잭션과 동시성 제어

- **트랜잭션(Transaction)**: 하나의 논리적 작업 단위, 모두 성공하거나 모두 실패해야 함
- **ACID**: 트랜잭션의 4가지 성질 (Atomicity, Consistency, Isolation, Durability)
- **격리 수준(Isolation Level)**: 동시에 실행되는 트랜잭션 간 충돌 방지를 위한 설정

## ✅ 인덱스와 성능

- **인덱스(Index)**: 검색 성능을 향상시키기 위한 자료구조 (B+Tree, Hash 등)
- **커버링 인덱스**: 인덱스만으로 쿼리 결과를 반환할 수 있어 테이블 접근 불필요
- **전문 검색**: Full Text Index / Inverted Index를 사용한 자연어 기반 검색

## ✅ 대용량 데이터 대응

- **파티셔닝(Partitioning)**: 하나의 테이블을 여러 물리적 파티션으로 분할
- **샤딩(Sharding)**: 데이터베이스를 수평으로 분산하여 확장성 확보
- **분산 DB**: 여러 노드에 데이터를 나눠 저장하고 처리하는 구조
- Hadoop, Spark, Kafka 등을 연계하여 배치, 실시간 분석 환경을 구성하는 사례도 증가

## ✅ 실무 고려사항

- **슬로우 쿼리 분석**: slow query log, EXPLAIN, ANALYZE
- **조인 최적화**: 적절한 인덱스 사용, 조건 순서 조정
- **정합성 유지**: 트랜잭션, 제약조건(FK, UNIQUE), CASCADE 설정 등
- **백업 및 복구**: 장애 대비를 위한 정기적인 백업과 복구 계획 수립
