---
name: grilling
description: Grill the user relentlessly about a plan, decision, or idea. Use when the user wants to stress-test their thinking, or uses any 'grill' trigger phrases (Korean: '그릴링', '빈틈 점검', '계획 검증').
---

Interview the user relentlessly until you reach a shared understanding. Map this as a **design tree**: every decision branches into the decisions that hang off it.

Work the tree in **rounds**. The **frontier** is every decision whose prerequisites are already settled: the questions you can ask _now_ without guessing at answers you haven't heard yet. Ask the whole frontier in one round: number each question and give your recommended answer. Then wait for the user's answers before the next round.

Format a round like so:

```
❓ **Q1** - **<question title>**: <question body, might be multiple paragraphs, including multiple choices>

➡️ <your recommended answer>

---

❓ **Q2** - **<question title>**: <question body, might be multiple paragraphs, including multiple choices>

➡️ <your recommended answer>
```

Word each question so "yes" accepts your recommended answer.

Each round the user answers reshapes the tree: settled decisions push the frontier outward and unblock questions that depended on them. Recompute the frontier and ask the next round. A question whose answer depends on another question still open in this round belongs to a _later_ round, not this one.

Finding _facts_ is your job, never the user's. When a frontier question needs a fact from the environment (filesystem, tools, etc.), dispatch a sub-agent with a fresh context window to find it; don't ask the user for anything you could look up yourself. Don't block on it: a running exploration is an unsettled prerequisite, so only the questions downstream of it wait for the sub-agent to report; ask the rest of the frontier now. The _decisions_ are the user's: put each to them and wait.

The session is done when the frontier is empty: every branch of the design tree visited, nothing left silently assumed. Do not act on it until the user confirms you have reached a shared understanding.

## 이 저장소에서의 적용 (원본: mattpocock/skills, MIT)

- 사용자가 이 스킬을 직접 요청하지 않았는데 필요하다고 판단되면, 시작하기 전에 이유를 짧게 알리고 승인을 받는다. (AGENTS.md 0.1)
- 질문은 한국어로 하되, 위의 라운드 형식(번호, 질문, 추천 답)을 그대로 따른다.
- 질문을 두 종류로 구분한다.
  - **설계·구현 결정:** 사용자에게 바로 묻는다.
  - **현업 도메인 사실** (업무 방식, 규칙, 데이터 의미, 성공 기준 등 환경에서 찾을 수 없고 현업 실무자만 아는 것): 사용자에게 묻지 말고 **INTERVIEW.md의 미해결 질문에 업무 언어로 적는다.** 에이전트는 현업 실무자에게 직접 메시지를 보낼 수 없다. 사용자가 DM으로 대신 묻고 답변을 붙여 넣어 준다. 그 답변이 올 때까지 해당 질문에 의존하는 결정은 보류한다.
  - 현업 실무자에게 보낼 질문에는 **추천 답을 붙이지 않는다.** 답을 유도하지 않도록 중립적으로 묻고, 기술 용어 없이 번호를 매겨 그대로 복사해 보낼 수 있게 쓴다. (추천 답 형식은 사용자에게 묻는 설계 결정에만 쓴다.)
- 현업 질문은 INTERVIEW.md에 이미 있는 기본 질문과 기존 질문을 먼저 확인하고, 중복되지 않게 쓴다.
- 사용자가 현업 답변을 붙여 넣으면 INTERVIEW.md 인터뷰 로그에 기록하고, 설계 트리와 frontier를 다시 계산해 다음 라운드를 진행한다.
- **시간 제한이 있는 작업이다.** 한 라운드는 중요도 순으로 최대 7개 정도로 묻고, 나머지는 다음 라운드로 넘긴다.
- 사용자가 "충분하다", "여기서 마무리"라고 하면 frontier가 남아 있어도 종료한다. 남은 질문은 INTERVIEW.md 미해결 질문에, 그 대신 둔 가정은 REQUIREMENTS.md나 DECISIONS.md에 "가정"으로 명시한다.
- 확정된 내용은 INTERVIEW.md(합의 사항), REQUIREMENTS.md, PLAN.md, DECISIONS.md에 반영한다. DECISIONS.md에 기록할 때는 AGENTS.md 규칙(대안 3개 정도, 감수한 것, 이유)을 따른다.
- 이 스킬로 질문하는 동안에는 코드를 작성하지 않는다(AGENTS.md 1.1 맥락 수집 단계).
