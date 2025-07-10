# Python 3.13 Cursor 룰 작성 완전 가이드 - 2025년 최신 버전

## 개요

Python 3.13과 현대적인 개발 도구를 위한 Cursor 룰 가이드입니다. Python 3.13의 최신 기능들과 FastAPI, Pydantic, Poetry 등 모던 Python 생태계를 활용한 효율적인 개발 환경을 구축할 수 있습니다.

## 1. Python 3.13 주요 신기능

### 1.1 핵심 개선사항
- **새로운 대화형 인터프리터**: 멀티라인 편집과 컬러 지원
- **실험적 Free-threaded 모드**: GIL 없이 실행 가능 (PEP 703)
- **JIT 컴파일러**: 실험적 Just-In-Time 컴파일러 (PEP 744)
- **컬러화된 오류 메시지**: 기본적으로 트레이스백에 컬러 지원
- **향상된 locals() 의미론**: PEP 667에 따른 정의된 의미론
- **타입 매개변수 기본값**: 제네릭 타입에 기본값 지원

### 1.2 성능 개선
- **mimalloc 포함**: 메모리 할당 성능 향상
- **docstring 들여쓰기 제거**: .pyc 파일 크기 감소
- **SQLite 백엔드**: dbm 모듈의 새로운 기본 백엔드

## 2. 현대적인 Python 개발 스택 (2025)

### 2.1 필수 도구
- **Python 3.13**: 최신 언어 기능
- **FastAPI**: 고성능 비동기 웹 프레임워크
- **Pydantic v2**: 데이터 검증 및 시리얼라이제이션
- **Poetry**: 의존성 관리 및 패키징
- **SQLAlchemy 2.0**: 비동기 ORM
- **pytest**: 테스팅 프레임워크
- **Ruff**: 초고속 린터 및 포매터
- **mypy**: 정적 타입 검사

### 2.2 프로젝트 구조
```
project/
├── pyproject.toml          # Poetry 설정
├── README.md
├── .gitignore
├── .cursorrules            # 레거시 룰 (선택사항)
├── .cursor/
│   └── rules/              # 새로운 MDC 룰들
│       ├── 001-core.mdc
│       ├── 100-fastapi.mdc
│       └── 200-testing.mdc
├── src/
│   └── project_name/
│       ├── __init__.py
│       ├── main.py         # FastAPI 앱
│       ├── api/            # API 라우터들
│       ├── models/         # SQLAlchemy 모델
│       ├── schemas/        # Pydantic 스키마
│       ├── services/       # 비즈니스 로직
│       ├── database/       # DB 설정
│       └── utils/
└── tests/
    ├── conftest.py
    ├── test_api/
    └── test_services/
```

## 3. 핵심 MDC 룰 파일들

### 3.1 001-core.mdc - 핵심 Python 3.13 룰

```markdown
---
description: Core Python 3.13 development guidelines and modern best practices
globs: **/*.py, **/pyproject.toml
alwaysApply: true
---

# Python 3.13 핵심 개발 룰

## AI 페르소나
당신은 Python 3.13의 최신 기능을 숙지한 시니어 Python 개발자입니다.
현대적인 개발 도구와 best practice를 적극 활용하며, 타입 힌트와 비동기 프로그래밍에 전문성을 가지고 있습니다.

## Python 3.13 필수 기능 활용

### 타입 힌트 및 제네릭
- Python 3.13의 새로운 타입 매개변수 기본값 사용
- Union 대신 | 연산자 우선 사용 (Python 3.10+)
- Generic 타입 적극 활용

```python
# Python 3.13 스타일
from typing import TypeVar, Generic

T = TypeVar('T', default=str)  # 기본값 지원

class Container(Generic[T]):
    def __init__(self, value: T) -> None:
        self.value = value

# Union 대신 | 사용
def process_data(data: str | int | None = None) -> dict[str, any]:
    pass
```

### 비동기 프로그래밍
- async/await 적극 활용
- contextvar 사용으로 컨텍스트 관리
- asyncio.run() 함수 우선 사용

### 모던 클래스 정의
- dataclasses 또는 Pydantic 모델 우선 사용
- __slots__ 활용으로 메모리 최적화
- property decorator 적극 활용

## 코딩 스타일

### 네이밍 컨벤션
- 변수/함수: snake_case
- 클래스: PascalCase
- 상수: UPPER_SNAKE_CASE
- private 멤버: _leading_underscore

### 파일 구조
- 한 파일당 하나의 주요 클래스/기능
- __init__.py에서 공개 API 명시적 정의
- 순환 import 방지

## 사용 금지 패턴

❌ 타입 힌트 없는 함수
```python
def process_user(user):  # 타입 힌트 없음
    return user.name
```

❌ 구식 Union 사용
```python
from typing import Union
def get_value() -> Union[str, int]:
    pass
```

❌ mutable 기본 인수
```python
def add_item(items=[]):  # 위험한 패턴
    pass
```

## 권장 패턴

✅ 완전한 타입 힌트
```python
from collections.abc import Sequence

def process_users(users: Sequence[User]) -> list[UserResponse]:
    return [UserResponse.from_user(user) for user in users]
```

✅ 현대적 Union 문법
```python
def get_value() -> str | int:
    pass
```

✅ dataclass 활용
```python
from dataclasses import dataclass, field

@dataclass(slots=True, frozen=True)
class User:
    id: int
    name: str
    tags: list[str] = field(default_factory=list)
```

## 의존성 관리
- Poetry 사용 필수
- pyproject.toml로 모든 설정 관리
- semantic versioning 준수

## 검증 체크리스트
- [ ] 모든 함수에 타입 힌트 적용
- [ ] async 함수는 적절히 await 사용
- [ ] Pydantic 모델로 데이터 검증
- [ ] pytest로 테스트 코드 작성
- [ ] Ruff로 코드 포매팅 및 린팅
- [ ] mypy 타입 검사 통과
```

### 3.2 100-fastapi.mdc - FastAPI 전용 룰

```markdown
---
description: FastAPI development patterns and best practices for high-performance APIs
globs: **/api/**/*.py, **/main.py, **/routers/**/*.py
alwaysApply: false
---

# FastAPI 개발 가이드

## FastAPI 아키텍처 패턴

### 프로젝트 구조
```
src/project/
├── main.py              # FastAPI 앱 엔트리포인트
├── api/
│   ├── __init__.py
│   ├── dependencies.py  # 의존성 주입
│   └── routes/          # 라우터 모듈들
├── schemas/             # Pydantic 스키마
├── models/              # SQLAlchemy 모델
├── services/            # 비즈니스 로직
└── database/            # DB 설정
```

### 핵심 원칙
1. **의존성 주입 활용**: 재사용 가능한 컴포넌트
2. **Pydantic 스키마 분리**: 요청/응답 모델 명확히 구분
3. **비동기 우선**: async/await 적극 활용
4. **자동 문서화**: OpenAPI 스키마 자동 생성

## 필수 사용 패턴

### FastAPI 앱 설정
```python
from fastapi import FastAPI, Depends
from fastapi.middleware.cors import CORSMiddleware
from contextlib import asynccontextmanager

@asynccontextmanager
async def lifespan(app: FastAPI):
    # 시작시 초기화
    print("앱 시작")
    yield
    # 종료시 정리
    print("앱 종료")

app = FastAPI(
    title="Project API",
    description="API 설명",
    version="1.0.0",
    lifespan=lifespan
)

# CORS 설정
app.add_middleware(
    CORSMiddleware,
    allow_origins=["*"],  # 프로덕션에서는 구체적으로 지정
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)
```

### Pydantic 스키마 정의
```python
from pydantic import BaseModel, Field, ConfigDict
from datetime import datetime

class UserBase(BaseModel):
    name: str = Field(..., min_length=1, max_length=100)
    email: str = Field(..., pattern=r'^[\w\.-]+@[\w\.-]+\.\w+$')

class UserCreate(UserBase):
    password: str = Field(..., min_length=8)

class UserResponse(UserBase):
    model_config = ConfigDict(from_attributes=True)
    
    id: int
    created_at: datetime
    is_active: bool = True
