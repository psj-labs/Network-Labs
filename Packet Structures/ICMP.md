# ICMP Message Structures (Echo & Error Control)

```text
################################################################################
                         ICMP Message Structures
      (Echo Request / Reply  &  Error-Control Message Combined Reference)
################################################################################


───────────────────────────────────────────────────────────────────────────────
                   ICMP Echo Request / Echo Reply Format
───────────────────────────────────────────────────────────────────────────────

┌───────────────┬───────────────┬──────────────────────────────┐
│     Type      │     Code      │           Checksum           │
│    (1 byte)   │   (1 byte)    │            (2 bytes)         │
└───────────────┴───────────────┴──────────────────────────────┘

┌──────────────────────────────────────────────────────────────┐
│   Rest of Header (Identifier + Sequence Number)  (4 bytes)   │
└──────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────┐
│                     Payload (variable length)                │
│                        Ping Data …                           │
└──────────────────────────────────────────────────────────────┘


Type (1 byte)
 : ICMP 메시지 종류
   예시 → 8 (Echo Request)
           0 (Echo Reply)

Code (1 byte)
 : Type 내 세부 코드
   예시 → 0 (Echo Request / Reply는 항상 0)

Checksum (2 bytes)
 : ICMP 헤더 + 데이터 전체 무결성 검증
   예시 → 0xF2B1

Rest of Header (4 bytes)
 : Echo 메시지 전용 필드
   - Identifier (2 bytes) → 요청·응답 매칭 식별자
   - Sequence Number (2 bytes) → 요청 순번
   예시 → Identifier = 0x1F2B
           Sequence  = 0x0001

Payload (variable)
 : Ping 테스트용 데이터 영역
   예시 → "abcdefghijklmnop..." (56 bytes)



───────────────────────────────────────────────────────────────────────────────
                      ICMP Error-Control Message Format
───────────────────────────────────────────────────────────────────────────────

┌───────────────┬───────────────┬──────────────────────────────┐
│     Type      │     Code      │            Checksum          │
│    (1 byte)   │   (1 byte)    │             (2 bytes)        │
└───────────────┴───────────────┴──────────────────────────────┘

┌──────────────────────────────────────────────────────────────┐
│            Error-specific Field (Unused / MTU / Pointer)     │
│                              (4 bytes)                       │
└──────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────┐
│                   Original IP Header (20 bytes)              │
└──────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────┐
│              Original Packet Data (First 8 bytes)            │
└──────────────────────────────────────────────────────────────┘


Type (1 byte)
 : ICMP 오류 메시지 종류

   주요 타입
     3  → Destination Unreachable (목적지 도달 불가)
     4  → Source Quench (폐기됨)
     5  → Redirect (경로 재지정)
     11 → Time Exceeded (TTL 만료)
     12 → Parameter Problem (헤더 오류)

Code (1 byte)
 : Type 별 세부 원인 코드

   예시:
     Type 3 - Destination Unreachable
       Code 0 → Network Unreachable
       Code 1 → Host Unreachable
       Code 3 → Port Unreachable

     Type 11 - Time Exceeded
       Code 0 → TTL Exceeded in Transit

Checksum (2 bytes)
 : ICMP 오류 패킷 전체 무결성 검증

Error-specific Field (4 bytes)
 : 오류 타입별 추가 정보
   - MTU 값 포함 (Fragmentation Needed)
   - 문제 위치 포인터
   - 사용하지 않는 경우 0

Original IP Header
 : 오류를 발생시킨 원본 IP 패킷의 헤더

Original Packet Data (8 bytes)
 : 원본 패킷 상위계층 헤더 일부
   → 송신자가 오류 발생 세션/포트 식별 가능
