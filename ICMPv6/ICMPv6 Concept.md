## 1. ICMPv6의 역할

IPv6에서 ICMPv6는 **선택이 아닌 필수 코어 프로토콜**이다.

IPv4 시절에는  
- ICMP → 오류 보고 / 진단  
- ARP → MAC 주소 탐색  
- 제어 기능이 여러 프로토콜로 분산되어 있었다.

IPv6에서는  
- **ICMPv6 하나로 통합**되어 아래 기능들을 담당한다.

기능:  
- 오류 보고  
- 네트워크 진단 (`ping`, `traceroute`)  
- **PMTUD (Path MTU Discovery)**  
- **NDP 수행 (ARP 완전 대체)**  
- **라우터 탐색 및 자동 주소 구성(SLAAC)**  

> ICMPv6 차단 시 IPv6 네트워크는 정상적으로 작동하지 않는다.

## 2. ICMPv6 패킷 구조
IPv6의 상위 프로토콜이며,  
IPv6 Header의 **Next Header 값 = 58**일 때 ICMPv6임을 의미한다.

[ IPv6 Header ]  
 └─ Next Header = 58 (ICMPv6)  
      ↓  
[ ICMPv6 Header ]  
 ├ Type (8bit)   → 메시지 종류  
 ├ Code (8bit)   → 세부 원인  
 ├ Checksum (16bit)  
 └ Message Body (가변)

**Type** = 메시지 대분류 ID  
**Code** = Type 내부 세부 구분

## 3. ICMPv6 메시지 분류
ICMPv6는 크게 두 부류로 나뉜다.
1) **Error Messages (오류 메시지)**  
2) **Informational Messages (정보 / 제어 메시지)**  

## 4. Error Messages (오류 메시지)
| Type | 이름 |
|------|------|
| 1 | Destination Unreachable |
| 2 | Packet Too Big |
| 3 | Time Exceeded |
| 4 | Parameter Problem |

### Type 1 — Destination Unreachable
목적지에 도달할 수 없을 때 전송된다.  
원인:  
- 라우팅 테이블 미존재  
- 방화벽(ACL) 차단  
- 목적지 IPv6 주소 미할당  

### Type 2 — Packet Too Big
IPv6 라우터는 **Fragment 불가**  
MTU 초과 시 **ICMPv6 Type 2 메시지 전송**  
송신자는 MTU 크기를 줄이고 재전송 → **PMTUD 작동**

### Type 3 — Time Exceeded
Hop Limit 초과 시 발생  
→ Traceroute의 핵심 원리  
IPv4의 TTL과 동일 개념

### Type 4 — Parameter Problem
IPv6 헤더 또는 확장 헤더의 문법 오류

## 5. Informational Messages

### Echo (Ping)
| Type | 기능 |
|------|------|
| 128 | Echo Request |
| 129 | Echo Reply |

사용 예:  
ping -6 google.com  
ping ::1  

### NDP 관련 메시지
| Type | 이름 |
|------|------|
| 133 | Router Solicitation (RS) |
| 134 | Router Advertisement (RA) |
| 135 | Neighbor Solicitation (NS) |
| 136 | Neighbor Advertisement (NA) |
| 137 | Redirect |

## 6. NDP (Neighbor Discovery Protocol)

IPv6에서 **ARP를 완전히 대체**하는 프로토콜로, **ICMPv6 기반**으로 동작한다.

기능:  
- IPv6 ↔ MAC 주소 매핑  
- 라우터 탐색  
- 이웃 노드 탐지  
- 자동 주소 구성 (SLAAC)  
- 중복 주소 검사 (DAD)  
- 최적 경로 통지 (Redirect)

### NDP 동작 과정
1. Host → RS (ff02::2)  
2. Router → RA (Prefix, Gateway 정보 제공)  
3. Host → SLAAC 주소 생성  
4. Host → NS (DAD 검사)  
5. 충돌 없으면 IPv6 주소 활성화


## 7. SLAAC 와 ICMPv6

IPv6 자동 주소 설정 절차 전체가 ICMPv6 기반으로 이루어진다.

RA 수신 → Prefix 획득 → Interface ID 생성 → IPv6 주소 조합 → NS 전송(DAD 수행) → 중복 없으면 사용


## 8. 보안 관점의 주요 공격

- **RA Spoofing** → 위조 Router Advertisement로 Gateway 속이기 (MITM)  
- **NDP Spoofing** → ARP Spoof와 동일 개념 (NS/NA 위조)  
- **SLAAC Spoofing** → 가짜 Prefix 광고, 트래픽 Redirect  
- **ICMPv6 Flood** → Echo/RA 대량 송신으로 DoS 유발  
- **IPv6 방화벽 누락** → IPv4만 정책 설정 시 우회 가능