```

### 의존성 주입 활용
```python
from fastapi import Depends, HTTPException, status
from sqlalchemy.ext.asyncio import AsyncSession

async def get_db() -> AsyncSession:
    async with get_async_session() as session:
        try:
            yield session
        finally:
            await session.close()

async def get_current_user(
    token: str = Depends(oauth2_scheme),
    db: AsyncSession = Depends(get_db)
) -> User:
    try:
        payload = jwt.decode(token, SECRET_KEY, algorithms=[ALGORITHM])
        username: str = payload.get("sub")
        if username is None:
            raise credentials_exception
    except JWTError:
        raise credentials_exception
    
    user = await get_user_by_username(db, username=username)
    if user is None:
        raise credentials_exception
    return user
```

### 라우터 정의
```python
from fastapi import APIRouter, Depends, HTTPException, status
from typing import Sequence

router = APIRouter(prefix="/users", tags=["users"])

@router.post("/", response_model=UserResponse, status_code=status.HTTP_201_CREATED)
async def create_user(
    user_data: UserCreate,
    db: AsyncSession = Depends(get_db)
) -> UserResponse:
    """새 사용자 생성"""
    # 이메일 중복 검사
    existing_user = await get_user_by_email(db, user_data.email)
    if existing_user:
        raise HTTPException(
            status_code=status.HTTP_400_BAD_REQUEST,
            detail="이미 등록된 이메일입니다."
        )
    
    # 사용자 생성
    user = await create_user_service(db, user_data)
    return UserResponse.model_validate(user)

@router.get("/", response_model=list[UserResponse])
async def get_users(
    skip: int = 0,
    limit: int = 100,
    db: AsyncSession = Depends(get_db),
    current_user: User = Depends(get_current_user)
) -> Sequence[User]:
    """사용자 목록 조회"""
    users = await get_users_service(db, skip=skip, limit=limit)
    return users
```

### 예외 처리
```python
from fastapi import HTTPException, Request
from fastapi.responses import JSONResponse
from fastapi.exceptions import RequestValidationError

@app.exception_handler(RequestValidationError)
async def validation_exception_handler(request: Request, exc: RequestValidationError):
    return JSONResponse(
        status_code=status.HTTP_422_UNPROCESSABLE_ENTITY,
        content={
            "detail": "입력 데이터가 유효하지 않습니다.",
            "errors": exc.errors()
        }
    )

class UserNotFoundError(Exception):
    def __init__(self, user_id: int):
        self.user_id = user_id
        super().__init__(f"User {user_id} not found")

@app.exception_handler(UserNotFoundError)
async def user_not_found_handler(request: Request, exc: UserNotFoundError):
    return JSONResponse(
        status_code=status.HTTP_404_NOT_FOUND,
        content={"detail": f"사용자 {exc.user_id}를 찾을 수 없습니다."}
    )
```

## 성능 최적화

### 비동기 데이터베이스 연결
```python
from sqlalchemy.ext.asyncio import create_async_engine, AsyncSession
from sqlalchemy.orm import sessionmaker

engine = create_async_engine(
    DATABASE_URL,
    echo=False,  # 프로덕션에서는 False
    pool_size=20,
    max_overflow=30,
    pool_pre_ping=True
)

async_session = sessionmaker(
    engine, class_=AsyncSession, expire_on_commit=False
)
```

### 백그라운드 태스크
```python
from fastapi import BackgroundTasks

@router.post("/send-email/")
async def send_email_endpoint(
    email_data: EmailSchema,
    background_tasks: BackgroundTasks
):
    background_tasks.add_task(send_email_task, email_data.to, email_data.subject)
    return {"message": "이메일 전송 요청이 접수되었습니다."}

async def send_email_task(to_email: str, subject: str):
    # 이메일 전송 로직
    pass
```

## 보안 best practice

### JWT 인증
```python
from passlib.context import CryptContext
from jose import JWTError, jwt
from datetime import datetime, timedelta

pwd_context = CryptContext(schemes=["bcrypt"], deprecated="auto")

def verify_password(plain_password: str, hashed_password: str) -> bool:
    return pwd_context.verify(plain_password, hashed_password)

def get_password_hash(password: str) -> str:
    return pwd_context.hash(password)

def create_access_token(data: dict, expires_delta: timedelta | None = None):
    to_encode = data.copy()
    if expires_delta:
        expire = datetime.utcnow() + expires_delta
    else:
        expire = datetime.utcnow() + timedelta(minutes=15)
    
    to_encode.update({"exp": expire})
    encoded_jwt = jwt.encode(to_encode, SECRET_KEY, algorithm=ALGORITHM)
    return encoded_jwt
```

### 입력 검증 및 보안 헤더
```python
from fastapi.security import HTTPBearer
from fastapi.middleware.trustedhost import TrustedHostMiddleware

# 보안 헤더 미들웨어
@app.middleware("http")
async def add_security_headers(request: Request, call_next):
    response = await call_next(request)
    response.headers["X-Content-Type-Options"] = "nosniff"
    response.headers["X-Frame-Options"] = "DENY"
    response.headers["X-XSS-Protection"] = "1; mode=block"
    return response

# 신뢰할 수 있는 호스트만 허용
app.add_middleware(
    TrustedHostMiddleware,
    allowed_hosts=["example.com", "*.example.com"]
)
```

## 사용 금지 패턴

❌ 동기 함수 혼용
```python
@app.get("/users/")
def get_users():  # async 없음
    return db.query(User).all()  # 동기 쿼리
```

❌ 예외 처리 누락
```python
@app.get("/users/{user_id}")
async def get_user(user_id: int):
    user = await get_user_by_id(user_id)
    return user  # user가 None일 수 있음
```

❌ Pydantic 모델 없이 딕셔너리 반환
```python
@app.post("/users/")
async def create_user(user_data: dict):  # Pydantic 없음
    return {"id": 1, "name": user_data["name"]}
```

## 검증 체크리스트
- [ ] 모든 엔드포인트에 적절한 타입 힌트 적용
- [ ] Pydantic 스키마로 요청/응답 검증
- [ ] 의존성 주입으로 재사용 가능한 컴포넌트 구성
- [ ] async/await 올바르게 사용
- [ ] 적절한 HTTP 상태 코드 반환
- [ ] 예외 처리 및 에러 메시지 구현
- [ ] 보안 헤더 및 CORS 설정
- [ ] API 문서화 자동 생성 확인
```

### 3.3 200-testing.mdc - 테스팅 룰

```markdown
---
description: Modern Python testing patterns with pytest, factory-boy, and async testing
globs: **/tests/**/*.py, **/test_*.py, **/*_test.py
alwaysApply: false
---

# Python 3.13 테스팅 가이드

## 테스팅 도구 스택
- **pytest**: 메인 테스팅 프레임워크
- **pytest-asyncio**: 비동기 테스트 지원
- **factory-boy**: 테스트 데이터 생성
- **httpx**: FastAPI 테스트 클라이언트
- **pytest-cov**: 코드 커버리지
- **freezegun**: 시간 관련 테스트

## 프로젝트 테스트 구조
```
tests/
├── conftest.py          # pytest 설정 및 fixture
├── test_api/
│   ├── test_users.py
│   └── test_auth.py
├── test_services/
│   └── test_user_service.py
├── test_models/
│   └── test_user_model.py
└── factories/
    └── user_factory.py
```

## 핵심 테스트 패턴

### conftest.py 설정
```python
import pytest
import pytest_asyncio
from httpx import AsyncClient
from sqlalchemy.ext.asyncio import AsyncSession, create_async_engine
from sqlalchemy.orm import sessionmaker
from fastapi.testclient import TestClient

from src.main import app
from src.database import get_db, Base

# 테스트 데이터베이스 엔진
TEST_DATABASE_URL = "sqlite+aiosqlite:///./test.db"

engine = create_async_engine(
    TEST_DATABASE_URL,
    connect_args={"check_same_thread": False}
)

async_session = sessionmaker(
    engine, class_=AsyncSession, expire_on_commit=False
)

