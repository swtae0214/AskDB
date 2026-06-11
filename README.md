# 💬 AskDB: LLM 기반 자연어 DB 조회 시스템

> **사용자의 자연어 질문을 SQL 쿼리로 변환하여 데이터베이스를 조회하고, 그 결과를 다시 자연어 답변 및 시각화 데이터로 제공하는 Text-to-SQL 프로젝트입니다.**

---

## 🚀 프로젝트 개요
기존의 데이터 조회 방식은 SQL 문법을 명확히 알고 있어야 한다는 한계가 있습니다. **AskDB**는 LLM(Large Language Model)을 활용하여 "지난달 매출이 가장 높은 상품 5개 보여줘"와 같은 일상적인 질문(자연어)만으로도 복잡한 데이터베이스를 안전하고 정확하게 조회할 수 있도록 돕는 서비스입니다.

### 🌟 주요 기능
- **Text-to-SQL 변환**: 사용자의 자연어 질문을 해석하여 최적의 SQL 쿼리 자동 생성
- **인터랙티브 챗 UI**: Streamlit 기반의 직관적인 대화형 웹 인터페이스 제공
- **데이터 시각화**: 조회된 결과를 단순 텍스트뿐만 아니라 테이블 및 차트 형태로 가공하여 시각적 통찰 제공

---

## 🛠️ Tech Stack (기술 스택)

### Backend & AI Orchestration
- **Python**: 시스템 메인 런타임 환경
- **LangChain**: LLM-DB 연결, 프롬프트 관리 및 SQL Agent 구현
- **FastAPI**: 프론트엔드와 유연한 통신을 위한 경량 API 서버

### LLM (AI Model)
- **OpenAI API (GPT-4o / GPT-4o-mini)**: 고성능 Text-to-SQL 구현 및 검증
- *Alternative*: **Ollama (Llama 3 / Qwen)** 를 통한 로컬 오픈소스 LLM 연동 지원

### Database
- **SQLite**: 로컬 프로토타이핑 및 초기 개발용 파일 기반 DB
- **PostgreSQL / MySQL**: 실환경 테스트 및 대용량 샘플 데이터 운영

### Frontend
- **Streamlit**: 빠르고 직관적인 데이터/AI 웹 대시보드 및 챗봇 UI 구축

---

## 🏗️ System Architecture (시스템 아키텍처)

1. **User Input**: 사용자가 자연어로 질문을 입력합니다. (예: *"올해 가장 많이 팔린 제품이 뭐야?"*)
2. **Context Provision**: 데이터베이스의 테이블 구조(Schema DDL)와 쿼리 정확도를 높이기 위한 Few-shot 예시를 LLM 프롬프트에 결합합니다.
3. **SQL Generation**: LLM이 주어진 스키마를 바탕으로 정확한 SQL 문을 생성합니다.
4. **SQL Execution**: 백엔드에서 생성된 SQL을 받아 데이터베이스(DB)에 쿼리를 실행합니다.
5. **Response & Visualization**: DB 응답 데이터를 가공하여 사용자가 이해하기 쉬운 자연어 답변과 테이블/차트 형태로 UI에 출력합니다.

---

## 🔒 Key Challenges & Solutions (핵심 챌린지)

포트폴리오의 완성도를 높이기 위해 단순 구현을 넘어 아래의 챌린지들을 해결하는 데 집중했습니다.

* **보안 강화 (SQL Injection 방지)**
  - LLM의 오작동이나 악의적인 유도로 인한 데이터 변조/삭제(`DROP`, `DELETE` 등)를 방지하기 위해, DB 연동 시 **Read-Only(읽기 전용) 계정**을 분리하여 바인딩했습니다.
* **LLM 스키마 크기 제한 극복 (Context Window Optimization)**
  - 수많은 테이블 스키마를 프롬프트에 모두 넣으면 토큰 제한과 비용 문제가 발생합니다. 이를 해결하기 위해 질문에 따라 **관련성 높은 테이블만 동적으로 찾아내는 메커니즘(Vector DB 텍스트 유사도 검색 등)**을 도입했습니다.
* **Few-shot Prompting을 통한 정확도 향상**
  - 복잡한 `JOIN` 연산이나 집계 함수에서 발생하는 LLM의 실수를 줄이기 위해, 주요 비즈니스 질문과 SQL 쌍의 예시 가이드를 체계적으로 설계하여 프롬프트에 반영했습니다.

---

## 💻 시작 가이드 (Getting Started)

### Prerequisite
- Python 3.10 이상
- OpenAI API Key (또는 로컬 Ollama 환경)
- Docker (로컬 DB 구성 시 필요)

### Installation
```bash
# 1. 저장소 클론
git clone [https://github.com/your-username/talk-to-db.git](https://github.com/your-username/talk-to-db.git)
cd talk-to-db

# 2. 가상환경 생성 및 활성화
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate

# 3. 필수 패키지 설치
pip install -r requirements.txt

# 4. 환경 변수 세팅 (.env)
echo "OPENAI_API_KEY=your_api_key_here" > .env

# 5. 어플리케이션 실행
streamlit run app.py
