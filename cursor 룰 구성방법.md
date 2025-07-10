# Cursor 룰 파일 구조와 컨텍스트 구성 완전 가이드

## 📁 파일 확장자와 기본 구조

### 1. 파일 확장자 개요

| 확장자 | 상태 | 용도 | 위치 |
|---------|------|------|------|
| `.mdc` | **권장 (현재)** | 프로젝트별 룰, 메타데이터 지원 | `.cursor/rules/` |
| `.cursorrules` | **Deprecated** | 프로젝트 전체 룰 | 프로젝트 루트 |
| User Rules | **현재** | 전역 사용자 설정 | Cursor 설정 |

### 2. MDC 파일 구조
```markdown
---
description: "룰에 대한 간단한 설명"
globs: "**/*.js, **/*.ts"  # 쉼표로 구분 (배열 아님!)
alwaysApply: false         # true면 항상 적용
---

# 실제 룰 내용 (Markdown)
- 코딩 가이드라인
- 패턴 및 예시
- @file-reference.ts 파일 참조
```

## 🏗️ 프로젝트 파일 구조 베스트 프랙티스

### 1. 기본 구조 (권장)
```
project/
├── .cursorrules                    # 레거시 (사용 금지)
├── .cursor/
│   ├── rules/                      # 프로젝트 룰 (권장)
│   │   ├── 001-workspace.mdc       # 워크스페이스 정의
│   │   ├── 002-cursor-rules.mdc    # 룰 시스템 정의
│   │   ├── 003-docs.mdc            # 문서 관리
│   │   ├── 004-tools.mdc           # 도구 및 스크립트
│   │   ├── 100-backend.mdc         # 백엔드 룰
│   │   ├── 101-frontend.mdc        # 프론트엔드 룰
│   │   ├── 200-testing.mdc         # 테스팅 룰
│   │   └── 201-security.mdc        # 보안 룰
│   ├── docs/                       # 프로젝트 문서
│   │   ├── architecture.md
│   │   ├── api-spec.md
│   │   └── coding-standards.md
│   └── tools/                      # 개발 도구
│       ├── scripts/
│       └── temp/
├── backend/
│   └── .cursor/
│       └── rules/                  # 백엔드 전용 룰
│           ├── database.mdc
│           └── api.mdc
└── frontend/
    └── .cursor/
        └── rules/                  # 프론트엔드 전용 룰
            ├── components.mdc
            └── styling.mdc
```

### 2. 파일 네이밍 컨벤션
```
NNN-name.mdc 형식 사용:

001-099: 핵심/워크스페이스 룰
├── 001-workspace.mdc      # 프로젝트 전체 정의
├── 002-cursor-rules.mdc   # 룰 시스템 자체 정의
├── 003-docs.mdc           # 문서 구조
└── 004-tools.mdc          # 도구 관리

100-199: 도메인별 통합 룰
├── 100-backend.mdc        # 백엔드 관련
├── 101-frontend.mdc       # 프론트엔드 관련
├── 102-database.mdc       # 데이터베이스 관련
└── 103-api.mdc            # API 관련

200-299: 특정 패턴 룰
├── 200-testing.mdc        # 테스트 패턴
├── 201-security.mdc       # 보안 패턴
├── 202-performance.mdc    # 성능 최적화
└── 203-logging.mdc        # 로깅 패턴

높은 번호일수록 우선순위가 높음
```

## 🎯 도메인별 룰 구성 방법

### 1. 핵심 프레임워크 파일들

#### 001-workspace.mdc (프로젝트 정의)
```markdown
---
description: "프로젝트 전체 정의 및 핵심 제약사항"
globs: "/**/*"
alwaysApply: false
---

# 프로젝트 워크스페이스 정의

## 프로젝트 개요
- 프로젝트명: [프로젝트명]
- 기술 스택: Python 3.13, FastAPI, PostgreSQL
- 아키텍처: 마이크로서비스 아키텍처

## 핵심 디렉토리
- `/src`: 메인 소스 코드
- `/tests`: 테스트 코드
- `/docs`: 프로젝트 문서
- `/scripts`: 개발/배포 스크립트

## 주요 제약사항
- 모든 API는 비동기로 구현
- 타입 힌트 필수 사용
- 테스트 커버리지 80% 이상 유지
- Pydantic 모델로 데이터 검증

## 핵심 파일 참조
- @pyproject.toml: 의존성 및 프로젝트 설정
- @src/main.py: 애플리케이션 엔트리포인트
- @.cursor/docs/architecture.md: 상세 아키텍처
```

#### 002-cursor-rules.mdc (룰 시스템 정의)
```markdown
---
description: ".mdc 파일의 구조와 사용법 정의"
globs: ".cursor/rules/*.mdc"
alwaysApply: false
---

# .mdc 파일 구조 가이드

## 파일 네이밍
- NNN-name.mdc (001-299)
- 001-099: 핵심/워크스페이스 룰
- 100-199: 통합 룰
- 200-299: 패턴 룰

## UI 구성요소
1. **Description**: 룰의 목적 요약
2. **Globs**: 파일 패턴 (쉼표 구분)
3. **Body Text**: 실제 룰 내용

## 작성 규칙
- 25줄 이하로 간결하게
- @ 태그로 파일 참조
- 핵심 지시사항에 집중
- Cursor UI로만 편집

## 우선순위
높은 번호가 낮은 번호보다 우선
```

