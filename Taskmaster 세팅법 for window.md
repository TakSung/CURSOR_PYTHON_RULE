# Windows 10/11 + Chocolatey 기반 Google Gemini API + Taskmaster AI + Cursor 1.0 완전 설정 가이드

이 가이드는 Windows 10/11 환경에서 Chocolatey 패키지 매니저를 활용하여 NVM을 통한 Node.js 18.20.8 버전 관리, Google Gemini API, 그리고 Taskmaster AI를 Cursor 1.0에 통합하는 완전한 방법을 단계별로 설명합니다.

## 📋 목차
1. [Windows 사전 준비사항](#1-windows-사전-준비사항)
2. [Chocolatey 설치](#2-chocolatey-설치)
3. [NVM을 통한 Node.js 버전 관리](#3-nvm을-통한-nodejs-버전-관리)
4. [Google Gemini API 키 생성](#4-google-gemini-api-키-생성)
5. [Cursor 1.0 설치 및 설정](#5-cursor-10-설치-및-설정)
6. [Taskmaster AI MCP 서버 설정](#6-taskmaster-ai-mcp-서버-설정)
7. [프로젝트 초기화 및 사용법](#7-프로젝트-초기화-및-사용법)
8. [검증 및 테스트](#8-검증-및-테스트)
9. [Windows 전용 문제 해결](#9-windows-전용-문제-해결)

---

## 1. Windows 사전 준비사항

### 관리자 권한 확인
Windows에서 패키지 관리자를 사용하려면 관리자 권한이 필요합니다. 모든 설치 과정은 관리자 권한으로 실행된 PowerShell에서 진행해야 합니다.

### PowerShell 실행 정책 설정
Windows에서는 보안상 PowerShell 스크립트 실행이 제한되어 있습니다. 이를 변경해야 합니다.

1. **시작 메뉴** → **PowerShell** 검색
2. **Windows PowerShell**을 **우클릭** → **관리자 권한으로 실행**
3. 다음 명령어 실행:

```powershell
# PowerShell 실행 정책 확인
Get-ExecutionPolicy

# 스크립트 실행을 허용하도록 정책 변경 (현재 세션만)
Set-ExecutionPolicy Bypass -Scope Process -Force

# 또는 영구적으로 변경하려면 (권장하지 않음, 보안 위험)
# Set-ExecutionPolicy RemoteSigned -Force
```

---

## 2. Chocolatey 설치

Chocolatey는 Windows용 패키지 매니저로, Linux의 apt-get이나 macOS의 Homebrew와 같은 역할을 수행합니다. 명령줄을 통해 소프트웨어를 쉽게 설치, 업데이트, 제거할 수 있습니다.

### Chocolatey 설치 명령어
관리자 권한 PowerShell에서 다음 명령어를 실행합니다:

```powershell
# Chocolatey 설치 (공식 설치 스크립트)
Set-ExecutionPolicy Bypass -Scope Process -Force; 
[System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; 
iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
```

### 설치 확인
```powershell
# Chocolatey 버전 확인
choco --version

# Chocolatey 기본 정보 확인
choco
```

성공적으로 설치되면 Chocolatey 버전 정보와 사용 가능한 명령어들이 표시됩니다.

---

## 3. NVM을 통한 Node.js 버전 관리

### NVM-Windows 설치
NVM(Node Version Manager)을 사용하면 여러 Node.js 버전을 쉽게 관리할 수 있습니다. Windows에서는 nvm-windows를 사용합니다.

```powershell
# Chocolatey를 통한 NVM 설치
choco install nvm -y

# 설치 확인
nvm --version
```

### PowerShell 재시작
NVM 설치 후에는 환경 변수가 제대로 적용되도록 PowerShell을 다시 시작해야 합니다.

1. 현재 PowerShell 창을 닫습니다
2. 다시 **관리자 권한으로 PowerShell을 실행**합니다

### Node.js 18.20.8 설치 및 설정
이제 NVM을 사용하여 특정 Node.js 버전을 설치하고 관리할 수 있습니다.

```powershell
# 사용 가능한 Node.js 버전 확인
nvm list available

# Node.js 18.20.8 설치
nvm install 18.20.8

# 설치된 버전들 확인
nvm list

# Node.js 18.20.8을 기본 버전으로 사용
nvm use 18.20.8

# 설치 및 버전 확인
node --version    # v18.20.8이 출력되어야 함
npm --version     # npm 버전 확인
```

### 환경 변수 영구 설정
Windows에서 Node.js 버전을 영구적으로 기본값으로 설정하려면:

```powershell
# 현재 사용 중인 Node.js 버전을 기본값으로 설정
# (새 PowerShell 세션에서도 이 버전이 자동으로 활성화됨)
nvm use 18.20.8

# 디폴트로 지정
nvm alias default 18.20.8
```

---

## 4. Google Gemini API 키 생성

### Google AI Studio 접속
1. [Google AI Studio](https://aistudio.google.com) 웹사이트에 접속
2. Google 계정으로 로그인
3. **"Get API Key"** 버튼을 클릭

### API 키 생성 과정
1. **"Create API key in new project"** 옵션 선택
2. Google APIs 서비스 약관과 Gemini API 추가 약관에 동의
3. API 키가 자동으로 생성됩니다 (안전한 곳에 복사하여 보관)

### Windows 환경 변수 설정
Windows에서 환경 변수를 설정하는 방법은 여러 가지가 있습니다:

#### 방법 1: PowerShell을 통한 임시 설정 (현재 세션만)
```powershell
# 현재 PowerShell 세션에서만 유효
$env:GOOGLE_API_KEY = "your_actual_gemini_api_key_here"

# 확인
echo $env:GOOGLE_API_KEY
```

#### 방법 2: 영구 환경 변수 설정 (권장)
```powershell
# 사용자 수준에서 영구 환경 변수 설정
[Environment]::SetEnvironmentVariable("GOOGLE_API_KEY", "your_actual_gemini_api_key_here", "User")

# 시스템 전체에서 설정하려면 (관리자 권한 필요)
# [Environment]::SetEnvironmentVariable("GOOGLE_API_KEY", "your_actual_gemini_api_key_here", "Machine")
```

#### 방법 3: GUI를 통한 설정
1. **시작 메뉴** → **"환경 변수"** 검색
2. **"시스템 환경 변수 편집"** 클릭
3. **"환경 변수"** 버튼 클릭
4. **"사용자 변수"** 섹션에서 **"새로 만들기"** 클릭
5. **변수 이름**: `GOOGLE_API_KEY`
6. **변수 값**: 생성한 API 키 입력

---

## 5. Cursor 1.0 설치 및 설정

### Chocolatey를 통한 Cursor 설치
```powershell
# Cursor 설치
choco install cursor -y

# 설치 확인
cursor --version
```

### 첫 실행 및 기본 설정
1. Cursor를 실행합니다
2. 계정 연결 또는 무료 플랜을 선택합니다
3. **Settings** (Ctrl+,) → **Model**에서 선호하는 AI 모델을 선택합니다
다음 링크를 복사하여 브라우저 주소창에 붙여넣고 실행하세요:

```
cursor://anysphere.cursor-deeplink/mcp/install?name=taskmaster-ai&config=eyJjb21tYW5kIjoibnB4IiwiYXJncyI6WyIteSIsIi0tcGFja2FnZT10YXNrLW1hc3Rlci1haSIsInRhc2stbWFzdGVyLWFpIl0sImVudiI6eyJBTlRIUk9QSUNfQVBJX0tFWSI6IllPVVJfQU5USFJPUElDX0FQSV9LRVlfSEVSRSIsIlBFUlBMRVhJVFlfQVBJX0tFWSI6IllPVVJfUEVSUExFWElUWV9BUElfS0VZX0hFUkUiLCJPUEVOQUlfQVBJX0tFWSI6IllPVVJfT1BFTkFJX0tFWV9IRVJFIiwiR09PR0xFX0FQSV9LRVkiOiJZT1VSX0dPT0dMRV9LRVlfSEVSRSIsIk1JU1RSQUxfQVBJX0tFWSI6IllPVVJfTUlTVFJBTF9LRVlfSEVSRSIsIk9QRU5ST1VURVJfQVBJX0tFWSI6IllPVVJfT1BFTlJPVVRFUl9LRVlfSEVSRSIsIlhBSV9BUElfS0VZIjoiWU9VUl9YQUlfS0VZX0hFUkUiLCJBWlVSRV9PUEVOQUlfQVBJX0tFWSI6IllPVVJfQVpVUkVfS0VZX0hFUkUiLCJPTExBTUFfQVBJX0tFWSI6IllPVVJfT0xMQU1BX0FQSV9LRVlfSEVSRSJ9fQo=
```

### MCP 설정 인터페이스 접근
1. (Ctrl+Shift+J)를 눌러서 설정 열기
3. MCP 서버 관리 인터페이스를 확인합니다

# Cursor에서 Task Master AI 설정 가이드

## 1. MCP를 통한 자동 설치 (권장 방법)


## 2. 수동 MCP 설정

### 2.1 Cursor 설정 열기
1. Cursor에서 `Ctrl+Shift+J` (또는 `Cmd+Shift+J` on Mac) 눌러서 설정 열기
2. 왼쪽 메뉴에서 **MCP** 탭 클릭

### 2.2 MCP 서버 설정
아래 JSON 설정을 MCP 설정에 추가:

```json
{
  "mcpServers": {
    "taskmaster-ai": {
      "command": "npx",
      "args": ["-y", "--package=task-master-ai", "task-master-ai"],
      "env": {
        "ANTHROPIC_API_KEY": "YOUR_ANTHROPIC_API_KEY_HERE",
        "PERPLEXITY_API_KEY": "YOUR_PERPLEXITY_API_KEY_HERE",
        "OPENAI_API_KEY": "YOUR_OPENAI_KEY_HERE",
        "GOOGLE_API_KEY": "YOUR_GOOGLE_KEY_HERE",
        "MISTRAL_API_KEY": "YOUR_MISTRAL_KEY_HERE",
        "OPENROUTER_API_KEY": "YOUR_OPENROUTER_KEY_HERE",
        "XAI_API_KEY": "YOUR_XAI_KEY_HERE",
        "AZURE_OPENAI_API_KEY": "YOUR_AZURE_KEY_HERE",
        "OLLAMA_API_KEY": "YOUR_OLLAMA_API_KEY_HERE"
      },
      "type": "stdio"
    }
  }
}
```

---

## 6. Taskmaster AI MCP 서버 설정

### 방법 1: 원클릭 MCP 서버 설치 (권장)
Cursor 1.0에서는 공식 MCP 서버 디렉토리를 통해 쉽게 서버를 추가할 수 있습니다:

1. Cursor의 **Settings** → **MCP**로 이동
2. **"Browse MCP Servers"** 버튼 클릭
3. 검색창에 "taskmaster" 입력
4. **Taskmaster AI** 서버를 찾아 **"Add to Cursor"** 클릭
5. 환경 변수 설정에서 `GOOGLE_API_KEY` 입력

### 방법 2: 수동 MCP 서버 추가
직접 서버를 추가하려면:

1. **Settings** → **MCP**에서 **"Add New MCP Server"** 클릭
2. 다음 정보를 입력:

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

### Node.js 버전 확인 및 테스트
MCP 서버가 올바른 Node.js 버전을 사용하는지 확인:

```powershell
# 현재 Node.js 버전 확인
node --version  # v18.20.8이 출력되어야 함

# Taskmaster AI 패키지 테스트
npx -y --package=task-master-ai task-master-ai --help
```

### 연결 상태 확인
- MCP 서버 목록에서 **녹색 점**: 정상 연결
- **빨간 점**: 연결 실패 (API 키 또는 Node.js 버전 확인 필요)

---

## 7. 프로젝트 초기화 및 사용법

### 새 프로젝트 디렉토리 생성
```powershell
# 프로젝트 디렉토리 생성
mkdir my-ai-project
cd my-ai-project

# Cursor로 프로젝트 열기
cursor .
```

### Taskmaster AI 초기화
Cursor의 채팅 인터페이스 (Ctrl+L)에서 다음과 같이 요청:

```
Taskmaster AI를 초기화해주세요. 새 프로젝트를 시작하고 싶습니다.
```

### PRD (Product Requirements Document) 작성
Taskmaster AI가 생성한 `.taskmaster/docs/prd.txt` 파일을 편집:

```markdown
# 프로젝트 제목: Windows용 AI 할 일 관리 데스크톱 앱

## 개요
Windows 10/11에서 작동하는 데스크톱 할 일 관리 애플리케이션으로, 
자연어 입력을 통해 작업을 추가하고 AI가 자동으로 우선순위를 설정합니다.

## 핵심 기능
- 자연어를 통한 작업 입력 및 관리
- AI 기반 자동 우선순위 설정
- Windows 알림 시스템 통합
- 다크/라이트 테마 지원

## 기술 요구사항
- Frontend: Electron + React + TypeScript
- Backend: Node.js 18.20.8 + Express
- Database: SQLite (로컬 저장)
- AI: Google Gemini API
- 패키징: Electron Builder
```

### Taskmaster AI 워크플로우 실행
Cursor 채팅에서 다음과 같이 진행:

```
PRD를 기반으로 개발 작업들을 생성해주세요.

첫 번째 작업부터 시작하겠습니다. 어떤 작업을 먼저 해야 할까요?

Electron 환경 설정을 도와주세요.
```

---

## 8. 검증 및 테스트

### 시스템 전체 확인
```powershell
# Node.js 및 npm 버전 확인
node --version   # v18.20.8
npm --version

# Chocolatey 패키지 목록 확인
choco list --local-only

# 환경 변수 확인
echo $env:GOOGLE_API_KEY
```

### Cursor MCP 연결 테스트
1. **Settings** → **MCP**에서 taskmaster-ai 서버 상태 확인
2. 녹색 점이 표시되어야 함

### 기본 기능 테스트
Cursor 채팅에서:

```
Taskmaster AI가 정상적으로 작동하는지 확인해주세요.

간단한 테스트 PRD를 만들고 작업을 생성해보세요.
```

---

## 9. Windows 전용 문제 해결

### 자주 발생하는 Windows 관련 문제들

#### 문제 1: PowerShell 실행 정책 오류
```
오류: 이 시스템에서 스크립트를 실행할 수 없으므로...
```

**해결책:**
```powershell
# 관리자 권한 PowerShell에서 실행
Set-ExecutionPolicy RemoteSigned -Force

# 또는 현재 세션만
Set-ExecutionPolicy Bypass -Scope Process -Force
```

#### 문제 2: Node.js 버전 인식 실패
NVM 설치 후 Node.js가 인식되지 않는 경우:

**해결책:**
```powershell
# PowerShell 완전히 종료 후 관리자 권한으로 재시작
# NVM 상태 확인
nvm list

# Node.js 재설정
nvm use 18.20.8

# 환경 변수 새로고침
refreshenv
```

#### 문제 3: 환경 변수 인식 실패
```powershell
# 현재 세션에서 환경 변수 재로드
refreshenv

# 또는 수동으로 다시 설정
$env:GOOGLE_API_KEY = "your_api_key_here"

# 영구 설정 다시 확인
[Environment]::GetEnvironmentVariable("GOOGLE_API_KEY", "User")
```

#### 문제 4: Chocolatey 명령어 인식 안됨
```powershell
# PATH 환경 변수 확인
echo $env:PATH

# Chocolatey 수동 추가 (필요시)
$env:PATH += ";C:\ProgramData\chocolatey\bin"

# 또는 PowerShell 재시작
```

#### 문제 5: 방화벽 또는 바이러스 백신 차단
Windows Defender나 다른 바이러스 백신이 설치를 차단할 수 있습니다:

**해결책:**
1. **Windows 보안** → **바이러스 및 위협 방지**
2. **실시간 보호** 일시적으로 비활성화
3. 설치 완료 후 다시 활성화

### Windows 최적화 팁

#### PowerShell 프로필 설정
자주 사용하는 명령어들을 자동으로 로드하도록 PowerShell 프로필을 설정:

```powershell
# PowerShell 프로필 생성/편집
notepad $PROFILE

# 다음 내용 추가:
# Node.js 버전 자동 설정
nvm use 18.20.8

# 자주 사용하는 별칭 설정
Set-Alias -Name cc -Value "choco"
Set-Alias -Name code -Value "cursor"
```

#### Windows Terminal 설정 (권장)
PowerShell 대신 Windows Terminal을 사용하면 더 나은 개발 경험을 얻을 수 있습니다:

```powershell
# Windows Terminal 설치
choco install microsoft-windows-terminal -y
```

---
# Gemini CLI

```bash
npx https://github.com/google-gemini/gemini-cil
npm install -g @google/gemini-cli

gemini
```



---

## 🎯 핵심 포인트 요약

Windows에서 Chocolatey를 활용한 개발 환경 구축은 다음과 같은 장점을 제공합니다:

**패키지 관리의 일관성**: Chocolatey를 통해 모든 개발 도구를 명령줄에서 일관되게 관리할 수 있습니다. 이는 Linux나 macOS 사용자들이 익숙한 패키지 관리 방식을 Windows에서도 동일하게 사용할 수 있게 해줍니다.

**버전 충돌 방지**: NVM을 통한 Node.js 버전 관리는 서로 다른 프로젝트에서 요구하는 다양한 Node.js 버전들을 효과적으로 격리하고 관리할 수 있게 해줍니다.

**자동화된 설치 과정**: 수동으로 여러 웹사이트를 방문하여 설치 파일을 다운로드하고 실행하는 번거로움 없이, 모든 도구를 명령어 한 줄로 설치할 수 있습니다.

**개발 환경의 재현성**: 팀원들이 동일한 명령어 세트를 실행하여 완전히 동일한 개발 환경을 구축할 수 있어, "내 컴퓨터에서는 잘 되는데요" 문제를 방지할 수 있습니다.

Windows에서도 이제 Linux나 macOS와 동등한 수준의 개발 도구 관리 경험을 얻을 수 있습니다!

---

## 📚 추가 리소스

- [Chocolatey 공식 문서](https://docs.chocolatey.org/)
- [NVM-Windows GitHub](https://github.com/coreybutler/nvm-windows)
- [Cursor 1.0 공식 문서](https://docs.cursor.com)
- [Google Gemini API 문서](https://ai.google.dev/gemini-api/docs)
- [Taskmaster AI GitHub](https://github.com/eyaltoledano/claude-task-master)

Windows 10/11에서 최신 AI 개발 환경이 완벽하게 구축되었습니다! 🚀
