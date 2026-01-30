# 데이터베이스 명세서 (Database Schema)
> 2026.01.30 기준 (최종 현행화 완료)

본 문서는 **Continue Bank** 생태계(위탁사, 수탁사, 콜센터, 카드 발급사)의 데이터베이스 스키마를 정의합니다.

---

## 1. 위탁사 DB (`entrusting_db`)
**포트**: 3306 (MySQL)
**용도**: 고객 프로필, 계좌 정보, 컴플라이언스(약관 동의) 및 접근 로그 보관

### `site_users` 테이블
사용자 프로필 및 각종 약관 동의 상태를 관리합니다.
| 컬럼명 | 타입 | 속성 | 설명 | 보안/컴플라이언스 |
| :--- | :--- | :--- | :--- | :--- |
| `id` | BIGINT | PK, Auto Inc | 고유 사용자 ID | |
| `username` | VARCHAR | Unique | 로그인 아이디 | |
| `password` | VARCHAR | | 암호화된 비밀번호 | BCrypt 해시 |
| `name` | VARCHAR(500) | | **암호화**(AES-256) 성명 | 2026 데이터 보호 규정 |
| `phone_number` | VARCHAR(500) | | **암호화**(AES-256) 연락처 | 2026 데이터 보호 규정 |
| `ci` | VARCHAR(120) | Unique | 연계정보 (Connecting Info) | 중복 가입 방지용 |
| `di` | VARCHAR(90) | | 중복가입확인정보 | 사이트 식별용 |
| `is_verified` | BOOLEAN | | 본인인증 완료 여부 | 실명 확인 상태 |
| `terms_agreed` | BOOLEAN | | [필수] 이용약관 동의 | |
| `privacy_agreed` | BOOLEAN | | [필수] 개인정보 수집 이용 동의 | |
| `unique_id_agreed` | BOOLEAN | | [필수] 고유식별정보 처리 동의 | 필수 항목 |
| `credit_info_agreed` | BOOLEAN | | [필수] 신용정보 조회·제공 동의 | 필수 항목 |
| `carrier_auth_agreed` | BOOLEAN | | [필수] 통신사 본인확인 동의 | 필수 항목 |
| `electronic_finance_agreed` | BOOLEAN | | [필수] 전자금융거래 기본약관 | 필수 항목 |
| `monitoring_agreed` | BOOLEAN | | [필수] 금융거래 모니터링/AML | 필수 항목 |
| `ssap_provision_agreed` | BOOLEAN | | [선택] 제휴 TM 센터 정보 제공 | 스타벅스 이벤트 연동 |
| `third_party_provision_agreed` | BOOLEAN | | [선택] 제3자 정보 제공 동의 | 위탁 업무용 |
| `marketing_agreed`| BOOLEAN | | [선택] 마케팅 수신 전체 동의 | 선택 항목 |
| `marketing_sms` | BOOLEAN | | SMS 마케팅 수신 여부 | |
| `marketing_email` | BOOLEAN | | 이메일 마케팅 수신 여부 | |
| `marketing_push` | BOOLEAN | | 푸시 마케팅 수신 여부 | |
| `marketing_personal_agreed` | BOOLEAN | | 개인맞춤형 상품 추천 동의 | 선택 항목 |
| `privacy_agreed_at`| DATETIME | | 개인정보 수집 동의 일시 | |
| `terms_agreed_at` | DATETIME | | 서비스 이용약관 동의 일시 | |
| `data_expire_at` | DATETIME | | 데이터 보관 만료일 | 잊혀질 권리 (5년) |
| `third_party_retention_until` | DATETIME | | 제3자 제공 정보 보관 기한| 콜센터 3개월 제한 |

### `account` 테이블
은행 계좌 정보를 보관합니다.
| 컬럼명 | 타입 | 속성 | 설명 |
| :--- | :--- | :--- | :--- |
| `id` | BIGINT | PK, Auto Inc | 계좌 ID |
| `user_id` | BIGINT | FK | 소유자 User ID (외래키) |
| `account_number` | VARCHAR | Unique | 계좌번호 (하이픈 포함) |
| `balance` | DECIMAL | | 현재 잔액 |
| `account_type` | VARCHAR | | 계좌 종류 (입출금, 예적금) |