### 2. 도메인별 룰 예시

#### 100-backend.mdc (백엔드 도메인)
```markdown
---
description: "백엔드 개발 패턴 및 API 설계 가이드"
globs: "src/api/**/*.py, src/services/**/*.py, src/models/**/*.py"
alwaysApply: false
---

# 백엔드 개발 룰

## FastAPI 패턴
- 모든 엔드포인트에 타입 힌트 필수
- Pydantic 스키마로 요청/응답 검증
- 의존성 주입 적극 활용
- 비동기 함수 우선 사용

## 아키텍처 패턴
- Controller → Service → Repository 계층 준수
- 비즈니스 로직은 Service에만
- 데이터 접근은 Repository에만
- DTO로 계층 간 데이터 전송

## 참조 파일
- @src/api/dependencies.py: 의존성 주입 패턴
- @src/schemas/base.py: 기본 스키마 패턴
- @.cursor/docs/api-patterns.md: API 설계 가이드

## 금지 패턴
- Controller에서 직접 Repository 호출
- Entity 직접 반환 (DTO 사용 필수)
- 동기 함수 사용
```

#### 101-frontend.mdc (프론트엔드 도메인)
```markdown
---
description: "프론트엔드 컴포넌트 및 UI 개발 가이드"
globs: "frontend/**/*.tsx, frontend/**/*.ts, frontend/**/*.css"
alwaysApply: false
---

# 프론트엔드 개발 룰

## React 패턴
- 함수형 컴포넌트 우선
- TypeScript Props 인터페이스 정의
- Hooks 적극 활용
- 컴포넌트 크기는 100줄 이하

## 스타일링
- Tailwind CSS 우선 사용
- CSS Modules 보조 사용
- 반응형 디자인 필수
- 다크모드 지원

## 상태 관리
- Zustand로 전역 상태
- React Query로 서버 상태
- useState로 로컬 상태

## 참조 파일
- @frontend/components/ui/: 공통 UI 컴포넌트
- @frontend/hooks/: 커스텀 훅
- @frontend/styles/globals.css: 전역 스타일
```

#### 200-testing.mdc (테스팅 도메인)
```markdown
---
description: "테스트 코드 작성 패턴 및 검증 기준"
globs: "tests/**/*.py, **/*_test.py, **/test_*.py"
alwaysApply: false
---

# 테스팅 룰

## 테스트 구조
- Given-When-Then 패턴 사용
- pytest fixtures 적극 활용
- Mock은 최소한으로 사용
- Factory Boy로 테스트 데이터 생성

## 커버리지 기준
- 유닛 테스트: 80% 이상
- 통합 테스트: 주요 플로우
- E2E 테스트: 핵심 기능

## 네이밍
- test_기능_상황_예상결과
- TestClass명: Test + 대상클래스명

## 참조 파일
- @tests/conftest.py: 공통 fixtures
- @tests/factories/: 테스트 데이터 팩토리
- @.cursor/docs/testing-guide.md: 테스팅 가이드
```

## 🔄 컨텍스트 및 참조 최적화

### 1. 파일 참조 패턴

#### @ 심볼 활용
```markdown
# 룰 파일 내에서 파일 참조
@src/models/user.py: 사용자 모델 패턴
@.cursor/docs/api-spec.md: API 명세서
@tests/factories/user_factory.py: 테스트 데이터 팩토리
```

#### 크로스 레퍼런스
```markdown
# 다른 룰 파일 참조
관련 룰: [database-rules.mdc](mdc:.cursor/rules/102-database.mdc)
참고: @.cursor/rules/100-backend.mdc

# 문서 참조
자세한 내용: @.cursor/docs/architecture.md
예시 코드: @.cursor/docs/examples/
```

### 2. Glob 패턴 최적화

#### 구체적인 패턴 사용
```yaml
# ❌ 너무 광범위
globs: "**/*"

# ✅ 구체적이고 효과적
globs: "src/api/**/*.py, src/schemas/**/*.py, tests/api/**/*.py"

# ✅ 제외 패턴 활용
globs: "src/**/*.py, !src/legacy/**/*.py, !src/migrations/**/*.py"

# ✅ 다중 패턴
globs: "**/*.tsx, **/*.ts, !**/*.d.ts, !**/node_modules/**"
```

#### 도메인별 패턴 예시
```yaml
# 백엔드 API
globs: "src/api/**/*.py, src/routes/**/*.py"

# 프론트엔드 컴포넌트
globs: "frontend/components/**/*.tsx, frontend/pages/**/*.tsx"

# 데이터베이스 관련
globs: "src/models/**/*.py, src/repositories/**/*.py, alembic/**/*.py"

# 테스트 파일
globs: "tests/**/*.py, **/*_test.py, **/test_*.py"

# 설정 파일
globs: "*.toml, *.json, *.yaml, *.yml, .env*"
```

