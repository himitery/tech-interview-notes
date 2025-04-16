# OSI 7 계층

## ✅ OSI 7계층이란?

OSI(Open Systems Interconnection) 모델은 통신 시스템을 7개의 계층으로 나누어 각 계층이 수행하는 역할을 표준화한 모델입니다. 각 계층은 상하위 계층과 명확히 분리되어 독립적으로 설계될 수 있도록 합니다.

## ✅ 계층별 설명

### 1. 물리 계층 (Physical Layer)

- 실제 비트 단위 전송 담당
- 전기적/기계적 신호로 변환
- 케이블, 허브, 리피터

### 2. 데이터 링크 계층 (Data Link Layer)

- MAC 주소 기반의 물리적 주소 지정
- 프레임 단위 전송, 오류 감지/수정
- Ethernet, PPP, Switch

### 3. 네트워크 계층 (Network Layer)

- 패킷의 라우팅 및 논리 주소(IP) 지정
- 서로 다른 네트워크 간의 통신 담당
- IP, ICMP, IGMP, ARP

### 4. 전송 계층 (Transport Layer)

- 종단 간(end-to-end) 데이터 전송 보장
- 오류 검출, 흐름 제어, 혼잡 제어 제공
- TCP, UDP

### 5. 세션 계층 (Session Layer)

- 세션의 설정, 유지, 종료 관리
- 동기화, 체크포인트, 재전송 관리
- API 통신, 로그인 세션 등

### 6. 표현 계층 (Presentation Layer)

- 데이터의 표현 형식을 담당 (인코딩/디코딩, 암호화/복호화)
- 서로 다른 시스템 간 데이터 호환 보장
- JPEG, MPEG, ASCII, TLS 등

### 7. 응용 계층 (Application Layer)

- 사용자와 가장 가까운 계층
- 응용 프로그램이 네트워크에 접근할 수 있도록 지원
- HTTP, FTP, SMTP, DNS 등 동작

## ✅ OSI vs TCP/IP 모델 비교

| OSI 계층         | TCP/IP 계층        | 주요 프로토콜 예시            |
| ---------------- | ------------------ | ----------------------------- |
| 응용, 표현, 세션 | 응용 계층          | HTTP, DNS, FTP, TLS           |
| 전송 계층        | 전송 계층          | TCP, UDP                      |
| 네트워크 계층    | 인터넷 계층        | IP, ICMP, ARP                 |
| 데이터링크, 물리 | 네트워크 접근 계층 | Ethernet, Wi-Fi, MAC, CSMA/CD |