### `access_log` 테이블
모든 개인정보 접근 및 수정 이력을 기록합니다.
| 컬럼명 | 타입 | 속성 | 설명 | 비고 |
| :--- | :--- | :--- | :--- | :--- |
| `id` | BIGINT | PK, Auto Inc | 로그 고유 ID | |
| `user_id` | BIGINT | | 조회된 대상 사용자 ID | |
| `accessor_type` | VARCHAR | | 접근 주체 (SELF, ADMIN, TM_AGENT)| |
| `accessor_id` | VARCHAR | | 접근한 상세 ID (로그인아이디 등) | |
| `action` | VARCHAR | | 작업 종류 (조회, 수정, 삭제) | |
| `details` | TEXT | | 상세 작업 내용 | |
| `accessed_at` | DATETIME | | 접근 일시 | 불변 로그 |

---

## 2. 수탁사(SSAP) DB (`trustee_db`)
**포트**: 3307 (MySQL)
**용도**: 휘발성 본인인증 토큰 및 가상 통신사 가입자 정보 관리

### `auth_token` 테이블
임시 본인인증 세션을 관리합니다 (TTL 3분 적용).
| 컬럼명 | 타입 | 속성 | 설명 | 보안 비고 |
| :--- | :--- | :--- | :--- | :--- |
| `token_id` | UUID | PK | 인증 세션 고유 키 (tokenId) | |
| `auth_request_id` | VARCHAR | | 위탁사 요청 식별자 | |
| `name` | VARCHAR(500) | | **암호화**(AES-256) 성명 | |
| `client_data` | VARCHAR(500) | | **암호화**(AES-256) 연락처 | |
| `otp` | VARCHAR(100) | | 발송된 OTP 번호 | 6자리 난수 |
| `ci` | VARCHAR(120) | | 생성된 연계정보 | |
| `di` | VARCHAR(90) | | 생성된 중복가입확인정보 | |
| `status` | VARCHAR | | 진행 상태 (PENDING, COMPLETED, USED) | |
| `created_at` | DATETIME | | 세션 생성 시간 | 3분 후 자동 파기 대상 |

---

## 3. 콜센터(TM) DB (`callcenter_db`)
**포트**: 3308 (MySQL)
**용도**: 상담 결과 및 상담원 활동 로그 보관 (개인정보는 조회만 하고 저장하지 않음)

### `call_results` 테이블
아웃바운드 상담 결과를 보관합니다.
| 컬럼명 | 타입 | 속성 | 설명 | 보안 비고 |
| :--- | :--- | :--- | :--- | :--- |
| `id` | BIGINT | PK, Auto Inc | 상담 결과 ID | |
| `agent_id` | VARCHAR | | 상담원 로그인 ID | |
| `target_id` | VARCHAR | | 위탁사 고객 참조 ID (No PII) | 비식별 참조 키 |
| `masked_name` | VARCHAR | | 마스킹된 성명 (홍*동) | 최소 수립 원칙 |
| `purpose` | VARCHAR | | 상담 목적 (상품 안내 등) | |
| `result` | VARCHAR | | 상담 결과 (성공, 부재, 거절) | |
| `recording_agreed`| BOOLEAN | | 음성 녹취 동의 여부 | |
| `retention_until` | DATETIME | | 기록 보관 기한 | 3개월 후 자동 삭제 |

### `audit_log` 테이블
상담원의 시스템 내 모든 활동을 추적합니다.
| 컬럼명 | 타입 | 속성 | 설명 |
| :--- | :--- | :--- | :--- |
| `id` | BIGINT | PK | 로그 고유 ID |
| `agent_id` | VARCHAR | | 작업을 수행한 상담원 ID |
| `action` | VARCHAR | | 작업 종류 (검색, 상세조회) |
| `target` | VARCHAR | | 대상 식별자 (마스킹 처리) |
| `timestamp` | DATETIME | | 이벤트 발생 일시 |

---

## 4. 카드 발급사(Issuer) DB (`issuer_db`)
**포트**: 8081 (H2 Console)
**용도**: 레거시 카드 발급 및 가입자 데이터 시뮬레이션 (In-Memory)

### `cards` 테이블 / `customers` 테이블
금융 거래 테스트를 위한 모의 데이터 세트입니다. (상세 생략)
