# 1. 주소 체계 차이

| 항목 | IPv4 | IPv6 |
|------|------|------|
| 주소 길이 | 32-bit | 128-bit |
| 표현 | 10진수 점 구분 | 16진수 콜론 구분 |
| 예시 | 192.168.1.1 | 2001:db8::1 |
| 주소 개수 | 약 43억 | 사실상 무한 |
| NAT 필요 | 필수 | 불필요 |
| 공인 IP | 매우 부족 | 모든 장비 직접 할당 |

IPv6 주소 타입
- Global Unicast : 공인 주소
- Link-Local : 로컬 자동 생성 주소 (fe80::)
- Unique Local : 사설 주소
- Multicast : 그룹 전송
- Anycast : 가장 가까운 단일 수신자

# 2. 헤더 구조 차이

IPv4 헤더
Version/IHL/TOS/Total Length
ID | Flags | Fragment Offset
TTL | Protocol | Checksum
Source IP
Destination IP
Options

IPv6 기본 헤더
Version | Traffic Class | Flow Label
Payload Length
Next Header
Hop Limit
Source IPv6
Destination IPv6

차이 요약
- IPv4 : 가변 길이, 체크섬 있음, 옵션 혼합
- IPv6 : 고정 40바이트, 체크섬 없음, 옵션은 Extension Header로 분리

# 3. 단편화 (Fragmentation)

IPv4
- 송신자와 모든 라우터가 단편화 가능
- DF 비트로 단편화 제어

IPv6
- **라우터 단편화 금지**
- 송신자만 Fragment Header로 분할
- 반드시 PMTUD 사용

# 4. 경로 MTU 탐색 (PMTUD)
```text
| 항목 | IPv4 | IPv6 |
|------|------|------|
| 필수 여부 | 선택 | **필수** |
| ICMP 메시지 | Type3 Code4 | ICMPv6 Packet Too Big |
| 실패 위험 | 있음 | 있음 |
```
# 5. 주소 할당 방식

IPv4
- DHCP 서버 필요
- 수동 설정 빈번

IPv6
- **SLAAC 완전 자동 설정**
- DHCPv6 선택적 사용

주소 생성 방식
Prefix + Interface ID(EUI-64)

# 6. ARP vs NDP

IPv4
- ARP 브로드캐스트 요청

IPv6
- **NDP (ICMPv6 기반)**
- 멀티캐스트 + 캐시

# 7. 브로드캐스트 & 멀티캐스트

| 항목 | IPv4 | IPv6 |
|------|------|------|
| Broadcast | O | ❌ 폐지 |
| Multicast | 선택 | **기본 필수** |

IPv6는 불필요한 Flood 제거 → 네트워크 효율 상승

# 8. 보안 구조

IPv4
- IPSec 옵션
- NAT, 브로드캐스트 공격 노출

IPv6
- **IPSec 표준 내장**
- 멀티캐스트
- IP 직접 식별 → 공격 추적 용이


# 9. NAT 구조


IPv4
- NAT 필수
- 포트 공유 복잡
- P2P 불리

IPv6
- NAT 불필요
- 모든 노드 직접 주소 보유
- P2P 완벽 지원

# 10. QoS 구조

IPv4 
- TOS 필드 활용

IPv6
- **Traffic Class + Flow Label**
- 스트림 단위 QoS 제어 가능

# 11. ICMP

IPv4
- ICMPv4

IPv6
- **ICMPv6**
- NDP, Router Advertisement, SLAAC 포함


# 12. 라우팅 효율

| 항목 | IPv4 | IPv6 |
|------|------|------|
| 주소 집약 | 불리 | 매우 유리 |
| 라우팅 규모 | 큼 | 작음 |
| 백본 효율 | 낮음 | 높음 |

# 13. 이동성

IPv4
- Mobile IP 비표준

IPv6
- Mobile IPv6 기본 표준


# 14. 터널링

IPv6 over IPv4
- ISATAP
- Teredo
- 6to4
- GRE

구조

[ IPv4 Header ]
[ IPv6 Packet ]


# 15. 성능 차이

IPv4
- NAT 처리 오버헤드 큼
- 브로드캐스트 부하

IPv6
- 헤더 처리 단순
- Flood 없음
- MTU 효율 상승


# 16. 방화벽 정책 차이

IPv4
- NAT 중심 방화벽

IPv6
- **주소 직접 필터링**
- 상태기반 필터링 최적화


# 17. 점검 & 스캐닝

IPv4
- 스캔 가능 범위 제한 → 스캔 용이

IPv6
- 공간 광대 → 무차별 스캔 사실상 불가능
- OSINT/패턴 공격 위주 전환


# 18. 블랙홀 문제

공통
- ICMP 차단 시 PMTUD 실패
- TCP 정체 발생

대응
- MSS Clamp
- PLPMTUD


# 19. 전송 계층 영향

TCP/UDP 상위 프로토콜은 동일
단 IPv6은:

- MTU 필수 규칙 따름
- 세션 안정성 증가


# 20. 실무 적용 차이

| 적용 영역 | IPv4 | IPv6 |
|-----------|------|------|
| VPN | NAT 극복 필요 | NAT 없음 |
| Cloud | NAT 필수 | 직접 연결 |
| P2P | 난이도 높음 | 자연 연결 |
| 보안 분석 | IP 추적 어려움 | 신뢰성 향상 |


# 21. 최종 비교 요약

IPv4
- 주소 고갈
- NAT 필수
- 브로드캐스트 존재
- 옵션 혼재 헤더
- Fragment router 가능

IPv6
- 주소 무한
- NAT 폐지
- 멀티캐스트 전용
- 확장헤더 구조
- Fragment 라우터 불가
