# 📚 법령 통합 질의응답 챗봇

**여러 법령 문서를 동시에 분석하고, AI 기반의 다중 에이전트 시스템을 통해 정확하고 종합적인 답변을 제공하는 법률 챗봇 애플리케이션입니다.**

## ✨ 주요 특징

#### - 🤖 다중 에이전트 협업: 각 법령별 전문 에이전트(Gemini-2.0/2.5-flash)가 협업하여 종합적 답변 생성
#### - 🔍 빠르고 정확한 근거 조항 검색: TF-IDF와 AI를 결합한 하이브리드 검색으로 노트북에서도 빠르고 정확한 법령 검색
#### - 📊 유연한 데이터 처리: PDF, JSON, API 등 다양한 형태의 법령 데이터 통합 지원

## ✨ 주요 기능 (Key Features)

### 1\. 다중 에이전트 아키텍처 (Multi-Agent System)

복잡한 법률 질의는 하나의 법령이 아닌 여러 법령을 종합적으로 검토해야 할 때가 많습니다. 이 챗봇은 다음과 같은 다중 에이전트 시스템을 도입하여 답변의 깊이와 정확성을 높였습니다.

  - **법령별 전문 에이전트 (Law Agents)**: 사용자가 업로드하거나 다운로드한 각각의 법령은 개별적인 전문 에이전트를 가집니다. 각 에이전트는 담당 법령 내에서 사용자의 질문과 가장 관련성이 높은 조항을 찾아내고, 이를 바탕으로 1차적인 답변을 생성합니다.
  - **헤드 에이전트 (Head Agent)**: 각 전문 에이전트가 생성한 답변들을 모두 취합하여, 이를 종합하고 상호 비교/분석합니다. 최종적으로 사용자에게 일관되고 논리적인 최종 답변을 두괄식으로 명확하게 제공하는 역할을 합니다.

이 구조를 통해 특정 법령에 국한되지 않는 포괄적인 법률 검토가 가능합니다.

### 2\. AI와 TF-IDF를 결합한 하이브리드 검색 (AI + TF-IDF Hybrid Search)

정확한 근거 조항을 찾는 것은 법률 질의응답의 핵심입니다. 저희는 전통적인 검색 방식의 한계를 AI로 보완했습니다.

  - **TF-IDF의 속도와 효율성**: 1차적으로 TF-IDF 벡터화를 통해 방대한 법령 텍스트에서 질문과 관련된 후보 조항들을 빠르게 찾아냅니다.
  - **AI의 맥락 이해 능력**: 하지만 TF-IDF는 단어의 빈도에만 의존하여 문맥을 파악하지 못하는 단점이 있습니다. 저희는 Google의 Gemini AI를 활용하여 이 문제를 해결합니다.
    1.  **유사 질문 생성**: 원본 질문의 의미는 유지하되, 표현을 달리하는 여러 개의 유사 질문을 생성합니다. 이를 통해 다양한 방식으로 법령 조항에 접근하여 검색 누락 가능성을 최소화합니다.
    2.  **키워드 및 유사어 확장**: 사용자의 질문 및 유사 질문을 분석하여 핵심 법률 용어를 추출하고, 동의어, 유의어, 관련어를 생성하여 검색 범위를 확장합니다. (`예: '급여' -> '임금', '보수', '근로소득'`)

이 하이브리드 방식은 **검색의 속도**와 **정확도**를 모두 높여, 사용자의 어떤 질문에도 최적의 근거 규정을 찾아냅니다.

### 3\. 유연한 데이터 수용 방식 (Flexible Data Ingestion)

사용자는 다양한 방법으로 챗봇에게 법령 데이터를 제공할 수 있습니다.

  - **📄 파일 업로드**:
      - **PDF 파일**: 가지고 있는 법령 PDF 파일을 직접 업로드하면, 시스템이 자동으로 텍스트를 추출하고 구조화된 JSON 형식으로 변환합니다.
      - **JSON 파일**: 정해진 형식(`[{ "조번호": "제1조", "제목": "목적", "내용": "이 법은..." }]`)의 JSON 파일이 있다면 바로 업로드하여 사용할 수 있습니다.
  - **⚖️ API 연동**:
      - **국가법령정보센터 API**: 분석하고 싶은 법령이나 행정규칙의 이름만 입력하면, API를 통해 최신 데이터를 실시간으로 다운로드하여 챗봇의 지식 기반으로 즉시 추가할 수 있습니다.

이를 통해 사용자는 특정 파일 형식이나 데이터 소스에 구애받지 않고 원하는 모든 법령을 분석 대상으로 삼을 수 있습니다.

