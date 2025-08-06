다음은 행동(behavior), 상태(state), 계산(computation)을 분리하는 주요 개발 방법론 및 아키텍처 패턴입니다:

---

## 🧹 1. **Separation of Concerns (SoC)**

* 프로그램을 기능별로 모듈화해 **presentation / business logic / data access**처럼 관심사를 분리하는 설계 원칙입니다.
* 런타임 행위(behavior), 데이터(state), 계산(computation)을 구조적으로 분리할 수 있어 유지보수성과 재사용성이 높아집니다.

---

## ⚙️ 2. **Command–Query Separation (CQS) & Command Query Responsibility Segregation (CQRS)**

* **CQS**는 “명령(command)은 상태를 변경하고, 쿼리(query)는 단순히 값을 반환해야 한다”고 규정해 **행동과 상태 변화 로직을 분리**합니다.
* **CQRS**는 이 원칙을 시스템 아키텍처 수준으로 확장해, **읽기 모델과 쓰기 모델**을 아예 별도로 유지·운영하는 방식입니다.

---

## 🔄 3. **Strategy Pattern**

* 런타임 행위(behavior)를 **추상 인터페이스(behavior interface)**로 분리하고, 이를 구현한 클래스에 계산(computation) 로직을 위임합니다.
* 예: `Car` 클래스는 `IBrakeBehavior` 인터페이스만 알아야 하며, 실제 브레이크 행동(ABS vs 일반 브레이크)은 별도의 클래스로 분리됩니다.

---

## 🧱 4. **Object‑Process Methodology (OPM)**

* 객체(Object, state)를 프로세스(Process, behavior)가 **소비하거나 생성하는 관계**를 시각적으로 표현해 **정적인 구조와 동적인 행동**을 동시에 모델링합니다.

---

## 🛠️ 5. **Shlaer–Mellor Method**
* 시스템을 **분석 도메인과 설계 도메인**으로 나누고, **state machine 기반 상태 모델**과 메시지 기반 행동 모델을 독립적으로 관리합니다.

---

## 🎯 6. **행동 멤버 기반 아키텍처 (Behavior‑centric Architecture)**

* NPS 논문 “Software architecture built from behavior models”에서는 이벤트 트레이스(event trace)와 이벤트 문법(event grammar)을 사용해 **행동 중심으로 아키텍처를 기술하고**, 이후에 구조적으로 refinement 및 구현하는 접근을 제안합니다.

---

### ✅ 요약 비교

| 방식                     | 상태 (State) 분리 | 행동 (Behavior) 분리 | 계산 (Computation) 분리        |
| ---------------------- | ------------- | ---------------- | -------------------------- |
| SoC                    | ✔ (데이터 레이어)   | ✔ (비즈니스 레이어)     | ✔ (계산 로직은 비즈니스 계층에서 분리 가능) |
| CQS / CQRS             | ✔ (쿼리 모델)     | ✔ (명령 모델)       | ✔ (명령 로직에 포함 가능)         |
| Strategy               | –             | ✔ (행위 인터페이스)     | ✔ (전략 객체 내부에 위치)           |
| OPM                    | ✔             | ✔                | ✔                          |
| Shlaer‑Mellor          | ✔             | ✔                | – (보통 state 중심)            |
| Behavior‑centric Arch. | –             | ✔ (이벤트 중심)       | ✔ (이벤트 내 포함)               |

---

## 📚 추가 자료/아티클

* **SoC**: Wikipedia – 분리의 중요성과 구현 방식 정보
* **CQS / CQRS**: Wikipedia – Meyer 설명 및 Fowler 사례
* **Strategy Pattern**: Wikipedia – 런타임 행위 변경 예시 포함
* **OPM**: Wikipedia – 객체와 프로세스의 상호작용 그래픽 모델링
* **Shlaer‑Mellor**: Wikipedia – 상태머신 기반 설계 모델
* **Behavior‑centric 아키텍처**: NPS 연구 – 이벤트를 중심으로 아키텍처 구조화
