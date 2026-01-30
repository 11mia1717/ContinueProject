# Continue Bank 통합 프로젝트 (2026)
> "데이터의 가치를 연결하고 금융의 경계를 넓힙니다."

Continue Bank의 통합 금융 플랫폼 프로젝트입니다.  
본 프로젝트는 **'위탁사(Continue Bank)'**와 **'수탁사(SSAP 본인인증, 콜센터)'** 간의 안전한 금융 데이터 처리 및 컴플라이언스(법규 준수) 시스템을 구축합니다.


## 📂 프로젝트 구조

| 폴더명 | 설명 | 주요 서비스 |
| :--- | :--- | :--- |
| **`homepage-project/`** | 대고객 뱅킹 및 본인인증 플랫폼 | **Continue Bank** (위탁사), **SSAP** (인증 수탁사) |
| **`call-center-project/`** | 상담원 업무 시스템 및 카드 관리 | **DAVADA 콜센터** (마케팅 수탁사), **카드 발행사** |

---

## 🚀 전체 서비스 요약

### 1. 홈페이지 프로젝트 (뱅킹 & 인증 인프라)
*   **은행 위탁사 (Continue Bank)**
    *   **백엔드**: 포트 `8085` (Java Spring Boot)
    *   **프론트엔드**: 포트 `5175` (React)
    *   **주요 기능**: 대고객 뱅킹, 계좌 개설, 마케팅 동의 관리(9+4종)
*   **인증 수탁사 (SSAP)**
    *   **백엔드**: 포트 `8086` (Java Spring Boot)
    *   **프론트엔드**: 포트 `5176` (React)
    *   **주요 기능**: 신원 인증, 본인 확인(OTP), 연계정보(CI/DI) 생성

### 2. 콜센터 프로젝트 (영업 및 원장 관리)
*   **위탁 콜센터 (DAVADA Call Center)**
    *   **백엔드**: 포트 `8082` (Java Spring Boot)
    *   **프론트엔드**: 포트 `5173` (React)
    *   **주요 기능**: 상담원 전용 어드민, 아웃바운드 콜, 개인정보 미저정(무상태)
*   **카드 발행사 (DAVADA Issuer)**
    *   **백엔드**: 포트 `8081` (Java Spring Boot)
    *   **프론트엔드**: 포트 `5174` (React)
    *   **주요 기능**: 카드 원장 관리, 발급 심사, 분실 정지 처리

---

## 🛠 실행 방법 (빠른 시작)

각 프로젝트 폴더의 배치 파일을 통해 모든 서비스를 한꺼번에 시작하거나 종료할 수 있습니다.

### 서비스 실행
1.  **홈페이지 서비스 (은행 + 본인인증)**
    ```bash
    cd homepage-project
    start-all.bat
    ```
2.  **콜센터 서비스 (상담 시스템)**
    ```bash
    cd call-center-project
    start_all_services.bat
    ```

### 서비스 종료
*   각 폴더의 `stop-all.bat` 또는 `stop_all_services.bat`를 실행하세요.

---

## 🔗 주요 가이드 및 설계 문서

*   [시스템 포트 명세 가이드](./system_port_specification.md)
*   [데이터베이스 설계 명세 (ERD 상세)](./docs/DB_SCHEMA.md)
*   [개인정보 처리 상세 흐름도](./docs/PRIVACY_FLOW_2026.md)
*   [인증 토큰(JWT) 및 데이터 통신 흐름](./docs/AUTH_PRIVACY_GUIDE.md)
*   [비즈니스 로직 및 워크플로우 명세](./docs/BUSINESS_LOGIC.md)
*   [홈페이지 프로젝트 상세 기술 문서](./homepage-project/README.md)
*   [콜센터 프로젝트 상세 기술 문서](./call-center-project/README.md)

