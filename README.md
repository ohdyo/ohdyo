<h1 align="center">Hello 👋, I'm Jaehyeok Lee</h1>
<h3 align="center">A current AI developer with full-stack training</h3>

# ABOUT ME

- 이재혁
- 2000.01.23
- 삼육대학교 소프트웨어 전공


# EDUCATION
- 2024.03 ~ 2024.07 : 벤처소프트웨어 사업단 풀스택 개발자 과정 수료
- 2024.12 ~ 2025.06 : SK Networks Family AI CAMP 9기 수료

# HISTORY
- 2025.09 ~ 2025.12 : (주)디비엔텍 AI 연구원 (INTERN)
- 2026.01 ~ 2026.08 : (주)컴앤휴면 AI 연구원

---

# PROJECT
<details>
<summary><strong>kor_doc_parser — 한국어 공공 문서 PDF 파서</strong></summary>

**한국어 공문서·보도자료 PDF를 RAG(검색 증강 생성)용 정형 데이터(JSON·JSONL·마크다운)로 바꾸는 문서 파서**

머리말·쪽번호·로고 같은 잡요소를 걸러 내고 표·제목·항목 계층·그림 속 글자까지 구조를 살려 검색에 바로 쓸 수 있게 만든다. 문서 인식 모델은 추가 학습 없이 쓰고 그 둘레를 규칙으로 보강했으며, 그림 해석이 약한 설명 모델은 직접 만든 학습 자료로 LoRA 튜닝했다.

- 개발 기간 : 2026년 추석 이후 ~ 진행 중
- 플랫폼 : Python 라이브러리 · CLI(`kdp`) · 클라우드 GPU 서버
- 개발 인원 1명

- **개발 환경**
  - 언어 : Python(3.12)
  - 모델 : PaddleOCR-VL-1.6(0.96B) · PP-DocLayoutV3 · SK텔레콤 A.X-4.0-VL-Light(7.7B, LoRA 튜닝)
  - 서빙 : vLLM(0.31.0)
  - 라이브러리 : PaddleX(3.7.2) · pypdfium2 · pydantic · typer · transformers(5.16) · peft(0.20)
  - 인프라 : 클라우드 GPU(NVIDIA A40 46GB)
  - 라이선스 원칙 : 무료·오픈소스만(AGPL·사용 규모 조건·자체 약관 모델은 배제)

### 차별점

#### 1. 학습 전에 규칙과 설정으로 — 비교군 셋과 같은 정답·같은 지표로 측정
PDF 글자층 우선 사용, PaddleX 내부 훅 4개(블록 오류 격리·잘림 기록·VLM 생략·그래프 글자 읽기), 한국어 문서 규칙(쪽 판정·표 머리행·제목/항목 계층·공문 메타)만으로 품질을 올렸다.

| 지표(공개 문서 2건 합계) | **kdp(이 프로젝트)** | docling | PaddleOCR-VL 기본 |
| --- | --- | --- | --- |
| 표 머리행 일치 | **16/16** | 9/16 | 2/16 |
| 제목 찾음 | **29/30** | 27/30 | 15/30 |
| 항목 수준 일치 | **41/41** | 34/41 | 0/41 |
| 잡요소 제거(머리 줄·로고·쪽번호) | **50/50** | 24/50 | 47/50 |
| 그림 제목 연결 | **12/12** | 7/12 | 0/12 |
| 그래프·지도 속 글자 읽음 | **81/136** | 0 | 0 |

=> ***처음 잰 뒤 찾은 문제를 규칙으로 고쳐 다시 잰 값이며, 13쪽 기상청 보도자료에서는 비교한 지표 모두가 비교군과 같거나 앞섰다.***

#### 2. 한국어 근거 중심의 모델 선정
- 공개 문서 인식 모델 약 30개를 한국어(35%)·영어 품질(15%)·속도/메모리(20%)·잡요소 라벨(15%)·학습/서빙 경로(15%)와 라이선스 관문으로 비교
- 영·중 벤치마크 순위가 한국어 품질을 예측하지 못한다는 점(예: OmniDocBench 95.2 인 모델이 한국어 61.2)을 확인해 한국어 근거에 가장 큰 가중치를 둠
- 1B 급 전용 파서(PaddleOCR-VL-1.6)를 선택 — 문서 파싱은 파라미터 크기보다 구조와 학습 데이터가 중요

