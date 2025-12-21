# 1. DNS 개요

→ **DNS는 사람이 이해하기 쉬운 도메인 이름을 IP 주소로 변환해주는 분산형 이름 해석 시스템이다.**

- 역할: **이름(Name) → IP 주소 변환**
- 분류: **애플리케이션 계층 프로토콜**
- 구조: **분산 데이터베이스**
- 인터넷 동작의 필수 기반 기술
- 사람이 URL을 쓰게 해주는 핵심 시스템
- 중앙 서버가 아닌 **계층적 구조**

---

# 2. DNS 동작 목적

- 사람은 **google.com**을 기억
- 네트워크는 **142.250.206.14** 같은 IP만 이해
→ DNS가 중간에서 **번역기 역할** 수행

---

# 3. DNS 동작 과정

1. 사용자가 브라우저에 **도메인 입력**
2. **로컬 DNS Resolver**에게 질의
3. 캐시 확인
   - 있으면 → 바로 응답
   - 없으면 → 상위 DNS로 질의
4. **Root DNS Server** 질의
5. **TLD DNS Server(.com, .net 등)** 질의
6. **Authoritative DNS Server** 질의
7. 최종 IP 주소 응답 수신
8. Resolver가 클라이언트에게 IP 전달
9. 브라우저가 해당 IP로 서버 접속

---

# 4. DNS 계층 구조

## ■ Root DNS Server
- DNS 최상위 서버
- “.com은 어디에 있냐?”를 알려줌
- 전 세계에 13개 논리적 루트 서버 존재

## ■ TLD DNS Server
- 최상위 도메인 담당
- 예: .com / .org / .kr
- 해당 도메인의 Authoritative 서버 위치 제공

## ■ Authoritative DNS Server
- **실제 도메인의 IP 정보를 보유**
- 최종 답변 제공자
- DNS의 “정답지”

---

# 5. DNS 질의 방식

## ■ Recursive Query
- 클라이언트 → Resolver
- “최종 답을 가져와라”
- 대부분의 사용자 요청 방식

## ■ Iterative Query
- Resolver ↔ DNS 서버들
- 단계별로 다음 서버 위치를 안내받음

---

# 6. DNS 레코드 종류

| 타입 | 이름 | 설명 |
|----|----|----|
| **A** | Address | 도메인 → IPv4 |
| **AAAA** | IPv6 Address | 도메인 → IPv6 |
| **CNAME** | Canonical Name | 별칭 설정 |
| **NS** | Name Server | 도메인 관리 서버 |
| **MX** | Mail Exchange | 메일 서버 |
| **TXT** | Text | 인증, 보안 설정 |
| **PTR** | Pointer | IP → 도메인 (역방향) |

---

# 7. DNS 패킷 / 전송 특징

- 전송 계층: **UDP / TCP**
- 기본 포트: **UDP 53**
- TCP 사용 경우:
  - Zone Transfer
  - 응답 데이터가 큰 경우
- 빠른 응답을 위해 UDP 기본 사용

---

# 8. DNS 캐싱(Cache)

- Resolver는 응답을 **TTL** 동안 캐싱
- 장점:
  - 응답 속도 향상
  - DNS 트래픽 감소
- 단점:
  - 변경 사항 반영이 지연될 수 있음

---

# 9. DNS 핵심 특징 요약

- Application Layer 프로토콜
- 도메인 이름 → IP 변환
- 계층적 분산 구조
- UDP 53 기본 사용
- 캐싱 기반 고속 응답
- 인터넷 동작의 기반 인프라
