# 시스템 포트 명세서 (System Port Specification)
> 2026.01.30 기준

본 문서는 Continue Bank 생태계를 구성하는 각 서비스의 네트워크 포트 할당 현황을 정의합니다. 모든 서비스는 본 명세에 정의된 포트를 준수해야 합니다.

---

## 1. 홈페이지 프로젝트 (Banking & Auth)

| 서비스명 | 구분 | 포트 (Port) | 기술 스택 | 비고 |
| :--- | :--- | :--- | :--- | :--- |
| **Continue Bank** | 백엔드 (API) | `8085` | Spring Boot | 위탁사 메인 WAS |
| **Continue Bank** | 프론트엔드 | `5175` | React / Vite | 대고객 홈페이지 |
| **SSAP (V-PASS)** | 백엔드 (API) | `8086` | Spring Boot | 인증 수탁사 WAS |
| **SSAP (V-PASS)** | 프론트엔드 | `5176` | React / Vite | 본인인증 전용 페이지 |

---

## 2. 콜센터 프로젝트 (TM & Issuer)

| 서비스명 | 구분 | 포트 (Port) | 기술 스택 | 비고 |
| :--- | :--- | :--- | :--- | :--- |
| **DAVADA Call Center**| 백엔드 (API) | `8082` | Spring Boot | 상담원 지원 WAS |
| **DAVADA Call Center**| 프론트엔드 | `5173` | React / Vite | 상담원 전용 포털 |
| **DAVADA Issuer** | 백엔드 (API) | `8081` | Spring Boot | 카드 발행사 WAS |
| **DAVADA Issuer** | 프론트엔드 | `5174` | React / Vite | 카드사 관리 어드민 |

---

## 3. 인프라 및 데이터베이스 (Infrastructure)

| 서비스명 | 포트 (Port) | 구분 | 비고 |
| :--- | :--- | :--- | :--- |
| **mysql-entrusting** | `3306` | 데이터베이스 | 위탁사(은행) 전용 DB |
| **mysql-trustee** | `3307` | 데이터베이스 | 수탁사(SSAP) 전용 DB |
| **mysql-callcenter** | `3308` | 데이터베이스 | 수탁사(콜센터) 전용 DB |
| **h2-console** | `8081` | DB 관리 | 카드 발행사(In-Memory) 콘솔 |

---

## 💡 개발 및 배포 참고사항

1.  **CORS 설정**: 프론트엔드(`5173~5176`)에서 백엔드(`8081~8086`)로의 요청은 각 WAS의 `WebConfig` 파일 또는 프록시 설정을 통해 허용됩니다.
2.  **포트 충돌 방지**: 로컬 개발 환경에서 다수의 서비스를 동시 기동할 경우 포트가 중복되지 않도록 주의하십시오.
3.  **환경 변수**: 각 서비스의 `application.properties` 또는 `.env` 파일에 위 포트 번호가 올바르게 설정되어 있는지 확인하십시오.
