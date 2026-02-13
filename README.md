# syncenv (@whitehander/syncenv)

AWS Secrets Manager에 등록된 환경변수를 로컬 파일로 동기화하는 CLI입니다.

## 1) 무엇을 하는가 / 범위
- 프로젝트별로 “어떤 Secret을 어떤 파일로 동기화할지” 설정을 등록(`syncenv init`)
- 등록된 설정대로 Secrets Manager 값을 내려받아 동기화(`syncenv sync`)
- 로컬 설정 파일 초기화(`syncenv reset`)

## 2) 아키텍처 / 데이터 흐름(상위)
- 사용자 CLI 실행 → AWS Secrets Manager 조회 → 로컬 파일(.env 등) 생성/갱신
- (일부 기능에서) DynamoDB를 사용하는 것으로 보입니다(의존성 기준, TODO: 실제 사용처 확인)

## 3) Quick Start

### 설치
```bash
# 로컬에서
npm install
node src/index.js --help

# 패키지로 설치하는 경우(예시)
# npm i -g @whitehander/syncenv
```

### 사용
```bash
syncenv init
syncenv sync
syncenv sync --verbose
syncenv reset
```

### 테스트
- TODO: 테스트 스크립트가 제공되지 않습니다.

## 4) 환경변수 / 설정 계약(Env & Config)
- AWS 인증
  - AWS SDK 기본 Credential Provider Chain을 사용합니다.
  - 로컬에서는 `AWS_PROFILE`, `AWS_ACCESS_KEY_ID/AWS_SECRET_ACCESS_KEY` 등 일반적인 방식으로 설정하세요.

- 로컬 설정 파일
  - `.syncenv` 파일을 생성/사용합니다(정확한 포맷은 코드 기준, TODO: 문서화).

## 5) 운영 / 런북(Deploy · Healthcheck · Logs)
- 팀 온보딩 시
  1) AWS 권한 부여(Secrets Manager read)
  2) `syncenv init`으로 프로젝트 설정 등록
  3) `syncenv sync`로 .env 동기화

- 장애 대응
  - 권한(AccessDenied) → AWS IAM 정책/프로파일 확인
  - Secret 형식(JSON/plain) 불일치 → Secret 값 포맷 확인

## 6) 레거시/호환성 제약(해당 시)
- TODO: 지원하는 Secret 형식/키 규칙을 문서화 필요

## 7) 기여 / 브랜치 전략
- 기본 브랜치: `main`
- CLI는 사용자가 많으므로, 변경 시 하위호환을 우선 고려하고 버전 업데이트 정책을 따릅니다.
