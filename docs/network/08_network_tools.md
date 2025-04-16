# Network Tools

## ✅ ping

- 목적: 대상 호스트와의 연결 가능 여부 확인
- 동작 원리: ICMP Echo Request → Echo Reply 수신
- 주요 옵션
  - `ping -c 3 example.com`: 3회만 시도
  - `ping -i 0.5`: 간격 설정 (초)

### 예시

```bash
ping google.com
```

## ✅ traceroute / tracert

- 목적: 목적지까지의 경로에 있는 라우터 IP 추적
- 동작 원리: TTL 값을 1부터 증가시키며 경유지 확인
- 윈도우: `tracert`, 리눅스/맥: `traceroute`

### 예시

```bash
traceroute example.com
tracert example.com
```

## ✅ dig / nslookup

- 목적: DNS 질의 결과 확인
- `dig`: 리눅스/맥에서 많이 사용 (정밀함)
- `nslookup`: 윈도우에서 사용 편리

### 예시

```bash
dig example.com
nslookup example.com
```

## ✅ netstat / ss

- 목적: 현재 연결된 네트워크 세션, 포트, 상태 조회
- `netstat`: 전통적인 명령어 (플랫폼별 호환성 좋음)
- `ss`: 리눅스 기반에서 더 빠르고 정확함

### 예시

```bash
netstat -tuln
ss -tuln
```

- `-t`: TCP
- `-u`: UDP
- `-l`: Listening 상태
- `-n`: 포트 번호 표시

## ✅ curl

- 목적: HTTP 요청 테스트 (GET, POST 등)
- 다양한 헤더, 바디, 인증 옵션 지원

### 예시

```bash
curl https://example.com
curl -X POST -d 'id=1' https://example.com
```

## ✅ 기타 도구

- `tcpdump`: 패킷 캡처 (Wireshark와 연계 사용)
- `nmap`: 포트 스캐닝, 보안 진단
- `ifconfig` / `ip addr`: 네트워크 인터페이스 정보
- `hostname -I`: 현재 장치의 IP 주소 확인

## ✅ 실무에서의 활용 팁

- 연결 문제 발생 시 ping → traceroute → curl 순으로 점검
- DNS 이슈는 dig/nslookup으로 빠르게 파악
- 외부 요청 로그가 없다면 netstat/ss로 서버 포트 상태 확인
- 스크립트 자동화 시 curl 활용 가능
