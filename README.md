# Continue Bank Ecosystem Project

Continue Bank의 통합 금융 플랫폼 프로젝트입니다.
본 프로젝트는 **'위탁사(Continue Bank)'**와 **'수탁사(SSAP, Call Center)'** 간의 안전한 금융 데이터 처리 및 컴플라이언스 워크플로우를 구현합니다.

> **📢 주요 변경 사항 (2026.01.30)**
> 1. **브랜드 변경**: 기존 `V-PASS` 본인인증 서비스가 **`SSAP`**으로 리브랜딩 되었습니다.
> 2. **구조 분리**: TM Center(콜센터)는 독립적인 `call-center-project` 폴더에서 관리됩니다.
> 3. **포트 현행화**: 전체 시스템의 포트 구성이 [시스템 포트 명세서](./system_port_specification.md)에 따라 재정의되었습니다.

## 📂 프로젝트 구조

| 폴더명 | 설명 | 주요 서비스 |
| :--- | :--- | :--- |
| **`homepage-project/`** | 대고객 뱅킹 서비스 및 인증 제공자 | **Continue Bank** (위탁사), **SSAP** (구 V-PASS, 인증 수탁사) |
| **`call-center-project/`** | 전문 상담원 시스템 (별도 관리) | **DAVADA Call Center** (TM 상담원), **DAVADA Issuer** (카드 발급/관리) |

---

## 🚀 전체 서비스 요약

### 1. Homepage Project (뱅킹 & 인증)
*   **위탁사 (Continue Bank)**
    *   **Backend**: Port `8085` (Java Spring Boot)
    *   **Frontend**: Port `5175` (React)
    *   **Role**: 고객용 웹사이트, 계좌 개설, 마케팅 동의 관리

*   **수탁사 (SSAP)** (구 V-PASS)
    *   **Backend**: Port `8086` (Java Spring Boot)
    *   **Frontend**: Port `5176` (React)
    *   **Role**: 신원 인증(Identity), 본인 확인, 가상 통신사 데이터 관리

### 2. Call Center Project (TM 센터)
*   **콜센터 (DAVADA Call Center)**
    *   **Backend**: Port `8082` (Java Spring Boot)
    *   **Frontend**: Port `5173` (React)
    *   **Role**: 상담원 전용 시스템, 아웃바운드/인바운드 상담, 개인정보 미저장(Stateless)

*   **카드 발급 (DAVADA Issuer)**
    *   **Backend**: Port `8081` (Java Spring Boot)
    *   **Frontend**: Port `5174` (React)
    *   **Role**: 카드 원장 관리, 발급 심사

---

## 🛠 실행 방법 (Quick Start)

각 프로젝트 폴더 내의 배치 파일을 사용하여 서비스를 시작하고 종료할 수 있습니다.

### 서비스 시작
1.  **Homepage Project (Continue Bank + SSAP)**
    ```bash
    cd homepage-project
    start-all.bat
    ```
2.  **Call Center Project (TM Center)**
    ```bash
    cd call-center-project
    start_all_services.bat
    ```

### 서비스 종료
*   각 폴더의 `stop-all.bat` 또는 `stop_all_services.bat` 실행

---

## 🔗 주요 문서

*   [시스템 포트 명세서 (System Port Specification)](./system_port_specification.md)
*   [데이터베이스 명세서 (DB Schema)](./docs/DB_SCHEMA.md)
*   [개인정보 처리 흐름도 (Privacy Flow)](./docs/PRIVACY_FLOW_2026.md)
*   [인증 토큰(JWT) & 상세 처리 흐름](./docs/AUTH_PRIVACY_GUIDE.md)
*   [비즈니스 로직 & 워크플로우 (Business Logic)](./docs/BUSINESS_LOGIC.md)
*   [Homepage Project 상세 문서](./homepage-project/README.md)
*   [Call Center Project 상세 문서](./call-center-project/README.md)
