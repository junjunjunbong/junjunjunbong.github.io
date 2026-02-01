---
layout: post
title: "SolarNote: AI로 만드는 스마트 수학 오답노트"
date: 2026-02-01 12:00:00+0900
description: "태양처럼 밝게, 실수도 빛나는 배움으로 - 틀린 수학 문제 사진을 업로드하면 유사문제 5개와 상세 풀이를 자동 생성하는 AI 멀티 에이전트 시스템"
tags: AI LLM multi-agent Upstage Solar math education
categories: projects
giscus_comments: false
featured: true
toc:
  beginning: true
---

## 프로젝트 소개

**SolarNote**는 학생들이 틀린 수학 문제 사진을 업로드하기만 하면, **유사문제 5개**와 **상세 풀이**를 자동으로 생성해주는 AI 멀티 에이전트 시스템입니다.

---

## 문제 정의

### 수학 오답노트, 왜 어려울까?

학생들이 수학을 어려워하는 가장 큰 이유 중 하나는 **오답 관리**에 있습니다:

| 기존 방식의 문제점 | 영향 |
|------------------|------|
| 문제를 직접 타이핑해야 함 | 시간 소모, 오타 발생 |
| 틀린 문제의 핵심 개념 파악 어려움 | 같은 유형 반복 실수 |
| 유사문제를 찾기 위해 여러 교재 뒤짐 | 학습 효율 저하 |
| 풀이 과정 정리가 체계적이지 않음 | 복습할 때 쓰기 어려움 |

> **결론**: 오답노트 작성 자체가 큰 부담이 되어, 학생들이 포기하는 경우가 많습니다.

---

{% include figure.liquid loading="eager" path="assets/img/solarnote.png" title="SolarNote AI Math Error Note Generator" class="img-fluid rounded z-depth-1" %}

---

## 해결 방안

### SolarNote의 6단계 AI 파이프라인

```
📤 문제 이미지 업로드
         ↓
┌─────────────────────────────────────────────────────────────┐
│              ☀️ OrchestratorAgent (총괄 조율)                 │
├─────────────────────────────────────────────────────────────┤
│  ① 📄 ParserAgent      → Document Parse API (OCR)           │
│  ② 🔍 ExtractorAgent   → Information Extract API            │
│  ③ 📚 ConceptAgent     → Solar LLM (개념/난이도 분류)        │
│  ④ ✏️ ProblemAgent     → Solar LLM (유사문제 5개 생성)       │
│  ⑤ 📝 SolutionAgent    → Solar LLM (단계별 풀이 작성)        │
│  ⑥ 📋 NoteAgent        → Solar LLM (오답노트 정리)           │
└─────────────────────────────────────────────────────────────┘
         ↓
📋 완성된 오답노트 (유사문제 + 풀이 + 학습조언)
```

### 5가지 유사문제 변형 전략

| 유형 | 설명 | 예시 |
|------|------|------|
| **숫자 변형** | 조건만 바꾼 기본 유형 | $$x=2 \rightarrow x=3$$ |
| **개념 확장** | 관련 개념 추가 적용 | 단순 미분 → 극값 문제 |
| **역문제** | 주어진 것과 구하는 것 바꿈 | 함수 찾기 → 역함수 찾기 |
| **응용** | 실생활 상황 적용 | 순수 방정식 → 거리/속력 문제 |
| **심화** | 난이도 상승 | 단일 개념 → 복합 개념 |

---

## 기대효과

### 학습자 관점

- **시간 절약**: 문제 타이핑 없이 사진 한 장으로 완료
- **맞춤 학습**: 내가 약한 부분의 유사문제 집중 훈련
- **체계적 복습**: 개념 → 문제 → 풀이 → 조언의 완전한 사이클

### 교육자 관점

- **과외 선생님**: 학생별 약점 분석 및 맞춤 문제 출제
- **학습 데이터**: 어떤 유형의 문제를 자주 틀리는지 패턴 파악

### 효과 수치 예상

| 지표 | 개선 효과 |
|------|----------|
| 오답노트 작성 시간 | 90% 단축 (30분 → 3분) |
| 유사문제 확보 | 무제한 생성 가능 |
| 개념 이해도 | 단계별 풀이로 40% 향상 |

---

## 핵심 기술

### 1. Upstage Document Parse API

**수학 OCR 특화**: 일반 텍스트뿐만 아니라 **수식(LaTeX)**까지 정확히 인식

```
입력: 수학 문제 사진 (PNG/JPG/PDF)
출력: "x^2 + 2x + 1 = 0 의 근을 구하시오"
```

### 2. Upstage Information Extract API

**구조화된 정보 추출**: 문제 텍스트에서 조건, 보기, 질문 등을 자동 분리

```python
@dataclass
class ExtractedFields:
    problem_text: str    # 전체 문제
    conditions: list     # 주어진 조건들
    question: str        # 구해야 할 것
    choices: list        # 보기 (있는 경우)
```

### 3. Upstage Solar Pro LLM

**한국어 수학 특화 AI**: 수학적 추론과 한국어 이해를 동시에 수행

