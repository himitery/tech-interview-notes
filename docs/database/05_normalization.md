# 정규화 (Normalization)

## ✅ 정규화란?

정규화(Normalization)는 관계형 데이터베이스에서 **데이터 중복을 줄이고 이상 현상을 방지하기 위해 테이블을 구조적으로 분해하는 과정**을 의미함. 이를 통해 데이터의 무결성과 일관성을 유지할 수 있음.

## ✅ 이상 현상(Anomaly)

정규화가 되지 않은 테이블에서는 다음과 같은 이상 현상이 발생할 수 있음:

- **삽입 이상**: 데이터를 삽입하려면 불필요한 다른 데이터를 함께 입력해야 하는 문제
- **삭제 이상**: 하나의 데이터를 삭제하면, 원치 않는 다른 정보도 함께 삭제되는 문제
- **갱신 이상**: 동일한 정보가 여러 곳에 중복되어 있어 수정 시 불일치가 발생하는 문제

## ✅ 정규화의 기본 개념 (원부이결다조)

- **원자성(Atomicity)**: 속성의 값은 더 이상 나눌 수 없는 원자값이어야 함
- **부분 함수 종속(Partial Dependency)**: 복합 키의 일부 속성에만 종속되는 속성
- **이행 함수 종속(Transitive Dependency)**: 기본 키가 아닌 속성이 또 다른 속성에 종속되는 관계
- **결정자 및 함수 종속(Function Dependency)**: A → B일 때, A가 결정자이며 B는 A에 종속됨
- **다치 종속(Multivalued Dependency)**: 하나의 속성이 둘 이상의 독립적인 속성 집합을 결정하는 경우
- **조인 종속(Join Dependency)**: 릴레이션이 여러 릴레이션으로 분해되었을 때, 조인하여 원래 릴레이션을 복원할 수 있어야 함

## ✅ 정규화 단계

### 비정규형 (UNF)

```
학생(학번, 이름, 수강과목1, 수강과목2)
```

- 컬럼에 여러 값이 존재함 → 원자성 위배

### 제1정규형 (1NF)

- 원자성 만족 → 반복 그룹 제거

```
학생(학번, 이름, 수강과목)
```

### 제2정규형 (2NF)

- 1NF + 부분 함수 종속 제거

```
학생(학번, 이름)
수강(학번, 수강과목)
```

### 제3정규형 (3NF)

- 2NF + 이행 함수 종속 제거

```
학생(학번, 학과코드)
학과(학과코드, 학과이름)
```

### BCNF

- 모든 결정자가 후보 키여야 함
- BCNF(보이스-코드 정규형)는 3.5정규형이라 불리며, 모든 결정자가 후보 키여야 한다는 더 엄격한 조건을 갖음

```
수강(학번, 과목코드)
강의(과목코드, 교수)
```

### 제4정규형 (4NF)

- 다치 종속 제거

```
강사_과목(강사명, 강의과목)
강사_기기(강사명, 사용기기)
```

### 제5정규형 (5NF)

- 조인 종속 제거

```
학생(이름, 과목, 교재)
→ 분해 후 복원 가능한 구조 유지
```

## ✅ 반정규화 (Denormalization)

- 성능 향상을 위해 정규화된 테이블을 다시 합치는 작업
- 조회 성능이 중요한 OLAP 환경에서 주로 사용됨

### 반정규화 적용 시점

- 과도한 조인으로 성능 저하가 발생하는 경우
- 데이터 변경 빈도가 낮고, 조회가 대부분인 경우

## ✅ 설계 원칙

- 초기에는 정규화를 통해 구조적 안정성과 무결성을 확보
- 성능 이슈 발생 시 반정규화를 적용하되, 데이터 중복과 무결성 이슈를 고려해야 함

## ✅ 면접 포인트

- 정규화 단계별 정의와 개념을 명확히 설명할 수 있어야 함
- 이상 현상이 발생하는 구조와 해결 방법을 예시와 함께 설명할 수 있어야 함
- 반정규화가 필요한 시점을 실무적 관점에서 설명할 수 있어야 함