@pytest_asyncio.fixture
async def async_session() -> AsyncSession:
    async with engine.begin() as conn:
        await conn.run_sync(Base.metadata.create_all)
    
    async with async_session() as session:
        try:
            yield session
        finally:
            await session.close()
    
    async with engine.begin() as conn:
        await conn.run_sync(Base.metadata.drop_all)

@pytest_asyncio.fixture
async def async_client(async_session: AsyncSession) -> AsyncClient:
    def get_test_db():
        return async_session
    
    app.dependency_overrides[get_db] = get_test_db
    
    async with AsyncClient(app=app, base_url="http://test") as client:
        yield client
    
    app.dependency_overrides.clear()
```

### Factory Boy 사용
```python
# factories/user_factory.py
import factory
from factory.alchemy import SQLAlchemyModelFactory
from src.models.user import User

class UserFactory(SQLAlchemyModelFactory):
    class Meta:
        model = User
        sqlalchemy_session_persistence = "commit"
    
    id = factory.Sequence(lambda n: n)
    name = factory.Faker("name")
    email = factory.Faker("email")
    is_active = True
    created_at = factory.Faker("date_time")

class ActiveUserFactory(UserFactory):
    is_active = True

class InactiveUserFactory(UserFactory):
    is_active = False
```

### FastAPI API 테스트
```python
# test_api/test_users.py
import pytest
from httpx import AsyncClient
from sqlalchemy.ext.asyncio import AsyncSession

from tests.factories.user_factory import UserFactory

@pytest.mark.asyncio
class TestUserAPI:
    
    async def test_create_user_success(self, async_client: AsyncClient):
        """사용자 생성 성공 테스트"""
        user_data = {
            "name": "홍길동",
            "email": "hong@example.com",
            "password": "strongpassword123"
        }
        
        response = await async_client.post("/users/", json=user_data)
        
        assert response.status_code == 201
        data = response.json()
        assert data["name"] == user_data["name"]
        assert data["email"] == user_data["email"]
        assert "id" in data
        assert "password" not in data  # 비밀번호는 응답에 포함되지 않음
    
    async def test_create_user_duplicate_email(self, async_client: AsyncClient, async_session: AsyncSession):
        """중복 이메일로 사용자 생성 실패 테스트"""
        # Given: 기존 사용자 생성
        existing_user = UserFactory(email="existing@example.com")
        async_session.add(existing_user)
        await async_session.commit()
        
        # When: 같은 이메일로 새 사용자 생성 시도
        user_data = {
            "name": "새사용자",
            "email": "existing@example.com",
            "password": "password123"
        }
        response = await async_client.post("/users/", json=user_data)
        
        # Then: 400 에러 반환
        assert response.status_code == 400
        assert "이미 등록된 이메일" in response.json()["detail"]
    
    async def test_get_users_with_pagination(self, async_client: AsyncClient, async_session: AsyncSession):
        """페이지네이션을 포함한 사용자 목록 조회 테스트"""
        # Given: 여러 사용자 생성
        users = UserFactory.create_batch(15)
        for user in users:
            async_session.add(user)
        await async_session.commit()
        
        # When: 첫 번째 페이지 요청 (limit=10)
        response = await async_client.get("/users/?skip=0&limit=10")
        
        # Then: 10개 사용자 반환
        assert response.status_code == 200
        data = response.json()
        assert len(data) == 10
        
        # When: 두 번째 페이지 요청
        response = await async_client.get("/users/?skip=10&limit=10")
        
        # Then: 나머지 5개 사용자 반환
        assert response.status_code == 200
        data = response.json()
        assert len(data) == 5

    @pytest.mark.parametrize("invalid_email", [
        "invalid-email",
        "@example.com",
        "user@",
        "",
    ])
    async def test_create_user_invalid_email(self, async_client: AsyncClient, invalid_email: str):
        """잘못된 이메일 형식으로 사용자 생성 실패 테스트"""
        user_data = {
            "name": "홍길동",
            "email": invalid_email,
            "password": "password123"
        }
        
        response = await async_client.post("/users/", json=user_data)
        
        assert response.status_code == 422
        assert "validation error" in response.json()["detail"].lower()
```

### 서비스 레이어 테스트
```python
# test_services/test_user_service.py
import pytest
from sqlalchemy.ext.asyncio import AsyncSession

from src.services.user_service import UserService, UserNotFoundError
from src.schemas.user import UserCreate
from tests.factories.user_factory import UserFactory

@pytest.mark.asyncio
class TestUserService:
    
    async def test_create_user_success(self, async_session: AsyncSession):
        """사용자 생성 서비스 테스트"""
        # Given
        service = UserService(async_session)
        user_data = UserCreate(
            name="홍길동",
            email="hong@example.com",
            password="strongpassword"
        )
        
        # When
        created_user = await service.create_user(user_data)
        
        # Then
        assert created_user.name == user_data.name
        assert created_user.email == user_data.email
        assert created_user.id is not None
        assert created_user.is_active is True
    
    async def test_get_user_by_id_not_found(self, async_session: AsyncSession):
        """존재하지 않는 사용자 조회 테스트"""
        # Given
        service = UserService(async_session)
        non_existent_id = 999
        
        # When & Then
        with pytest.raises(UserNotFoundError) as exc_info:
            await service.get_user_by_id(non_existent_id)
        
        assert str(exc_info.value) == f"User {non_existent_id} not found"
    
    async def test_update_user_success(self, async_session: AsyncSession):
        """사용자 정보 업데이트 테스트"""
        # Given
        service = UserService(async_session)
        user = UserFactory(name="원래이름")
        async_session.add(user)
        await async_session.commit()
        
        update_data = {"name": "변경된이름"}
        
        # When
        updated_user = await service.update_user(user.id, update_data)
        
        # Then
        assert updated_user.name == "변경된이름"
        assert updated_user.id == user.id
```

### 비동기 테스트 최적화
```python
import pytest
from unittest.mock import AsyncMock
from freezegun import freeze_time
from datetime import datetime

@pytest.mark.asyncio
async def test_async_operation_with_mock():
    """비동기 함수 모킹 테스트"""
    # Given
    mock_service = AsyncMock()
    mock_service.get_data.return_value = {"result": "success"}
    
    # When
    result = await mock_service.get_data()
    
    # Then
    assert result == {"result": "success"}
    mock_service.get_data.assert_called_once()

@freeze_time("2025-01-01 12:00:00")
@pytest.mark.asyncio
async def test_time_dependent_operation():
    """시간 의존적 기능 테스트"""
    # Given
    expected_time = datetime(2025, 1, 1, 12, 0, 0)
    
    # When
    result = await some_time_dependent_function()
    
    # Then
    assert result.created_at == expected_time
```

## 테스트 작성 Best Practice

### Given-When-Then 패턴
```python
async def test_user_creation():
    # Given: 테스트 데이터 준비
    user_data = UserCreate(name="홍길동", email="hong@test.com")
    
    # When: 실제 동작 수행
    result = await create_user(user_data)
    
    # Then: 결과 검증
    assert result.name == user_data.name
    assert result.id is not None
```

### 검증 체크리스트
- [ ] 모든 API 엔드포인트에 테스트 작성
- [ ] 성공/실패 시나리오 모두 테스트
- [ ] Given-When-Then 패턴 사용
- [ ] Factory Boy로 테스트 데이터 생성
- [ ] 비동기 함수는 pytest.mark.asyncio 사용
- [ ] Mock/Patch 적절히 활용
- [ ] 80% 이상 코드 커버리지 달성
```

## 4. 추가 전문화 룰 파일들

### 4.1 300-database.mdc - SQLAlchemy 2.0 룰

```markdown
---
description: SQLAlchemy 2.0 async patterns and database best practices
globs: **/models/**/*.py, **/database/**/*.py, **/alembic/**/*.py
alwaysApply: false
---

# SQLAlchemy 2.0 비동기 데이터베이스 가이드

## 핵심 원칙
- SQLAlchemy 2.0 스타일 사용 필수
- 비동기 세션 활용
- 명시적 트랜잭션 관리
- Type 힌트와 함께 사용

## 모델 정의 패턴

### 기본 모델 구조
```python
from sqlalchemy import String, Integer, Boolean, DateTime, ForeignKey
from sqlalchemy.orm import DeclarativeBase, Mapped, mapped_column, relationship
from sqlalchemy.sql import func
from datetime import datetime
from typing import Optional