#### 3. 그림 해석 튜닝 — 학습 자료를 직접 만들어 LoRA 학습
그림 설명이 그림 글자를 옮길 뿐 '어디가 가장 높은지·흐름이 어떤지' 같은 해석을 못 해서, 학습 없이 고치는 두 방법(확신도 다시 재기·물음 꼴로 묻기)을 미리 정한 기준으로 시험해 막힌 것을 확인한 뒤 지도 학습으로 넘어갔다.

| 항목 | 내용 |
| --- | --- |
| 학습 자료 | 합성 그래프·지도·흐름도·조직도 3,000장(정답은 코드가 계산) + 공개 정부 문서 33개의 실제 그림을 라벨러 둘이 따로 매겨 합의한 정답 745개(정답 다툼 0) |
| 학습 | LoRA(rank 16) 1 에폭, A40 한 장에서 2시간 55분 |
| 결과 | 실제 문서 그래프 해석 정답률 **57% → 81%**, 새 시험지(140물음) 62% → 81%, 지도 방위는 19% 그대로 — 미리 정한 통과 기준은 넘지 못해 다음 단계를 정함 |

#### 4. 측정과 검증
- 자동 시험 623개, 독립 코드 검토에서 확인된 지적 207건 반영
- 개발 자료로 만든 규칙은 처음 보는 문서로 다시 재고, 판정 문턱은 개발 자료에서만 골라 확인 자료에 그대로 적용
- 그림 설명은 문장마다 그림과 대조 판정 — 틀린 문장 27% → 10~16%

### 레포지토리 링크
<a href="https://github.com/ohdyo/kor_doc_parser">kor_doc_parser</a>

</details>

<details>
<summary><strong>WLB_MATE</strong></summary>
<img src="https://github.com/user-attachments/assets/79db4aad-9fa6-44b0-bbed-713088275a65" width="300"/>

- 개발 기간 : 2025.04.~2025.06.20 (2개월)
- 플랫폼 : Web
- 개발 인원 5명

- **개발 환경**
  - 언어 : Python(3.12), JavaScript, Css
  - 서버 : Nginx, Uvicorn, 
  - 프레임워크 : React.js(node:18), FastAPI(0.115.12), LangChain(0.3.63), Qdrant(1.14.2)
  - DB : Mysql 8.4.5
  - IDE :
  - API : RESTful API (JSON)
 
### 차별점

#### 1. Qdrant-Client
   
| 항목                 | **Qdrant**                            | **FAISS**                             |
| ------------------ | ------------------------------------- | ------------------------------------- |
| **기본 구조**          | 네트워크 기반 **서버형 벡터 DB** (gRPC/REST API) | **로컬 인메모리 라이브러리** (Python/C++)        |
| **실시간 쓰기/검색**      |  지원 (write & search 동시에 가능)          |  인덱스 재구성 필요 (write 후 search까지 지연 발생) |
| **동시성 처리**         |  다중 클라이언트 요청 처리 (웹 API)              |  싱글 프로세스 or 직접 멀티프로세싱 구현 필요          |
| **필터 기반 검색**       |  메타데이터 기반 실시간 필터링 지원                 |  메타데이터 필터 미지원 (별도 처리 필요)             |
| **CRUD 처리**        |  실시간 삽입, 수정, 삭제 가능                   |  삭제/수정은 전체 인덱스 재생성 필요                |
| **LangChain 연동**   |  `QdrantRetriever` 등 네이티브 지원         |  제한적 지원 (사용자 정의 Retriever 필요)        |
| **스케일링**           |  Docker/클러스터로 수평 확장 가능               |  단일 머신 중심                            |
| **실시간 삽입 후 즉시 검색** |  `upsert → immediate search` 가능      |  `add → reindex` 필요, 실시간성 낮음         |

=> ***실시간 적용을 통해 업로드된 문서를 따로 저장해줄 필요 없이 벡터 추가가 가능하다.***

