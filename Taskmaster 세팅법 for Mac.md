# 맥 + Homebrew 기반 Google Gemini API + Taskmaster AI + Cursor 1.0 완전 설정 가이드

이 가이드는 2025년 Cursor 1.0의 최신 기능인 원클릭 MCP 설정을 활용하여 Google Gemini API와 Taskmaster AI를 통합하는 방법을 단계별로 안내합니다.

## 📋 목차
1. [사전 준비사항](#1-사전-준비사항)
2. [Google Gemini API 키 생성](#2-google-gemini-api-키-생성)
3. [Cursor 1.0 설치 및 설정](#3-cursor-10-설치-및-설정)
4. [Taskmaster AI MCP 서버 설정 (원클릭)](#4-taskmaster-ai-mcp-서버-설정-원클릭)
5. [프로젝트 초기화 및 사용법](#5-프로젝트-초기화-및-사용법)
6. [검증 및 테스트](#6-검증-및-테스트)
7. [문제 해결](#7-문제-해결)

---

## 1. 사전 준비사항

### Node.js 버전 관리 (NVM 설치)
Node Version Manager(NVM)를 사용하면 여러 Node.js 버전을 효율적으로 관리할 수 있습니다. 특히 MCP 서버들은 특정 Node.js 버전에서 최적화되어 작동하므로, 버전 관리가 중요합니다.

```bash
# NVM 설치 (Homebrew 사용)
brew install nvm

# NVM 환경 설정을 위해 .zshrc에 추가
echo 'export NVM_DIR="$HOME/.nvm"' >> ~/.zshrc
echo '[ -s "/opt/homebrew/share/nvm/nvm.sh" ] && \. "/opt/homebrew/share/nvm/nvm.sh"' >> ~/.zshrc
echo '[ -s "/opt/homebrew/share/nvm/bash_completion" ] && \. "/opt/homebrew/share/nvm/bash_completion"' >> ~/.zshrc

# 설정 적용
source ~/.zshrc

# NVM 설치 확인
nvm --version
```

### Node.js 18.20.8 설치 및 설정
Node.js 18.20.8 버전은 MCP 서버들과의 호환성이 뛰어나며, 장기 지원(LTS) 버전으로 안정성이 검증되었습니다.

```bash
# Node.js 18.20.8 설치
nvm install 20.19.0

# 기본 버전으로 설정 (새 터미널 세션에서 자동으로 이 버전 사용)
nvm alias default 20.19.0

# 현재 세션에서 18.20.8 사용
nvm use 20.19.0

# 설치 및 버전 확인
node --version    # v18.20.8
npm --version     # npm 버전 확인
npx --version     # npx 버전 확인

# 설치된 Node.js 버전들 확인
nvm list
```

### Cursor 1.0 설치
```bash
# Homebrew를 통해 Cursor 설치
brew install --cask cursor
```

설치 후 터미널에서 `cursor` 명령어 사용을 위해:
1. Cursor 실행
2. **View** → **Command Palette** (⌘+Shift+P)
3. "Install 'cursor' command in PATH" 검색 후 실행

---

## 2. Google Gemini API 키 생성

### Google AI Studio 접속
1. [Google AI Studio](https://aistudio.google.com) 방문
2. Google 계정으로 로그인
3. **"Get API Key"** 버튼 클릭

### API 키 생성
1. **"Create API key in new project"** 선택
2. Google APIs 서비스 약관 동의
3. API 키 자동 생성 (안전하게 복사 후 저장)

### 환경 변수 설정
```bash
# ~/.zshrc에 환경 변수 추가 (macOS 기본 쉘)
echo 'export GOOGLE_API_KEY="your_actual_gemini_api_key_here"' >> ~/.zshrc

# 설정 적용
source ~/.zshrc

# 확인
echo $GOOGLE_API_KEY
```

---

## 3. Cursor 1.0 설치 및 설정

### 첫 실행 설정
1. Cursor 실행
2. 계정 연결 또는 무료 플랜 선택
3. **Settings** (⌘+,) → **Model** → 선호하는 AI 모델 선택

### MCP 설정 접근
1. **Settings** (⌘+,) 열기
2. 좌측 사이드바에서 **"MCP"** 클릭
3. MCP 서버 관리 인터페이스 확인

---

## 4. Taskmaster AI MCP 서버 설정 (원클릭)

### 방법 1: 공식 MCP 서버 디렉토리 (권장)
1. Cursor에서 **Settings** → **MCP** 이동
2. **"Browse MCP Servers"** 클릭
3. 검색창에 "taskmaster" 입력
4. **Taskmaster AI** 찾아서 **"Add to Cursor"** 클릭
5. 원클릭 설치 완료!

### 방법 2: 수동 MCP 서버 추가
Node.js 18.20.8 환경에서 MCP 서버를 수동으로 추가하는 경우, Settings → MCP에서 **"Add New MCP Server"** 클릭 후 다음 정보 입력:

```json
{
  "name": "taskmaster-ai",
  "command": "npx",
  "args": ["-y", "--package=task-master-ai", "task-master-ai"],
  "env": {
    "GOOGLE_API_KEY": "your_gemini_api_key_here",
    "NODE_VERSION": "18.20.8"
  }
}
```

**중요**: MCP 서버가 올바른 Node.js 버전을 사용하도록 하기 위해 터미널에서 다음을 실행해주세요:

```bash
# 현재 터미널 세션에서 Node.js 18.20.8 활성화
nvm use 20.19.0

# MCP 서버가 이 버전을 사용하는지 확인
node --version  # v20.19.0이 출력되어야 함

# Taskmaster AI 패키지가 올바르게 실행되는지 테스트
npx -y --package=task-master-ai task-master-ai --help
```

### API 키 설정 확인
MCP 서버 추가 후:
1. **Environment Variables** 섹션에서 `GOOGLE_API_KEY` 확인
2. 필요시 추가 API 키들도 설정 가능:
   - `ANTHROPIC_API_KEY` (Claude)
   - `OPENAI_API_KEY` (OpenAI)
   - `PERPLEXITY_API_KEY` (Perplexity)

### 연결 상태 확인
- MCP 서버 목록에서 **녹색 점** = 정상 연결
- **빨간 점** = 연결 실패 (API 키 또는 설정 확인 필요)

---

## 5. 프로젝트 초기화 및 사용법

### 새 프로젝트 설정
```bash
# 프로젝트 디렉토리 생성
mkdir my-ai-project
cd my-ai-project

# Taskmaster AI 초기화 (MCP를 통해 자동 실행됨)
# Cursor 채팅에서 다음과 같이 요청:
```

### Cursor 채팅에서 Taskmaster AI 사용
Cursor의 채팅 인터페이스 (⌘+L)에서 다음과 같이 요청:

```
Taskmaster AI를 초기화해주세요. 새 프로젝트를 시작하고 싶습니다.
```

### PRD (Product Requirements Document) 작성
Taskmaster AI가 `.taskmaster/docs/prd.txt` 파일을 생성하면 다음과 같이 작성:

```markdown
# 프로젝트 제목: AI 기반 할 일 관리 앱

## 개요
사용자가 자연어로 작업을 추가하고 AI가 자동으로 우선순위를 설정하는 앱

## 핵심 기능
- 자연어 작업 입력
- AI 기반 우선순위 자동 설정
- 스마트 알림 시스템
- 진행 상황 시각화

## 기술 요구사항
- Frontend: React + TypeScript
- Backend: Node.js + Express
- Database: PostgreSQL
- AI: Google Gemini API
```

### Taskmaster AI 워크플로우 사용
Cursor 채팅에서:

```
PRD를 파싱해서 개발 작업들을 생성해주세요.

다음에 해야 할 작업은 무엇인가요?

첫 번째 작업을 구현하는 데 도움을 주세요.
```

---

## 6. 검증 및 테스트

### MCP 연결 상태 확인
1. **Settings** → **MCP**에서 taskmaster-ai 서버 상태 확인
2. 녹색 점이 표시되어야 함

### 기본 기능 테스트
Cursor 채팅에서 테스트:

```
Taskmaster AI가 제대로 작동하는지 확인해주세요.
```

### 작업 생성 테스트
```
간단한 예제 PRD를 만들고 작업들을 생성해주세요.
```

---

## 7. 문제 해결

### 자주 발생하는 문제들

#### 문제 1: MCP 서버 연결 실패 (빨간 점)
**해결책:**
```bash
# 먼저 올바른 Node.js 버전이 활성화되어 있는지 확인
nvm use 18.20.8
node --version  # v18.20.8이 출력되어야 함

# API 키 재확인
echo $GOOGLE_API_KEY

# MCP 서버가 올바른 Node.js 버전으로 실행되는지 테스트
npx -y --package=task-master-ai task-master-ai --version

# Cursor 재시작 후 MCP 서버 재설정
```

#### 문제 2: Node.js 버전 충돌
이 문제는 시스템에 여러 Node.js 버전이 설치되어 있을 때 발생할 수 있습니다.

**해결책:**
```bash
# 현재 활성화된 Node.js 버전 확인
nvm current

# 18.20.8로 변경
nvm use 20.19.0

# 기본 버전 재설정
nvm alias default 20.19.0

# 새 터미널 세션에서도 올바른 버전이 사용되는지 확인
# (새 터미널 열고 다음 실행)
node --version
```

#### 문제 2: Taskmaster AI 명령어 인식 안됨
**해결책:**
1. MCP 서버가 활성화되어 있는지 확인
2. Chat에서 "Available tools" 확인
3. 필요시 MCP 서버 재시작

#### 문제 3: API 할당량 초과
**해결책:**
- Google AI Studio에서 사용량 확인
- 무료 티어 한도: 일일 1,500 요청
- 필요시 유료 플랜 고려

### 디버깅 팁

#### 환경 변수 확인
```bash
# 모든 관련 환경 변수 확인
env | grep -E "(GOOGLE|GEMINI|ANTHROPIC|OPENAI)_API_KEY"
```

#### MCP 서버 로그 확인
Cursor의 **Developer Tools** (⌘+Option+I)에서 콘솔 로그 확인

#### 권한 문제 해결
```bash
# npm 전역 권한 문제 시
sudo chown -R $(whoami) $(npm config get prefix)/{lib/node_modules,bin,share}
```

---

## 🎯 핵심 포인트 요약

1. **Cursor 1.0의 원클릭 MCP 설정** 기능으로 복잡한 JSON 설정 없이 간편하게 서버 추가 가능

2. **Google Gemini API**의 관대한 무료 티어 (일일 1,500 요청)로 충분한 테스트 가능

3. **환경 변수 설정**이 핵심 - `GOOGLE_API_KEY`가 올바르게 설정되어야 함

4. **Taskmaster AI**는 복잡한 프로젝트를 관리 가능한 작업으로 자동 분할

5. **실시간 피드백**을 통해 AI가 프로젝트 컨텍스트를 유지하며 작업 진행

---

## 📚 추가 리소스

- [Cursor 1.0 공식 문서](https://docs.cursor.com)
- [Google Gemini API 문서](https://ai.google.dev/gemini-api/docs)
- [Taskmaster AI GitHub](https://github.com/eyaltoledano/claude-task-master)
- [MCP 서버 디렉토리](https://cursor.directory/mcp)

이제 최신 Cursor 1.0과 Google Gemini API, Taskmaster AI의 완벽한 통합 환경이 구축되었습니다! 🚀