class Base(DeclarativeBase):
    pass

class User(Base):
    __tablename__ = "users"
    
    # Primary Key
    id: Mapped[int] = mapped_column(Integer, primary_key=True, index=True)
    
    # Required fields
    name: Mapped[str] = mapped_column(String(100), nullable=False)
    email: Mapped[str] = mapped_column(String(255), unique=True, nullable=False, index=True)
    
    # Optional fields
    phone: Mapped[Optional[str]] = mapped_column(String(20), nullable=True)
    
    # Boolean with default
    is_active: Mapped[bool] = mapped_column(Boolean, default=True)
    
    # Timestamps
    created_at: Mapped[datetime] = mapped_column(DateTime(timezone=True), server_default=func.now())
    updated_at: Mapped[Optional[datetime]] = mapped_column(DateTime(timezone=True), onupdate=func.now())
    
    # Relationships
    posts: Mapped[list["Post"]] = relationship("Post", back_populates="author", lazy="selectin")
```

### 관계 설정
```python
from sqlalchemy.orm import relationship

class Post(Base):
    __tablename__ = "posts"
    
    id: Mapped[int] = mapped_column(Integer, primary_key=True)
    title: Mapped[str] = mapped_column(String(200), nullable=False)
    content: Mapped[str] = mapped_column(String, nullable=False)
    
    # Foreign Key
    author_id: Mapped[int] = mapped_column(ForeignKey("users.id"), nullable=False)
    
    # Relationship with lazy loading
    author: Mapped["User"] = relationship("User", back_populates="posts", lazy="joined")
    tags: Mapped[list["Tag"]] = relationship("Tag", secondary="post_tags", back_populates="posts")
```

## 비동기 데이터베이스 작업

### 세션 관리
```python
from sqlalchemy.ext.asyncio import create_async_engine, AsyncSession, async_sessionmaker
from contextlib import asynccontextmanager

# 엔진 생성
engine = create_async_engine(
    DATABASE_URL,
    echo=False,  # 개발시에만 True
    pool_size=20,
    max_overflow=30,
    pool_pre_ping=True,
    pool_recycle=3600,
)

# 세션 팩토리
async_session = async_sessionmaker(
    engine,
    class_=AsyncSession,
    expire_on_commit=False
)

# 의존성 주입용 세션 생성기
async def get_async_session() -> AsyncSession:
    async with async_session() as session:
        try:
            yield session
        finally:
            await session.close()
```

### CRUD 작업 패턴
```python
from sqlalchemy import select, update, delete
from sqlalchemy.orm import selectinload, joinedload
from typing import Sequence

class UserRepository:
    def __init__(self, session: AsyncSession):
        self.session = session
    
    async def create(self, user_data: UserCreate) -> User:
        """사용자 생성"""
        user = User(**user_data.model_dump())
        self.session.add(user)
        await self.session.commit()
        await self.session.refresh(user)
        return user
    
    async def get_by_id(self, user_id: int) -> User | None:
        """ID로 사용자 조회"""
        stmt = select(User).where(User.id == user_id)
        result = await self.session.execute(stmt)
        return result.scalar_one_or_none()
    
    async def get_by_email(self, email: str) -> User | None:
        """이메일로 사용자 조회"""
        stmt = select(User).where(User.email == email)
        result = await self.session.execute(stmt)
        return result.scalar_one_or_none()
    
    async def get_users_with_posts(self, skip: int = 0, limit: int = 100) -> Sequence[User]:
        """게시글과 함께 사용자 목록 조회 (N+1 방지)"""
        stmt = (
            select(User)
            .options(selectinload(User.posts))
            .offset(skip)
            .limit(limit)
        )
        result = await self.session.execute(stmt)
        return result.scalars().all()
    
    async def update(self, user_id: int, update_data: dict) -> User | None:
        """사용자 정보 업데이트"""
        stmt = (
            update(User)
            .where(User.id == user_id)
            .values(**update_data)
            .returning(User)
        )
        result = await self.session.execute(stmt)
        await self.session.commit()
        return result.scalar_one_or_none()
    
    async def delete(self, user_id: int) -> bool:
        """사용자 삭제"""
        stmt = delete(User).where(User.id == user_id)
        result = await self.session.execute(stmt)
        await self.session.commit()
        return result.rowcount > 0
```

### 트랜잭션 관리
```python
from sqlalchemy.exc import SQLAlchemyError

async def transfer_money(
    session: AsyncSession,
    from_account_id: int,
    to_account_id: int,
    amount: float
) -> bool:
    """계좌 이체 (트랜잭션 예제)"""
    try:
        async with session.begin():
            # 출금 계좌 잔액 확인 및 차감
            from_account = await session.get(Account, from_account_id)
            if not from_account or from_account.balance < amount:
                raise ValueError("잔액 부족")
            
            from_account.balance -= amount
            
            # 입금 계좌 잔액 증가
            to_account = await session.get(Account, to_account_id)
            if not to_account:
                raise ValueError("대상 계좌를 찾을 수 없습니다")
            
            to_account.balance += amount
            
            # 거래 내역 기록
            transaction = Transaction(
                from_account_id=from_account_id,
                to_account_id=to_account_id,
                amount=amount,
                transaction_type="transfer"
            )
            session.add(transaction)
            
        return True
        
    except SQLAlchemyError as e:
        await session.rollback()
        raise e
```

## 성능 최적화

### N+1 쿼리 방지
```python
# ❌ N+1 문제 발생
async def get_users_with_posts_bad():
    users = await session.execute(select(User))
    for user in users.scalars():
        # 각 사용자마다 추가 쿼리 실행됨
        posts = await session.execute(select(Post).where(Post.author_id == user.id))

# ✅ 올바른 방법 - Eager Loading
async def get_users_with_posts_good():
    stmt = select(User).options(selectinload(User.posts))
    result = await session.execute(stmt)
    return result.scalars().all()

# ✅ 대안 - Join 사용
async def get_users_with_posts_joined():
    stmt = select(User).options(joinedload(User.posts))
    result = await session.execute(stmt)
    return result.unique().scalars().all()
```

### 대용량 데이터 처리
```python
from sqlalchemy import text

async def bulk_insert_users(session: AsyncSession, users_data: list[dict]):
    """대량 데이터 삽입"""
    await session.execute(
        text("INSERT INTO users (name, email) VALUES (:name, :email)"),
        users_data
    )
    await session.commit()

async def stream_large_dataset(session: AsyncSession):
    """대용량 데이터 스트리밍"""
    stmt = select(User).execution_options(stream_results=True)
    result = await session.execute(stmt)
    
    async for row in result:
        user = row[0]
        # 메모리 효율적으로 처리
        yield user
```

## 마이그레이션 (Alembic)

### alembic.ini 설정
```ini
[alembic]
script_location = alembic
sqlalchemy.url = driver://user:pass@localhost/dbname

[post_write_hooks]
hooks = black
black.type = console_scripts
black.entrypoint = black
black.options = -l 79
```

### 마이그레이션 파일 작성
```python
"""사용자 테이블 생성

Revision ID: 001_create_users
Revises: 
Create Date: 2025-01-01 12:00:00.000000

"""
from alembic import op
import sqlalchemy as sa

# revision identifiers
revision = '001_create_users'
down_revision = None
branch_labels = None
depends_on = None

def upgrade() -> None:
    """사용자 테이블 생성"""
    op.create_table(
        'users',
        sa.Column('id', sa.Integer(), primary_key=True, index=True),
        sa.Column('name', sa.String(100), nullable=False),
        sa.Column('email', sa.String(255), nullable=False, unique=True, index=True),
        sa.Column('is_active', sa.Boolean(), default=True),
        sa.Column('created_at', sa.DateTime(timezone=True), server_default=sa.func.now()),
        sa.Column('updated_at', sa.DateTime(timezone=True), onupdate=sa.func.now()),
    )