#### 2. MemorySaver
**모듈 간 독립성과 사용자 스레드 분리를 위한 세션 관리 최적화**
- MemorySaver는 세션마다 독립된 메모리 저장 구조를 갖추고 있어, 사용자의 기능별 대화 흐름을 분리하고 유지하기 용이함.
- 반면 BufferMemory는 모든 대화를 이어 붙이는 방식이라, 모듈 간 문맥 충돌 가능성이 존재.
- LangChain 내 Tool/Chain과의 유연한 연동 -> **Qdrant로 작성한 RAG와 사용하기 적절**

#### 3. STT (WhisperX)
**WhisperX 선택 이유 **

| 항목    | WhisperX          | 응답 고도화 목적       |
| ----- | ---------------------- | --------------- |
| 속도    | Whisper 대비 4\~5배 빠름    | 대용량 오디오 실시간 처리  |
| 정확도   | 단어 단위 정렬, float32 설정   | 정밀 회의록/상담 추출    |
| 화자 구분 | Speaker diarization 지원 | 상담 분석, 회의 화자 파악 |
| 효율성   | VAD로 무음 제거 + GPU 병렬화   | 리소스 최적화         |
| 안정성   | 전사 실패 허용 안 함           | 누락 없는 완전 전사     |

### 레포지토리 링크
<a href="https://github.com/ohdyo/WLB_MATE">WLB_MATE</a>

</details>




## 🚀 Languages & Tools

### 🖥️ Programming Languages
<p align="left">
  <a href="#"><img src="https://raw.githubusercontent.com/devicons/devicon/master/icons/python/python-original.svg" alt="Python" width="40" height="40"/></a>
  <a href="#"><img src="https://raw.githubusercontent.com/devicons/devicon/master/icons/java/java-original.svg" alt="Java" width="40" height="40"/></a>
  <a href="#"><img src="https://raw.githubusercontent.com/devicons/devicon/master/icons/javascript/javascript-original.svg" alt="JavaScript" width="40" height="40"/></a>
</p>

### 🌐 Web / Backend Frameworks
<p align="left">
  <a href="#"><img src="https://cdn.worldvectorlogo.com/logos/django.svg" alt="Django" width="40" height="40"/></a>
  <a href="#"><img src="https://www.vectorlogo.zone/logos/springio/springio-icon.svg" alt="Spring" width="40" height="40"/></a>
  <a href="#"><img src="https://raw.githubusercontent.com/devicons/devicon/master/icons/nodejs/nodejs-original-wordmark.svg" alt="Node.js" width="40" height="40"/></a>
</p>

### 💻 Frontend
<p align="left">
  <a href="#"><img src="https://raw.githubusercontent.com/devicons/devicon/master/icons/react/react-original-wordmark.svg" alt="React" width="40" height="40"/></a>
</p>

### ☁️ DevOps & Cloud
<p align="left">
  <a href="#"><img src="https://raw.githubusercontent.com/devicons/devicon/master/icons/docker/docker-original-wordmark.svg" alt="Docker" width="40" height="40"/></a>
  <a href="#"><img src="https://raw.githubusercontent.com/devicons/devicon/master/icons/amazonwebservices/amazonwebservices-original-wordmark.svg" alt="AWS" width="40" height="40"/></a>
  <a href="#"><img src="https://raw.githubusercontent.com/devicons/devicon/master/icons/nginx/nginx-original.svg" alt="Nginx" width="40" height="40"/></a>
</p>

### 🧠 AI / Machine Learning
<p align="left">
  <a href="#"><img src="https://www.vectorlogo.zone/logos/pytorch/pytorch-icon.svg" alt="PyTorch" width="40" height="40"/></a>
  <a href="#"><img src="https://www.vectorlogo.zone/logos/tensorflow/tensorflow-icon.svg" alt="TensorFlow" width="40" height="40"/></a>
  <a href="#"><img src="https://upload.wikimedia.org/wikipedia/commons/0/05/Scikit_learn_logo_small.svg" alt="Scikit-learn" width="40" height="40"/></a>
</p>

### 🛠️ Tools & Others
<p align="left">
  <a href="#"><img src="https://www.vectorlogo.zone/logos/git-scm/git-scm-icon.svg" alt="Git" width="40" height="40"/></a>
  <a href="#"><img src="https://raw.githubusercontent.com/devicons/devicon/master/icons/linux/linux-original.svg" alt="Linux" width="40" height="40"/></a>
</p>

---