## 🛠️ 기술 스택 및 아키텍처 (Tech Stack & Architecture)

  - **언어**: Python
  - **프레임워크**: Streamlit
  - **AI 모델**: Google Gemini 2.0 Flash / 2.5 Flash
  - **검색/임베딩**: Scikit-learn (TF-IDF, Cosine Similarity)

### 아키텍처 흐름

1.  **데이터 수집**: 사용자가 Sidebar UI를 통해 PDF/JSON 업로드 또는 API로 법령 데이터를 수집합니다.
2.  **데이터 전처리**: 수집된 데이터는 `{"조번호", "제목", "내용"}` 형식의 통일된 JSON 구조로 변환됩니다.
3.  **임베딩 및 캐싱**: 각 법령 데이터는 조문 단위로 나뉘어 TF-IDF 벡터로 변환(임베딩)됩니다. 파일 내용의 해시값을 키로 사용하여 처리 결과를 캐싱함으로써, 동일한 파일에 대한 반복적인 임베딩 작업을 방지합니다.
4.  **질의응답**:
    a. 사용자가 질문을 입력합니다.
    b. **Query Expansion**: Gemini AI가 질문을 분석하여 검색 키워드 및 유사 질문 목록을 생성합니다.
    c. **검색 (Retrieval)**: 확장된 쿼리들을 사용하여 각 법령 데이터에서 가장 관련성 높은 조문(Chunk)들을 TF-IDF 기반 코사인 유사도로 검색합니다.
    d. **답변 생성 (Generation)**:
      -   **Law Agents**: 각 법령별로 검색된 조문과 원본 질문을 Gemini에 전달하여 법령 특화 답변을 생성합니다. (비동기 처리로 동시 실행)
      -   **Head Agent**: 모든 Law Agent의 답변을 종합하여 최종 답변을 생성합니다.
    e. **결과 출력**: 최종 답변이 사용자 화면에 표시됩니다.

## 🚀 시작하기 (Getting Started)

### 사전 요구사항

  - Python 3.8 이상
  - Google API Key
  - 국가법령정보센터 API Key (법률, 행정규칙)

### 설치

1.  **저장소 복제:**

    ```bash
    git clone https://github.com/your-username/your-repo-name.git
    cd your-repo-name
    ```

2.  **가상 환경 생성 및 활성화 (권장):**

    ```bash
    python -m venv venv
    source venv/bin/activate  # Windows: venv\Scripts\activate
    ```

3.  **필요한 라이브러리 설치:**

    ```bash
    pip install -r requirements.txt
    ```

4.  **환경 변수 설정:**
    프로젝트 루트 디렉터리에 `.env` 파일을 생성하고 아래와 같이 API 키를 입력하세요.

    ```
    GOOGLE_API_KEY="your_google_api_key"
    LAW_API_KEY="your_law_center_api_key"
    ADMIN_API_KEY="your_law_center_api_key"
    ```

## 💻 사용 방법 (How to Use)

1.  **애플리케이션 실행:**

    ```bash
    streamlit run main.py
    ```

2.  **법령 데이터 준비 (사이드바):**

      - '파일 업로드' 탭에서 PDF 또는 JSON 파일을 업로드합니다.
      - '법률 API' 또는 '행정규칙 API' 탭에서 원하는 법령명을 검색하여 다운로드합니다.

3.  **데이터 처리:**

      - 수집된 법률 데이터 목록을 확인하고, `챗봇용 데이터 변환 (벡터 임베딩 생성)` 버튼을 클릭하여 챗봇이 사용할 수 있도록 데이터를 처리합니다.

4.  **질의응답 시작:**

      - 데이터 처리가 완료되면 메인 화면의 채팅 입력창에 질문을 입력하여 법률 상담을 시작할 수 있습니다.

## 📂 파일 구조 (File Structure)

```
.
├── 📄 main.py              # Streamlit UI 및 메인 애플리케이션
├── 🛠️ utils.py             # 핵심 로직 (임베딩, 검색, 에이전트)
├── 📋 pdf_json.py          # PDF → JSON 변환 유틸리티
├── ⚖️ lawapi.py            # 국가법령정보센터 법률 API
├── 📊 adminapi.py          # 국가법령정보센터 행정규칙 API
├── 📦 requirements.txt     # 프로젝트 의존성
├── 🔐 .env                 # 환경 변수 (API 키)
└── 📚 README.md           # 프로젝트 문서
```

## 라이센스

이 프로젝트는 MIT 라이센스 하에 배포됩니다. 자세한 내용은 [LICENSE](LICENSE) 파일을 참조하세요.

```
MIT License

Copyright (c) 2025 YSCHOI-github

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```