def downgrade() -> None:
    """사용자 테이블 삭제"""
    op.drop_table('users')
```

## 사용 금지 패턴

❌ 동기 SQLAlchemy 사용
```python
from sqlalchemy.orm import sessionmaker  # 동기 세션

session = sessionmaker()
user = session.query(User).first()  # 구식 쿼리 방식
```

❌ 명시적 트랜잭션 관리 없음
```python
async def update_user_unsafe(session, user_id, data):
    user = await session.get(User, user_id)
    user.name = data["name"]
    # commit 없음 - 데이터가 저장되지 않을 수 있음
```

❌ N+1 쿼리 문제
```python
async def get_posts_with_authors_bad():
    posts = await session.execute(select(Post))
    for post in posts.scalars():
        author = await session.get(User, post.author_id)  # N+1 발생
```

## 검증 체크리스트
- [ ] SQLAlchemy 2.0 스타일 사용
- [ ] 모든 DB 작업에 비동기 세션 사용
- [ ] Mapped 타입 힌트 적용
- [ ] N+1 쿼리 방지 (eager loading)
- [ ] 적절한 트랜잭션 관리
- [ ] 인덱스 설정으로 성능 최적화
- [ ] Alembic으로 마이그레이션 관리
```

### 4.2 400-security.mdc - 보안 룰

```markdown
---
description: Python security best practices and vulnerability prevention
globs: **/api/**/*.py, **/auth/**/*.py, **/middleware/**/*.py
alwaysApply: false
---

# Python 보안 가이드

## 보안 원칙
1. **입력 검증**: 모든 사용자 입력 검증
2. **최소 권한**: 필요한 최소한의 권한만 부여
3. **암호화**: 민감한 데이터 암호화 저장
4. **로깅**: 보안 이벤트 로깅
5. **업데이트**: 의존성 정기 업데이트

## 인증 및 인가

### JWT 토큰 보안
```python
from jose import JWTError, jwt
from passlib.context import CryptContext
from datetime import datetime, timedelta
import secrets

# 강력한 시크릿 키 생성
SECRET_KEY = secrets.token_urlsafe(32)  # 프로덕션에서는 환경변수 사용
ALGORITHM = "HS256"
ACCESS_TOKEN_EXPIRE_MINUTES = 30

pwd_context = CryptContext(schemes=["bcrypt"], deprecated="auto")

def create_access_token(data: dict, expires_delta: timedelta | None = None):
    to_encode = data.copy()
    if expires_delta:
        expire = datetime.utcnow() + expires_delta
    else:
        expire = datetime.utcnow() + timedelta(minutes=15)
    
    to_encode.update({"exp": expire, "iat": datetime.utcnow()})
    encoded_jwt = jwt.encode(to_encode, SECRET_KEY, algorithm=ALGORITHM)
    return encoded_jwt

def verify_token(token: str) -> dict | None:
    try:
        payload = jwt.decode(token, SECRET_KEY, algorithms=[ALGORITHM])
        
        # 토큰 만료 시간 확인
        exp = payload.get("exp")
        if exp is None or datetime.utcnow() > datetime.fromtimestamp(exp):
            return None
            
        return payload
    except JWTError:
        return None
```

### 비밀번호 보안
```python
import secrets
from passlib.context import CryptContext

pwd_context = CryptContext(
    schemes=["bcrypt"],
    deprecated="auto",
    bcrypt__rounds=12  # 보안 강화를 위해 라운드 수 증가
)

def hash_password(password: str) -> str:
    """비밀번호 해싱"""
    return pwd_context.hash(password)

def verify_password(plain_password: str, hashed_password: str) -> bool:
    """비밀번호 검증"""
    return pwd_context.verify(plain_password, hashed_password)

def generate_random_password(length: int = 16) -> str:
    """안전한 랜덤 비밀번호 생성"""
    alphabet = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!@#$%^&*"
    return ''.join(secrets.choice(alphabet) for _ in range(length))

# 비밀번호 강도 검증
import re

def validate_password_strength(password: str) -> bool:
    """비밀번호 강도 검증"""
    if len(password) < 8:
        return False
    
    # 최소 하나의 대문자, 소문자, 숫자, 특수문자
    patterns = [
        r'[A-Z]',  # 대문자
        r'[a-z]',  # 소문자
        r'\d',     # 숫자
        r'[!@#$%^&*(),.?":{}|<>]'  # 특수문자
    ]
    
    return all(re.search(pattern, password) for pattern in patterns)
```

## 입력 검증 및 보안

### Pydantic 보안 검증
```python
from pydantic import BaseModel, validator, Field
import re
from typing import Optional

