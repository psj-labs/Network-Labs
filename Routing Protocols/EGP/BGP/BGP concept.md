# 1. BGP 개요

→ **BGP(Border Gateway Protocol)는 서로 다른 자율 시스템(AS) 간에 경로를 교환하며, 정책(Policy) 기반으로 최적의 경로를 선택하는 EGP이다.**

- 라우팅 범위: **AS 간 라우팅 (Inter-AS)**
- 라우팅 방식: **Path-Vector**
- 메트릭: **정책 + 속성(Attribute) 기반**
- 인터넷의 핵심 라우팅 프로토콜
- 대규모 네트워크(ISP, 데이터센터)에 사용
- 링크 상태/거리 벡터가 아닌 **경로(Path) 중심**

---

# 2. BGP 동작 과정

1. **TCP 세션 수립 (3-Way Handshake)**
2. **OPEN 메시지 교환 → BGP Neighbor 형성**
3. **KEEPALIVE 메시지로 세션 유지**
4. **UPDATE 메시지로 경로 정보 교환**
5. 수신한 경로에 대해
   - 속성(Attribute)을 기준으로 정책 판단
   - 최적 경로 선택
6. 선택된 경로를 **BGP 테이블 → RIB → FIB**에 반영
7. 토폴로지 변경 발생 시
   - 변경된 경로만 선택적으로 전파

---

# 3. BGP 종류

## ■ eBGP (External BGP)
- **서로 다른 AS 간** 경로 교환
- AS 번호가 다름
- 기본 TTL = 1
- 인터넷 상에서 가장 일반적인 형태

## ■ iBGP (Internal BGP)
- **같은 AS 내부**에서 경로 공유
- AS 번호 동일
- Full-Mesh 필요(기본)
- Route Reflector / Confederation으로 확장 가능

---

# 4. BGP 메트릭 / 경로 선택 기준

BGP는 단순 숫자가 아닌 **속성(Attribute)** 조합으로 경로를 선택한다.

### 주요 경로 선택 순서(핵심만)

1. **Weight (Cisco 전용)** – 가장 우선
2. **Local Preference** – 높을수록 우선
3. **AS_PATH 길이** – 짧을수록 우선
4. **Origin Type** – IGP < EGP < Incomplete
5. **MED** – 낮을수록 우선
6. **eBGP > iBGP**
7. **IGP Cost** – 다음 홉까지 비용
8. **Router ID** – 최종 타이브레이커

---

# 5. BGP Path Attribute

## ■ Well-known Mandatory
- **AS_PATH**: 거쳐온 AS 목록 (루프 방지 핵심)
- **NEXT_HOP**: 다음 홉 라우터
- **ORIGIN**: 경로 기원 정보

## ■ Well-known Discretionary
- **LOCAL_PREF**: AS 내부 선호도

## ■ Optional
- **MED**: 인접 AS에게 선호 경로 힌트 제공
- **COMMUNITY**: 정책 태깅용 그룹 속성

---

# 6. BGP 루프 방지 방식

BGP는 거리 벡터처럼 홉 수를 쓰지 않고  
**AS_PATH**를 통해 루프를 방지한다.

- UPDATE 수신 시
  - 자신의 AS 번호가 AS_PATH에 있으면
  - **루프로 판단 → 경로 폐기**

---

# 7. BGP 패킷 / 전송 특징

- 전송 계층: **TCP**
- 포트 번호: **TCP 179**
- 신뢰성 보장(재전송, 순서 보장)
- 주기적 전체 업데이트 ❌  
  → **변경된 경로만 전송**

---

# 8. BGP 라우팅 테이블 구조

BGP는 여러 단계의 테이블을 사용한다.

1. **BGP Table (Adj-RIB-In)**
   - 수신한 모든 경로 저장
2. **Loc-RIB**
   - 정책 적용 후 최적 경로
3. **Global RIB**
   - 라우팅 테이블에 반영
4. **FIB**
   - 실제 패킷 포워딩에 사용

---

# 9. BGP 수렴(Convergence) 특징

- 장점:
  - 인터넷 규모에서도 확장 가능
  - 정책 기반 제어 가능
  - 루프에 매우 강함
- 단점:
  - 수렴 속도가 **느림**
  - 설정과 운영이 복잡
  - 메모리/CPU 사용량 큼

---

# 10. BGP 핵심 특징 요약

- Path-Vector 기반
- AS 간 라우팅(EGP)
- 정책 중심 경로 선택
- TCP 179 사용
- AS_PATH로 루프 방지
- eBGP / iBGP 구조
- 인터넷 백본의 핵심 프로토콜