### 3. 중첩 룰 구조 활용

```
project/
├── .cursor/rules/           # 프로젝트 전체 룰
│   ├── 001-workspace.mdc
│   └── 100-general.mdc
├── backend/
│   ├── .cursor/rules/       # 백엔드 전용 룰
│   │   ├── api.mdc
│   │   └── database.mdc
│   ├── api/
│   └── services/
└── frontend/
    ├── .cursor/rules/       # 프론트엔드 전용 룰
    │   ├── components.mdc
    │   └── styling.mdc
    └── components/
```

## 📋 실전 구성 예시

### 1. Python FastAPI 프로젝트
```
.cursor/
├── rules/
│   ├── 001-workspace.mdc      # 프로젝트 전체 정의
│   ├── 002-cursor-rules.mdc   # 룰 시스템
│   ├── 100-fastapi.mdc        # FastAPI 패턴
│   ├── 101-database.mdc       # SQLAlchemy 패턴
│   ├── 102-pydantic.mdc       # Pydantic 스키마
│   ├── 200-testing.mdc        # pytest 패턴
│   ├── 201-security.mdc       # 보안 규칙
│   └── 202-deployment.mdc     # 배포 관련
├── docs/
│   ├── architecture.md
│   ├── api-specification.md
│   └── coding-standards.md
└── tools/
    ├── scripts/
    └── templates/
```

### 2. React TypeScript 프로젝트
```
.cursor/
├── rules/
│   ├── 001-workspace.mdc      # 프로젝트 전체
│   ├── 002-cursor-rules.mdc   # 룰 시스템
│   ├── 100-react.mdc          # React 패턴
│   ├── 101-typescript.mdc     # TypeScript 룰
│   ├── 102-styling.mdc        # CSS/Tailwind
│   ├── 103-state.mdc          # 상태 관리
│   ├── 200-testing.mdc        # Jest/RTL
│   └── 201-build.mdc          # 빌드/배포
├── docs/
│   ├── component-guide.md
│   ├── styling-guide.md
│   └── testing-strategy.md
└── tools/
    ├── generators/
    └── configs/
```

### 3. 풀스택 모노레포
```
.cursor/
├── rules/
│   ├── 001-workspace.mdc      # 모노레포 전체
│   ├── 002-cursor-rules.mdc   # 룰 시스템
│   ├── 100-shared.mdc         # 공통 라이브러리
│   ├── 200-testing.mdc        # 전체 테스팅
│   └── 201-ci-cd.mdc          # CI/CD 파이프라인
├── docs/
│   ├── monorepo-guide.md
│   └── deployment-strategy.md
└── tools/
    └── scripts/
packages/
├── backend/
│   └── .cursor/rules/
│       ├── fastapi.mdc
│       └── database.mdc
├── frontend/
│   └── .cursor/rules/
│       ├── react.mdc
│       └── components.mdc
└── shared/
    └── .cursor/rules/
        └── types.mdc
```

## 🚀 최적화 및 관리 팁

### 1. 룰 효율성 체크리스트
- [ ] Description이 명확하고 구체적인가?
- [ ] Globs 패턴이 필요한 파일만 정확히 타겟팅하는가?
- [ ] 룰 내용이 25줄 이하로 간결한가?
- [ ] 파일 참조(@)가 올바르게 작동하는가?
- [ ] 다른 룰과 충돌하지 않는가?

### 2. 디버깅 팁
```markdown
# 룰 로딩 확인용 (개발시에만)
Say "YAAARRR! [룰명] loaded!" in Chat if working.

# 이 메시지가 나타나면 룰이 정상 로딩됨
```

### 3. 성능 최적화
- **파일 크기**: 각 룰 파일은 25줄 이하로 유지
- **Glob 최적화**: 너무 광범위한 패턴 피하기
- **컨텍스트 관리**: 필요한 정보만 포함
- **중복 제거**: 여러 룰에서 중복되는 내용 통합

### 4. 팀 협업
```markdown
# .cursor/rules/README.md (팀 가이드)
# Cursor Rules 사용 가이드

## 룰 추가/수정 시
1. 적절한 번호 대역 선택
2. Description과 Globs 정확히 작성
3. 팀원들과 변경사항 공유
4. 테스트 후 커밋

## 파일 네이밍 규칙
- 001-099: 핵심 룰 (신중히 수정)
- 100-199: 도메인 룰 (자유롭게 추가)
- 200-299: 패턴 룰 (실험적 가능)
```

이러한 구조적 접근법을 통해 Cursor 룰을 체계적으로 관리하고, AI와의 협업 효율성을 극대화할 수 있습니다. 중요한 것은 프로젝트의 특성에 맞게 적절히 커스터마이징하는 것입니다! 🎯