| Agent | 역할 | LLM 활용 |
|-------|------|----------|
| ConceptAgent | 단원/개념/난이도 분류 | Few-shot 프롬프트 |
| ProblemAgent | 5가지 유형의 유사문제 생성 | Chain-of-Thought |
| SolutionAgent | 단계별 상세 풀이 작성 | Structured Output |
| NoteAgent | 학습 조언 및 공식 정리 | Context-aware |

### 4. Multi-Agent Architecture

**단일 책임 원칙(SRP)**: 각 에이전트가 하나의 역할에만 집중

```python
class OrchestratorAgent:
    """총괄 조율자 - 각 에이전트 순차 실행"""

    def execute(self, file_content, filename):
        parsed = self.parser.execute(file_content, filename)      # Step 1
        extracted = self.extractor.execute(parsed)                 # Step 2
        concept = self.concept.execute(extracted)                  # Step 3
        problems = self.problem.execute(extracted, concept)        # Step 4
        solutions = self.solution.execute(extracted, problems)     # Step 5
        note = self.note.execute(original, concept, problems, ...) # Step 6
        return note
```

### 5. Streamlit + Solar 테마 UI

**쓰기 쉬운 UI**: 누구나 쉽게 사용 가능한 웹 인터페이스

- Solar 브랜딩 컬러 (#FF6B35 오렌지, #FFB347 골드)
- 실시간 진행 상태 표시 (6단계 프로그레스바)
- 반응형 탭 기반 오답노트 UI

---

## 시작하기 (입문자 가이드)

### Step 1: 프로젝트 클론

```bash
git clone https://github.com/yourusername/solarnote.git
cd solarnote
```

### Step 2: 의존성 설치

```bash
pip install -r requirements.txt
```

**requirements.txt 주요 패키지:**
- `streamlit` - 웹 UI 프레임워크
- `requests` - API 호출
- `python-dotenv` - 환경변수 관리

### Step 3: Upstage API 키 발급

1. [Upstage Console](https://console.upstage.ai/) 접속
2. 회원가입 및 로그인
3. **API Keys** 메뉴에서 새 키 생성
4. `.env` 파일 생성:

```bash
echo "UPSTAGE_API_KEY=your_api_key_here" > .env
```

### Step 4: 앱 실행

```bash
streamlit run app.py
```

브라우저에서 `http://localhost:8501` 접속!

---

## 프로젝트 구조

```
SolarNote/
├── app.py                    # Streamlit 메인 앱 (입력)
├── config.py                 # 환경 설정 (API 설정)
│
├── agents/                   # AI 에이전트들 (핵심 로직)
│   ├── orchestrator_agent.py # 총괄 조율
│   ├── parser_agent.py       # 이미지 → 텍스트
│   ├── extractor_agent.py    # 텍스트 → 구조화
│   ├── concept_agent.py      # 개념 분류
│   ├── problem_agent.py      # 유사문제 생성
│   ├── solution_agent.py     # 풀이 작성
│   └── note_agent.py         # 오답노트 정리
│
├── core/
│   └── upstage_client.py     # Upstage API 래퍼
│
├── models/
│   └── schemas.py            # 데이터 모델 정의
│
├── prompts/
│   └── templates.py          # LLM 프롬프트
│
└── ui/
    ├── components.py         # UI 컴포넌트
    └── styles.py             # CSS 스타일
```

---

## AI Agent 개발 입문자를 위한 팁

### 1. Agent란 무엇인가요?

> **Agent** = "특정 작업을 수행하는 AI 작업자"

일반 프로그램과 차이점:
- 일반 함수: `입력 → [고정 로직] → 출력`
- AI Agent: `입력 → [LLM 판단] → 출력` (유연한 처리)

### 2. Multi-Agent의 장점

| 단일 LLM | Multi-Agent |
|---------|-------------|
| 모든 작업을 한 번에 처리 | 각 작업을 전문가가 처리 |
| 프롬프트 복잡함 | 프롬프트 단순화 |
| 오류 발생 시 전체 재시도 | 해당 Agent만 재시도 |
| 확장 어려움 | 새 Agent 추가로 기능 확장 |

### 3. SolarNote에서 배울 수 있는 패턴

```python
# 1. 재시도 로직 (Graceful Degradation)
def _with_retry(self, func, max_retries=2):
    for attempt in range(max_retries + 1):
        try:
            return func()
        except Exception as e:
            if attempt < max_retries:
                continue
            raise e

# 2. 진행 상태 콜백 (UX 개선)
def set_progress_callback(self, callback):
    self.progress_callback = callback  # UI 업데이트용

# 3. dataclass 기반 타입 안전성
@dataclass
class ConceptInfo:
    topic: str       # 단원명
    concept: str     # 핵심 개념
    difficulty: int  # 난이도 (1-5)
```

---

## 참고 자료

- [Upstage 공식 문서](https://developers.upstage.ai/)
- [Solar LLM 소개](https://www.upstage.ai/solar-llm)
- [Streamlit 문서](https://docs.streamlit.io/)

---

## 라이선스

MIT License - 자유롭게 사용하고 개선해주세요!

---

> **"실수도 배움이 됩니다. SolarNote로 똑똑하게 복습하세요!"**
