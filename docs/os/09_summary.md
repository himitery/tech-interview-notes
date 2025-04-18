# OS Summary

운영체제(Operating System, OS)는 하드웨어와 사용자 사이에서 중재자 역할을 하며, 시스템 자원을 효율적으로 관리하고 응용 프로그램이 실행될 수 있는 환경을 제공합니다.

### 1. 프로세스 및 스레드

- 프로세스: 실행 중인 프로그램의 인스턴스
- 스레드: 프로세스 내 실행 단위
- 문맥 교환(Context Switching)을 통해 CPU 자원 공유

### 2. 메모리 관리

- 가상 메모리: 논리 주소 → 물리 주소 변환 (MMU)
- 페이징, 세그멘테이션 기법으로 메모리 효율화
- TLB, 페이지 폴트, 교체 알고리즘 사용

### 3. CPU 스케줄링

- FCFS, SJF, Priority, Round Robin 등 알고리즘
- 선점형/비선점형 방식 존재
- 스케줄링 지표: 응답 시간, 대기 시간, 처리량 등

### 4. 동기화 및 데드락

- 공유 자원 접근 시 임계 구역 문제 발생
- 세마포어, 뮤텍스, 모니터로 해결
- 데드락 발생 조건: 상호 배제, 점유 대기, 비선점, 순환 대기

### 5. 파일 시스템 & I/O

- 연속/연결/인덱스 할당 방식
- Programmed I/O, Interrupt-driven I/O, DMA
- 캐시와 버퍼로 입출력 성능 보완

### 6. 가상화

- 하이퍼바이저 기반 VM vs OS 레벨 컨테이너
- 자원 활용도 향상 및 환경 격리 제공
- 성능/격리/이식성 측면에서 선택

### 7. 보안 및 보호

- 인증, 접근 제어, 시스템 보호 기법 적용
- 사용자/커널 모드, 권한 분리, ASLR 등 활용
- 최소 권한 원칙과 시스템 호출 모니터링 필요
