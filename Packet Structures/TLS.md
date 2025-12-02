# TLS Record Layer Structure (5 bytes header)

```text
################################################################################
                         TLS Record Layer Header (5 bytes)
################################################################################

┌───────────────┬────────────────────┬──────────────────────────┐
│  Content Type │        Version     │           Length         │
│   (1 byte)    │       (2 bytes)    │         (2 bytes)        │
└───────────────┴────────────────────┴──────────────────────────┘

┌──────────────────────────────────────────────────────────────────────────────┐
│                           Encrypted Fragment (variable)                      │
└──────────────────────────────────────────────────────────────────────────────┘


Content Type (1 byte)
 : TLS 레코드 상위 메시지 타입

   주요 값
     - 20 → ChangeCipherSpec
     - 21 → Alert
     - 22 → Handshake
     - 23 → Application Data

   예시 → 23 (암호화된 Application Data)

Version (2 bytes)
 : TLS 프로토콜 버전 필드

   주요 값
     - 0x0301 → TLS 1.0
     - 0x0302 → TLS 1.1
     - 0x0303 → TLS 1.2
     - TLS 1.3 에서도 레코드 필드는 여전히 0x0303 사용

   예시 → 0x0303

Length (2 bytes)
 : Fragment 데이터 길이
   (Encrypted Fragment 크기)

   예시 → 517 bytes

Encrypted Fragment
 : 실제 암호화된 TLS 데이터
   - 핸드셰이크 메시지
   - Application Data
   - Alert 메시지 등
