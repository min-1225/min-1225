# 커플 가계부

두 사람이 함께 지출을 기록하고, 소비 패턴을 분석해 과소비를 줄이는 커플 전용 가계부 웹앱입니다.

자동 은행 연동 대신 직접 입력을 중심으로 설계했습니다. 지출을 입력하는 과정 자체가 소비를 의식하게 만들고, 두 사람이 하나의 household 데이터를 공유하면서 월별 지출 흐름을 함께 확인할 수 있도록 만드는 것이 목표입니다.

## Repository

https://github.com/min-1225/budget-web

## 핵심 컨셉

기존 자동 연동 가계부는 은행 API 연결이 불안정하거나 데이터가 늦게 반영되는 문제가 있습니다. 이 프로젝트는 수동 입력을 기반으로 정확한 데이터를 만들고, 그 데이터를 바탕으로 카테고리별 지출과 과소비 위험을 보여주는 방향으로 설계했습니다.

| 기존 자동 연동 앱 | 커플 가계부 |
| --- | --- |
| 은행 API 연결에 의존 | 직접 입력으로 데이터 정확도 확보 |
| 개인 단위 관리 | 커플/동거인 household 공유 |
| 소비 후 집계 중심 | 소비 패턴 분석과 사전 경고 지향 |
| 단순 통계 표시 | 한국어 인사이트 제공 목표 |

## 현재 구현된 기능

- 이메일 기반 로그인/회원가입
- 수입/지출 기록 추가
- 월별 거래 내역 조회
- 카테고리별 지출 집계
- 카테고리별 지출 차트
- 이전 기간 평균/표준편차 기반 과소비 감지
- 더미 데이터 생성
- Supabase Auth/DB 연동

## 목표 기능

- 커플 household 초대 링크
- 파트너 지출 실시간 반영
- 본인 지출만 수정/삭제 가능
- 카테고리별 월 목표 예산 설정
- Claude Haiku 기반 한국어 소비 인사이트
- 70% / 90% / 100% 예산 도달 알림
- Web Push 및 이메일 fallback

## 기술 스택

- Frontend: Next.js, React
- Styling: Tailwind CSS
- Backend/DB: Supabase
- Auth: Supabase Auth
- Deploy: Vercel
- AI: Claude Haiku API 예정
- Notification: Web Push API 예정

## 주요 화면 구성

### 대시보드

- 이번 달 총 지출
- 수입/지출 합계
- 카테고리별 지출 차트
- 과소비 경고 카드
- 거래 내역 리스트
- 지출 입력 폼

### 지출 입력

- 수입/지출 선택
- 금액 입력
- 카테고리 선택
- 날짜 선택
- 메모 입력

### 소비 분석

현재는 최근 기간 데이터를 기준으로 카테고리별 평균과 표준편차를 계산하고, 이번 달 지출이 기준치를 넘으면 경고를 표시합니다.

추후에는 30일 지출 데이터를 Claude Haiku API에 전달해 한국어 인사이트 3개를 생성하는 기능을 추가할 예정입니다.

## 데이터 모델 계획

```text
Household
├── id
├── name
├── invite_token
├── invite_used_at
└── created_at

User
├── id
├── household_id
├── email
├── display_name
└── created_at

Expense
├── id
├── household_id
├── user_id
├── amount
├── category
├── memo
├── date
└── created_at

BudgetGoal
├── id
├── household_id
├── category
├── monthly_limit
└── month
```

Supabase 배포 시 모든 테이블은 household_id 기준 Row Level Security 설정이 필요합니다.

## 개발 로드맵

### Phase 1. MVP

- 이메일 로그인
- household 초대 시스템
- 지출 입력/조회
- 카테고리별 월간 집계
- 파트너 지출 실시간 반영
- 기본 대시보드

### Phase 2. AI 인사이트

- 30일 지출 데이터 분석
- Claude Haiku API 연동
- household 단위 1시간 캐시
- 한국어 소비 인사이트 표시

### Phase 3. 예산 및 알림

- 카테고리별 목표 예산
- 예산 도달률 계산
- 70% / 90% / 100% 도달 알림
- Web Push 및 이메일 fallback

## 주의사항

- Supabase RLS를 설정하지 않으면 다른 사용자의 데이터가 노출될 수 있습니다.
- iOS Web Push는 PWA 설치 이후에만 동작합니다.
- AI 분석 API는 호출 비용이 있으므로 캐시 전략이 필요합니다.
- 초대 토큰은 1회 사용 후 만료되도록 관리해야 합니다.

