# 1. IS-IS 개요

→ **IS-IS는 링크 상태(Link-State) 정보를 기반으로 AS 내부에서 최단 경로를 계산하는 IGP이다.**

- 라우팅 범위: **AS 내부 라우팅 (Intra-AS)**
- 라우팅 방식: **Link-State**
- 최단 경로 계산: **SPF (Dijkstra)**
- 대규모 네트워크(ISP, 통신사 백본)에 적합
- OSPF와 유사한 동작 구조
- **IP 위가 아닌 L2(Data Link Layer)에서 직접 동작**

---

# 2. IS-IS 동작 과정

1. **Hello(PDU) 교환 → Neighbor 형성**
2. **LSP(Link State PDU) 교환**
3. **LSDB(Link-State Database) 구성**
4. 네트워크 전체 토폴로지 인식
5. **SPF(Dijkstra) 알고리즘 실행**
6. 최단 경로 계산
7. 라우팅 테이블(RIB)에 반영
8. 토폴로지 변경 발생 시
   - 변경된 LSP만 플러딩
   - 즉각 SPF 재계산 수행

---

# 3. IS-IS Level 구조

## ■ Level 1 (L1)
- **Area 내부 라우팅**
- 같은 Area 내에서만 라우팅 정보 교환
- OSPF Area 내부와 유사

## ■ Level 2 (L2)
- **Area 간 라우팅 (Backbone 역할)**
- 전체 네트워크 연결 담당
- OSPF Area 0과 유사

## ■ Level 1-2 Router
- L1과 L2를 동시에 수행
- Area 경계 라우터 역할

---

# 4. IS-IS Area 구조 특징

- **Backbone Area(Area 0) 강제 없음**
- Level 구조로 자연스럽게 계층화
- OSPF보다 **구조가 단순하고 유연**

---

# 5. IS-IS PDU 종류

| PDU | 이름 | 설명 |
|-----|------|------|
| **IIH** | Hello PDU | 이웃 발견 및 유지 |
| **LSP** | Link State PDU | 링크 상태 정보 전달 |
| **CSNP** | Complete SNP | LSDB 전체 요약 |
| **PSNP** | Partial SNP | 일부 LSP 요청/확인 |

---

# 6. IS-IS 루프 방지 방식

- Link-State 기반 프로토콜
- Area(Level) 내 모든 라우터가 **동일한 LSDB 공유**
- SPF 알고리즘으로
  - **Loop-free 경로 계산**
- 거리 벡터 방식의 Count-to-Infinity 문제 없음

---

# 7. IS-IS 패킷 / 전송 특징

- 동작 계층: **Data Link Layer (L2)**
- IP 주소에 의존하지 않음
- 멀티캐스트 사용
- 주기적 전체 업데이트 ❌  
  → **변경된 LSP만 전송**

---

# 8. IS-IS 라우팅 테이블 특징

- LSDB를 기반으로 SPF 계산
- 계산 결과를 라우팅 테이블(RIB)에 반영
- Equal-Cost Multi-Path(ECMP) 지원

---

# 9. IS-IS 수렴(Convergence) 특징

- 장점:
  - 대규모 네트워크에서도 높은 확장성
  - 빠른 수렴
  - IP 변경에도 안정적
- 단점:
  - 설정과 이해 난이도가 높음
  - 소규모 네트워크에는 과한 구조

---

# 10. IS-IS 핵심 특징 요약

- Link-State 기반 IGP
- SPF(Dijkstra) 사용
- Level 1 / Level 2 계층 구조
- L2에서 동작 (IP 비의존)
- 루프에 매우 강함
- ISP / 통신사 백본에서 선호