class SecureUserCreate(BaseModel):
    username: str = Field(..., min_length=3, max_length=20)
    email: str = Field(..., regex=r'^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,} )
    password: str = Field(..., min_length=8)
    phone: Optional[str] = Field(None, regex=r'^\+?1?\d{9,15} )
    
    @validator('username')
    def validate_username(cls, v):
        # SQL 인젝션 방지를 위한 특수문자 검증
        if not re.match(r'^[a-zA-Z0-9_]+ , v):
            raise ValueError('사용자명은 영문, 숫자, 언더스코어만 사용 가능합니다')
        
        # 예약어 검증
        reserved_words = ['admin', 'root', 'system', 'null', 'undefined']
        if v.lower() in reserved_words:
            raise ValueError('사용할 수 없는 사용자명입니다')
        
        return v
    
    @validator('password')
    def validate_password(cls, v):
        if not validate_password_strength(v):
            raise ValueError(
                '비밀번호는 8자 이상이며 대문자, 소문자, 숫자, 특수문자를 포함해야 합니다'
            )
        return v

# HTML 이스케이프
import html

def sanitize_input(text: str) -> str:
    """XSS 방지를 위한 입력 sanitization"""
    return html.escape(text.strip())

# SQL 인젝션 방지
from sqlalchemy import text

# ❌ 위험한 패턴
async def get_user_unsafe(session, user_id):
    query = f"SELECT * FROM users WHERE id = {user_id}"  # SQL 인젝션 위험
    return await session.execute(text(query))

# ✅ 안전한 패턴
async def get_user_safe(session, user_id: int):
    query = text("SELECT * FROM users WHERE id = :user_id")
    return await session.execute(query, {"user_id": user_id})
```

### 파일 업로드 보안
```python
import os
from pathlib import Path
from fastapi import UploadFile, HTTPException
import magic

ALLOWED_EXTENSIONS = {'.jpg', '.jpeg', '.png', '.gif', '.pdf', '.txt'}
MAX_FILE_SIZE = 10 * 1024 * 1024  # 10MB

async def validate_file_upload(file: UploadFile) -> bool:
    """파일 업로드 보안 검증"""
    
    # 파일 크기 검증
    content = await file.read()
    await file.seek(0)  # 포인터 리셋
    
    if len(content) > MAX_FILE_SIZE:
        raise HTTPException(status_code=413, detail="파일 크기가 너무 큽니다")
    
    # 파일 확장자 검증
    file_extension = Path(file.filename).suffix.lower()
    if file_extension not in ALLOWED_EXTENSIONS:
        raise HTTPException(status_code=400, detail="허용되지 않는 파일 형식입니다")
    
    # MIME 타입 검증 (확장자 조작 방지)
    mime_type = magic.from_buffer(content[:1024], mime=True)
    allowed_mimes = {
        '.jpg': 'image/jpeg',
        '.jpeg': 'image/jpeg',
        '.png': 'image/png',
        '.gif': 'image/gif',
        '.pdf': 'application/pdf',
        '.txt': 'text/plain'
    }
    
    if allowed_mimes.get(file_extension) != mime_type:
        raise HTTPException(status_code=400, detail="파일 형식이 일치하지 않습니다")
    
    return True

def generate_safe_filename(original_filename: str) -> str:
    """안전한 파일명 생성"""
    # 위험한 문자 제거
    safe_chars = re.sub(r'[^a-zA-Z0-9._-]', '', original_filename)
    
    # 타임스탬프 추가로 중복 방지
    timestamp = int(time.time())
    name, ext = os.path.splitext(safe_chars)
    
    return f"{name}_{timestamp}{ext}"
```

## 보안 미들웨어

### 보안 헤더 설정
```python
from fastapi import Request, Response
from fastapi.middleware.cors import CORSMiddleware
from fastapi.middleware.trustedhost import TrustedHostMiddleware

@app.middleware("http")
async def add_security_headers(request: Request, call_next):
    response = await call_next(request)
    
    # XSS 방지
    response.headers["X-Content-Type-Options"] = "nosniff"
    response.headers["X-Frame-Options"] = "DENY"
    response.headers["X-XSS-Protection"] = "1; mode=block"
    
    # HSTS (HTTPS 강제)
    response.headers["Strict-Transport-Security"] = "max-age=31536000; includeSubDomains"
    
    # CSP (Content Security Policy)
    response.headers["Content-Security-Policy"] = (
        "default-src 'self'; "
        "script-src 'self' 'unsafe-inline'; "
        "style-src 'self' 'unsafe-inline'; "
        "img-src 'self' data: https:; "
        "font-src 'self' https:; "
        "connect-src 'self'"
    )
    
    # 정보 누출 방지
    response.headers["Server"] = "WebServer"
    
    return response

# CORS 설정
app.add_middleware(
    CORSMiddleware,
    allow_origins=["https://yourdomain.com"],  # 프로덕션에서는 구체적으로 지정
    allow_credentials=True,
    allow_methods=["GET", "POST", "PUT", "DELETE"],
    allow_headers=["*"],
)

# 신뢰할 수 있는 호스트만 허용
app.add_middleware(
    TrustedHostMiddleware,
    allowed_hosts=["yourdomain.com", "*.yourdomain.com"]
)
```

### Rate Limiting
```python
from slowapi import Limiter, _rate_limit_exceeded_handler
from slowapi.util import get_remote_address
from slowapi.errors import RateLimitExceeded

limiter = Limiter(key_func=get_remote_address)
app.state.limiter = limiter
app.add_exception_handler(RateLimitExceeded, _rate_limit_exceeded_handler)

@app.post("/api/login")
@limiter.limit("5/minute")  # 분당 5회 제한
async def login(request: Request, credentials: UserLogin):
    # 로그인 로직
    pass

@app.post("/api/register")
@limiter.limit("3/hour")  # 시간당 3회 제한
async def register(request: Request, user_data: UserCreate):
    # 회원가입 로직
    pass
```

## 로깅 및 모니터링

### 보안 이벤트 로깅
```python
import logging
from datetime import datetime
from fastapi import Request

# 보안 로거 설정
security_logger = logging.getLogger("security")
security_handler = logging.FileHandler("security.log")
security_formatter = logging.Formatter(
    '%(asctime)s - %(levelname)s - %(message)s'
)
security_handler.setFormatter(security_formatter)
security_logger.addHandler(security_handler)
security_logger.setLevel(logging.WARNING)

def log_security_event(event_type: str, details: dict, request: Request = None):
    """보안 이벤트 로깅"""
    event_data = {
        "timestamp": datetime.utcnow().isoformat(),
        "event_type": event_type,
        "details": details
    }
    
    if request:
        event_data.update({
            "ip_address": request.client.host,
            "user_agent": request.headers.get("user-agent"),
            "endpoint": str(request.url)
        })
    
    security_logger.warning(f"SECURITY_EVENT: {event_data}")

# 사용 예시
async def login_attempt(request: Request, username: str, success: bool):
    if not success:
        log_security_event(
            "FAILED_LOGIN",
            {"username": username, "reason": "invalid_credentials"},
            request
        )
    else:
        log_security_event(
            "SUCCESSFUL_LOGIN",
            {"username": username},
            request
        )
```

## 환경변수 보안

### 민감한 정보 관리
```python
from pydantic_settings import BaseSettings
from functools import lru_cache

class Settings(BaseSettings):
    # 데이터베이스
    database_url: str
    
    # JWT
    secret_key: str = secrets.token_urlsafe(32)
    algorithm: str = "HS256"
    access_token_expire_minutes: int = 30
    
    # 외부 서비스
    smtp_server: str
    smtp_port: int = 587
    smtp_username: str
    smtp_password: str
    
    # 보안 설정
    allowed_hosts: list[str] = ["localhost", "127.0.0.1"]
    cors_origins: list[str] = ["http://localhost:3000"]
    
    class Config:
        env_file = ".env"
        env_file_encoding = "utf-8"

@lru_cache()
def get_settings():
    return Settings()

# .env 파일 예시
"""
DATABASE_URL=postgresql+asyncpg://user:password@localhost/dbname
SECRET_KEY=your-secret-key-here
SMTP_SERVER=smtp.gmail.com
SMTP_USERNAME=your-email@gmail.com
SMTP_PASSWORD=your-app-password
"""
```

## 사용 금지 패턴

❌ 하드코딩된 시크릿
```python
SECRET_KEY = "my-secret-key-123"  # 위험!
DATABASE_URL = "postgresql://user:pass@localhost/db"  # 위험!
```

❌ 약한 비밀번호 정책
```python
def weak_password_validation(password):
    return len(password) >= 4  # 너무 약함
```

❌ SQL 인젝션 취약점
```python
def get_user(user_id):
    query = f"SELECT * FROM users WHERE id = {user_id}"  # 위험!
```

❌ XSS 취약점
```python
def display_message(message):
    return f"<div>{message}</div>"  # HTML 이스케이프 없음
```

## 검증 체크리스트
- [ ] 모든 비밀번호 bcrypt로 해싱
- [ ] JWT 토큰에 적절한 만료시간 설정
- [ ] 모든 입력값 Pydantic으로 검증
- [ ] SQL 쿼리에 파라미터 바인딩 사용
- [ ] 파일 업로드시 확장자/MIME 타입 검증
- [ ] 보안 헤더 설정 (CSP, HSTS 등)
- [ ] Rate limiting 적용
- [ ] 민감한 정보 환경변수로 관리
- [ ] 보안 이벤트 로깅 구현
- [ ] HTTPS 사용 필수
```

## 5. 실전 사용 가이드

### 5.1 룰 파일 설치 및 활용
1. **.cursor/rules/** 디렉토리 생성
2. 필요한 .mdc 파일들 복사
3. Cursor 설정에서 룰 활성화 확인
4. 프로젝트별 customization

### 5.2 팀 협업 설정
```bash
# Poetry 프로젝트 초기화
poetry new my-fastapi-project
cd my-fastapi-project

# 의존성 추가
poetry add fastapi[standard] uvicorn sqlalchemy[asyncio] alembic pydantic-settings
poetry add --group dev pytest pytest-asyncio pytest-cov black ruff mypy

# 개발 환경 설정
poetry shell
```

### 5.3 지속적인 개선
- 월 1회 룰 파일 리뷰 및 업데이트
- 팀 피드백 수집 및 반영  
- Python 및 도구 업데이트에 따른 룰 수정

이 가이드를 통해 Python 3.13의 최신 기능과 현대적인 개발 도구를 활용한 고품질 코드를 작성할 수 있습니다.

## 6. 실전 예시 파일

### 6.1 완전한 FastAPI 앱 예시

```python
# src/main.py
from fastapi import FastAPI, Depends, HTTPException, status
from fastapi.middleware.cors import CORSMiddleware
from contextlib import asynccontextmanager
from sqlalchemy.ext.asyncio import AsyncSession

from .database import engine, get_db
from .api.routes import users, auth
from .models import Base

@asynccontextmanager
async def lifespan(app: FastAPI):
    """앱 생명주기 관리"""
    # 시작시 데이터베이스 테이블 생성
    async with engine.begin() as conn:
        await conn.run_sync(Base.metadata.create_all)
    
    print("✅ 애플리케이션 시작")
    yield
    
    # 종료시 정리 작업
    await engine.dispose()
    print("✅ 애플리케이션 종료")

# FastAPI 앱 생성
app = FastAPI(
    title="Modern Python API",
    description="Python 3.13 + FastAPI + SQLAlchemy 2.0을 사용한 현대적 API",
    version="1.0.0",
    lifespan=lifespan,
    docs_url="/docs",
    redoc_url="/redoc"
)

# CORS 미들웨어 설정
app.add_middleware(
    CORSMiddleware,
    allow_origins=["http://localhost:3000", "https://yourdomain.com"],
    allow_credentials=True,
    allow_methods=["GET", "POST", "PUT", "DELETE"],
    allow_headers=["*"],
)

# 보안 헤더 미들웨어
@app.middleware("http")
async def add_security_headers(request, call_next):
    response = await call_next(request)
    response.headers["X-Content-Type-Options"] = "nosniff"
    response.headers["X-Frame-Options"] = "DENY"
    response.headers["X-XSS-Protection"] = "1; mode=block"
    return response

# 라우터 등록
app.include_router(auth.router, prefix="/api/auth", tags=["인증"])
app.include_router(users.router, prefix="/api/users", tags=["사용자"])

@app.get("/")
async def root():
    """API 상태 확인"""
    return {
        "message": "Modern Python API with FastAPI",
        "version": "1.0.0",
        "python_version": "3.13",
        "status": "healthy"
    }

@app.get("/health")
async def health_check(db: AsyncSession = Depends(get_db)):
    """헬스 체크 (데이터베이스 연결 포함)"""
    try:
        # 데이터베이스 연결 테스트
        await db.execute("SELECT 1")
        return {"status": "healthy", "database": "connected"}
    except Exception as e:
        raise HTTPException(
            status_code=status.HTTP_503_SERVICE_UNAVAILABLE,
            detail=f"데이터베이스 연결 실패: {str(e)}"
        )
```

### 6.2 pyproject.toml 설정 예시

```toml
[tool.poetry]
name = "modern-python-api"
version = "1.0.0"
description = "Python 3.13과 FastAPI를 사용한 현대적 API"
authors = ["Your Name <your.email@example.com>"]
readme = "README.md"
packages = [{include = "src"}]

[tool.poetry.dependencies]
python = "^3.13"
fastapi = {extras = ["standard"], version = "^0.115.0"}
uvicorn = {extras = ["standard"], version = "^0.32.0"}
sqlalchemy = {extras = ["asyncio"], version = "^2.0.0"}
asyncpg = "^0.30.0"
alembic = "^1.14.0"
pydantic = {extras = ["email"], version = "^2.10.0"}
pydantic-settings = "^2.6.0"
python-jose = {extras = ["cryptography"], version = "^3.3.0"}
passlib = {extras = ["bcrypt"], version = "^1.7.4"}
python-multipart = "^0.0.12"

[tool.poetry.group.dev.dependencies]
pytest = "^8.3.0"
pytest-asyncio = "^0.24.0"
pytest-cov = "^6.0.0"
factory-boy = "^3.3.0"
httpx = "^0.28.0"
ruff = "^0.8.0"
mypy = "^1.13.0"
black = "^24.0.0"
pre-commit = "^4.0.0"

[tool.poetry.group.test.dependencies]
pytest-xdist = "^3.6.0"
pytest-mock = "^3.14.0"
freezegun = "^1.5.0"

[build-system]
requires = ["poetry-core"]
build-backend = "poetry.core.masonry.api"

# Ruff 설정 (린터 + 포매터)
[tool.ruff]
target-version = "py313"
line-length = 88
select = [
    "E",   # pycodestyle errors
    "W",   # pycodestyle warnings
    "F",   # pyflakes
    "I",   # isort
    "C4",  # flake8-comprehensions
    "B",   # flake8-bugbear
    "UP",  # pyupgrade
    "N",   # pep8-naming
    "S",   # bandit (보안)
]
ignore = [
    "E501",  # line too long
    "S101",  # use of assert
]

[tool.ruff.format]
quote-style = "double"
indent-style = "space"

[tool.ruff.isort]
known-first-party = ["src"]

# MyPy 설정
[tool.mypy]
python_version = "3.13"
strict = true
warn_return_any = true
warn_unused_configs = true
disallow_untyped_defs = true
disallow_incomplete_defs = true
check_untyped_defs = true
disallow_untyped_decorators = true

[[tool.mypy.overrides]]
module = "tests.*"
ignore_errors = true

# Pytest 설정
[tool.pytest.ini_options]
testpaths = ["tests"]
python_files = ["test_*.py", "*_test.py"]
python_classes = ["Test*"]
python_functions = ["test_*"]
addopts = [
    "-ra",
    "--strict-markers",
    "--strict-config",
    "--cov=src",
    "--cov-report=term-missing",
    "--cov-report=html",
    "--cov-fail-under=80"
]
asyncio_mode = "auto"
markers = [
    "slow: marks tests as slow",
    "integration: marks tests as integration tests",
]

# Coverage 설정
[tool.coverage.run]
source = ["src"]
omit = [
    "*/tests/*",
    "*/migrations/*",
    "*/__init__.py",
]

[tool.coverage.report]
exclude_lines = [
    "pragma: no cover",
    "def __repr__",
    "raise AssertionError",
    "raise NotImplementedError",
    "if __name__ == .__main__.:",
    "if TYPE_CHECKING:",
]
```

### 6.3 Docker 설정 (선택사항)

```dockerfile
# Dockerfile
FROM python:3.13-slim

# 작업 디렉토리 설정
WORKDIR /app

# 시스템 의존성 설치
RUN apt-get update && apt-get install -y \
    build-essential \
    curl \
    && rm -rf /var/lib/apt/lists/*

# Poetry 설치
RUN pip install poetry

# Poetry 설정
ENV POETRY_NO_INTERACTION=1 \
    POETRY_VENV_IN_PROJECT=1 \
    POETRY_CACHE_DIR=/tmp/poetry_cache

# 의존성 파일 복사
COPY pyproject.toml poetry.lock ./

# 의존성 설치
RUN poetry install --only=main && rm -rf $POETRY_CACHE_DIR

# 소스 코드 복사
COPY src/ ./src/

# 포트 노출
EXPOSE 8000

# 앱 실행
CMD ["poetry", "run", "uvicorn", "src.main:app", "--host", "0.0.0.0", "--port", "8000"]
```

```yaml
# docker-compose.yml
version: '3.8'

services:
  api:
    build: .
    ports:
      - "8000:8000"
    environment:
      - DATABASE_URL=postgresql+asyncpg://postgres:password@db:5432/appdb
      - SECRET_KEY=your-secret-key-here
    depends_on:
      - db
    volumes:
      - ./src:/app/src
    command: poetry run uvicorn src.main:app --host 0.0.0.0 --port 8000 --reload

  db:
    image: postgres:16
    environment:
      - POSTGRES_DB=appdb
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=password
    ports:
      - "5432:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data

  redis:
    image: redis:7-alpine
    ports:
      - "6379:6379"
    volumes:
      - redis_data:/data

volumes:
  postgres_data:
  redis_data:
```

### 6.4 Pre-commit 훅 설정

```yaml
# .pre-commit-config.yaml
repos:
  - repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v4.6.0
    hooks:
      - id: trailing-whitespace
      - id: end-of-file-fixer
      - id: check-yaml
      - id: check-added-large-files
      - id: check-merge-conflict

  - repo: https://github.com/astral-sh/ruff-pre-commit
    rev: v0.8.0
    hooks:
      - id: ruff
        args: [--fix, --exit-non-zero-on-fix]
      - id: ruff-format

  - repo: https://github.com/pre-commit/mirrors-mypy
    rev: v1.13.0
    hooks:
      - id: mypy
        additional_dependencies: [types-all]
        args: [--strict, --ignore-missing-imports]

  - repo: https://github.com/PyCQA/bandit
    rev: 1.7.10
    hooks:
      - id: bandit
        args: [-c, pyproject.toml]
        additional_dependencies: ["bandit[toml]"]
```

## 7. 최적화 및 모니터링

### 7.1 성능 최적화 팁

```python
# 비동기 최적화
import asyncio
from typing import Sequence

async def fetch_multiple_users_optimized(
    user_ids: Sequence[int],
    session: AsyncSession
) -> list[User]:
    """여러 사용자를 효율적으로 조회"""
    # 하나의 쿼리로 모든 사용자 조회
    stmt = select(User).where(User.id.in_(user_ids))
    result = await session.execute(stmt)
    return result.scalars().all()

# 캐싱 활용
from functools import lru_cache
import redis.asyncio as redis

redis_client = redis.from_url("redis://localhost:6379")

@lru_cache(maxsize=128)
def get_cached_settings():
    """설정값 캐싱"""
    return Settings()

async def get_user_with_cache(user_id: int) -> User | None:
    """Redis 캐시를 활용한 사용자 조회"""
    cache_key = f"user:{user_id}"
    
    # 캐시에서 먼저 확인
    cached_user = await redis_client.get(cache_key)
    if cached_user:
        return User.model_validate_json(cached_user)
    
    # 데이터베이스에서 조회
    user = await get_user_by_id(user_id)
    if user:
        # 캐시에 저장 (10분)
        await redis_client.setex(
            cache_key, 
            600, 
            user.model_dump_json()
        )
    
    return user
```

### 7.2 모니터링 및 로깅

```python
# 구조화된 로깅
import structlog
from datetime import datetime

# Structlog 설정
structlog.configure(
    processors=[
        structlog.stdlib.filter_by_level,
        structlog.stdlib.add_logger_name,
        structlog.stdlib.add_log_level,
        structlog.stdlib.PositionalArgumentsFormatter(),
        structlog.processors.TimeStamper(fmt="iso"),
        structlog.processors.StackInfoRenderer(),
        structlog.processors.format_exc_info,
        structlog.processors.UnicodeDecoder(),
        structlog.processors.JSONRenderer()
    ],
    context_class=dict,
    logger_factory=structlog.stdlib.LoggerFactory(),
    cache_logger_on_first_use=True,
)

logger = structlog.get_logger()

# 미들웨어로 요청 로깅
@app.middleware("http")
async def log_requests(request: Request, call_next):
    start_time = time.time()
    
    # 요청 로깅
    logger.info(
        "request_started",
        method=request.method,
        url=str(request.url),
        client_ip=request.client.host,
        user_agent=request.headers.get("user-agent")
    )
    
    response = await call_next(request)
    
    # 응답 로깅
    process_time = time.time() - start_time
    logger.info(
        "request_completed",
        method=request.method,
        url=str(request.url),
        status_code=response.status_code,
        process_time=process_time
    )
    
    return response

# 메트릭 수집 (Prometheus 스타일)
from prometheus_client import Counter, Histogram, generate_latest

REQUEST_COUNT = Counter(
    'http_requests_total',
    'Total HTTP requests',
    ['method', 'endpoint', 'status']
)

REQUEST_DURATION = Histogram(
    'http_request_duration_seconds',
    'HTTP request duration'
)

@app.get("/metrics")
async def metrics():
    """Prometheus 메트릭 엔드포인트"""
    return Response(generate_latest(), media_type="text/plain")
```

## 8. 배포 및 운영

### 8.1 환경별 설정

```python
# src/config.py
from enum import Enum
from pydantic_settings import BaseSettings

class Environment(str, Enum):
    DEVELOPMENT = "development"
    TESTING = "testing"
    STAGING = "staging"
    PRODUCTION = "production"

class Settings(BaseSettings):
    environment: Environment = Environment.DEVELOPMENT
    debug: bool = False
    
    # 데이터베이스
    database_url: str
    database_pool_size: int = 10
    database_max_overflow: int = 20
    
    # Redis
    redis_url: str = "redis://localhost:6379"
    
    # JWT
    secret_key: str
    access_token_expire_minutes: int = 30
    
    # 외부 서비스
    smtp_server: str = ""
    smtp_port: int = 587
    
    # 로깅
    log_level: str = "INFO"
    
    @property
    def is_production(self) -> bool:
        return self.environment == Environment.PRODUCTION
    
    @property
    def is_development(self) -> bool:
        return self.environment == Environment.DEVELOPMENT

    class Config:
        env_file = ".env"
        case_sensitive = False

# 환경별 설정 파일
# .env.development
"""
ENVIRONMENT=development
DEBUG=true
DATABASE_URL=postgresql+asyncpg://user:pass@localhost/dev_db
LOG_LEVEL=DEBUG
"""

# .env.production
"""
ENVIRONMENT=production
DEBUG=false
DATABASE_URL=postgresql+asyncpg://user:pass@prod-server/prod_db
LOG_LEVEL=INFO
SECRET_KEY=very-secure-production-key
"""
```

### 8.2 CI/CD 파이프라인 (GitHub Actions)

```yaml
# .github/workflows/ci.yml
name: CI/CD Pipeline

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        python-version: ["3.13"]

    services:
      postgres:
        image: postgres:16
        env:
          POSTGRES_PASSWORD: postgres
          POSTGRES_DB: test_db
        options: >-
          --health-cmd pg_isready
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5
        ports:
          - 5432:5432

    steps:
    - uses: actions/checkout@v4
    
    - name: Set up Python ${{ matrix.python-version }}
      uses: actions/setup-python@v5
      with:
        python-version: ${{ matrix.python-version }}
    
    - name: Install Poetry
      uses: snok/install-poetry@v1
      with:
        version: latest
        virtualenvs-create: true
        virtualenvs-in-project: true
    
    - name: Load cached venv
      id: cached-poetry-dependencies
      uses: actions/cache@v4
      with:
        path: .venv
        key: venv-${{ runner.os }}-${{ steps.setup-python.outputs.python-version }}-${{ hashFiles('**/poetry.lock') }}
    
    - name: Install dependencies
      if: steps.cached-poetry-dependencies.outputs.cache-hit != 'true'
      run: poetry install --no-interaction --no-root
    
    - name: Install project
      run: poetry install --no-interaction
    
    - name: Run code quality checks
      run: |
        poetry run ruff check .
        poetry run ruff format --check .
        poetry run mypy src/
    
    - name: Run tests
      run: |
        poetry run pytest --cov=src --cov-report=xml
      env:
        DATABASE_URL: postgresql+asyncpg://postgres:postgres@localhost/test_db
    
    - name: Upload coverage to Codecov
      uses: codecov/codecov-action@v4
      with:
        file: ./coverage.xml

  security:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v4
    - name: Run Bandit Security Scan
      uses: securecodewarrior/github-action-add-sarif@v1
      with:
        sarif-file: bandit-report.sarif
    - name: Run Safety Check
      run: |
        pip install safety
        safety check

  deploy:
    needs: [test, security]
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    
    steps:
    - uses: actions/checkout@v4
    
    - name: Deploy to production
      run: |
        echo "Deploy to production server"
        # 실제 배포 로직 구현
```

## 9. 마무리

이 가이드를 통해 Python 3.13의 최신 기능과 현대적인 개발 도구를 활용한 고품질 코드를 작성할 수 있습니다. 

### 9.1 주요 포인트 요약

1. **Python 3.13 신기능 활용**: Free-threading, JIT 컴파일러, 향상된 타입 시스템
2. **현대적 도구 스택**: FastAPI + Pydantic + SQLAlchemy 2.0 + Poetry
3. **보안 우선**: 입력 검증, 인증/인가, 보안 헤더, 로깅
4. **테스트 중심**: pytest + 비동기 테스트 + 높은 커버리지
5. **성능 최적화**: 비동기 프로그래밍 + 캐싱 + 모니터링

### 9.2 다음 단계

1. **프로젝트에 적용**: 실제 프로젝트에서 이 룰들을 적용해보세요
2. **팀과 공유**: 팀원들과 룰을 공유하고 피드백을 수집하세요
3. **지속적 개선**: 정기적으로 룰을 업데이트하고 최적화하세요
4. **커뮤니티 참여**: Python 커뮤니티에서 최신 동향을 계속 학습하세요

Modern Python development with Python 3.13 - 더 빠르고, 더 안전하고, 더 효율적인 코드를 작성하세요! 🚀