# Claude용 Todo 프롬프트 엔지니어링 기법 (최신)

Claude에서 Todo 리스트나 작업 관리를 효과적으로 수행하기 위한 최신 프롬프트 엔지니어링 기법을 아래와 같이 정리합니다.

-----

## 1\. 목적 중심 분명한 지시 (Be explicit & specific)

Claude는 **명확하고 구체적인 지시**에 민감하게 반응합니다.

**예시:**

"오늘의 할 일 목록(task list)을 아래 형식(JSON)으로 출력하세요. 작업명, 우선순위(high/medium/low), 예상 소요 시간, 마감일 포함:"

-----

## 2\. 예시 포함 (Few-shot prompting)

작업 샘플과 그 결과 형식을 프롬프트에 함께 포함하면 Claude가 보다 일관성 있는 출력을 제공합니다.

-----

## 3\. Chain of Thought (CoT) + 작업 분해

"Think step by step" 혹은 "작업을 단계적으로 나눠줘" 지시를 활용해 복잡한 업무를 논리적으로 분해시킬 수 있습니다.

-----

## 4\. Prompt Chaining (단계적 프롬프트 연결)

복잡한 todo 생성 시 아래와 같은 단계를 분리해 구성할 수 있습니다:

  * 전체 작업 개요 요청 →
  * 세부 작업 항목 요청 →
  * 우선순위 및 시간 예측 요청

-----

## 5\. Prefill + XML 혹은 JSON 태그 활용

Claude는 **구조화된 포맷(XML, JSON 등)에 강합니다.** 원하는 포맷의 일부를 미리 제공하면 안정적인 결과를 유도할 수 있습니다.

**예시:**

```xml
<todoList>
  <item>
    <task>...</task>
    <priority>...</priority>
  </item>
</todoList>
```

-----

## 6\. 역할(Role) 지정

Claude에게 역할을 부여하면 응답의 일관성과 정확도가 올라갑니다.

**예시:**

"너는 나의 생산성 도우미야. 오늘의 할 일을 정리해줘."

-----

## 7\. 외부 문맥 or RAG 기반 구성

노션, 캘린더, 회의록 등 외부 문서를 Claude에게 주고 그 맥락을 기반으로 todo를 생성하는 방식입니다. **Retrieval-Augmented Generation(RAG) 구조**에 기반한 기법입니다.

이때, 필요한 파일이나 문서들은 다음과 같은 참조 패턴을 활용하여 명확히 지정할 수 있습니다:

### 🔄 컨텍스트 및 참조 최적화

#### 1\. 파일 참조 패턴

**@ 심볼 활용**

  * @src/models/user.py: 사용자 모델 패턴
  * @.cursor/docs/api-spec.md: API 명세서
  * @tests/factories/user\_factory.py: 테스트 데이터 팩토리

**크로스 레퍼런스**

  * 관련 룰: [database-rules.mdc](https://www.google.com/search?q=mdc:.cursor/rules/102-database.mdc)
  * 참고: @.cursor/rules/100-backend.mdc

**문서 참조**

  * 자세한 내용: @.cursor/docs/architecture.md
  * 예시 코드: @.cursor/docs/examples/

-----

## ✅ Todo 프롬프트 종합 예시

너는 나의 생산성 도우미야.

\<context\>
내 오늘 주요 목표는:

  - 이메일 10개 답변
  - 논문 개요 리뷰 (참조: @.cursor/docs/research/paper\_outline.md)
  - 팀 미팅 준비 (회의록: @docs/meetings/20250715\_team\_sync.md)
    \</context\>

\<instructions\>

1.  핵심 작업(task)을 세 개로 분류해서 JSON 배열로 만들어줘.
2.  각 작업에 priority(high/med/low), 예상 소요 시간(분 단위), 마감시간 포함.
3.  먼저 step-by-step으로 작업 밑단계(subtasks)를 나열해줘.
4.  최종 JSON 구조는 아래와 같아야 해:
    \<todoList\>
    \<item\>
    \<task\>...\</task\>
    \<subtasks\>[... ]\</subtasks\>
    \<priority\>...\</priority\>
    \<time\>...\</time\>
    \<due\>HH:MM\</due\>
    \</item\>
    \</todoList\>
    \</instructions\>

-----

## 📌 요약

| 기법             | 설명                                    |
| :--------------- | :-------------------------------------- |
| 명확한 지시      | Form, format, 역할까지 포함한 지시      |
| 예시 제공        | 입력/출력 페어를 통한 few-shot          |
| CoT + 분해       | 단계적 사고 + 작업 분해                 |
| Prompt Chain     | 프롬프트 순차 연결                      |
| Prefill/XML      | 구조를 미리 정의                        |
| 역할 지정        | 톤/관점 설정                            |
| RAG 기반         | 외부 문서 활용                          |

-----

## 🔗 참고 링크

  * [7 Claude 4 Prompts That Will Supercharge Your Ideas And Your Output](https://www.tomsguide.com/ai/7-claude-4-prompts-that-will-supercharge-your-ideas-and-your-output)
  * [Prompt engineering techniques and best practices: Learn by doing with Anthropic’s Claude 3 on Amazon Bedrock](https://aws.amazon.com/blogs/machine-learning/prompt-engineering-techniques-and-best-practices-learn-by-doing-with-anthropics-claude-3-on-amazon-bedrock/)
  * [Prompt engineering for Claude](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/claude-4-best-practices)
  * [Prompt Engineering with Anthropic’s Claude](https://medium.com/promptlayer/prompt-engineering-with-anthropic-claude-5399da57461d)
  * [Mastering Prompt Engineering for Claude](https://www.walturn.com/insights/mastering-prompt-engineering-for-claude)