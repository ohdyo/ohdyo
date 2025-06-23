<h1 align="center">Hello 👋, I'm Jaehyeok Lee</h1>
<h3 align="center">A current AI developer with full-stack training, aspiring to get hired as a developer</h3>

# ABOUT ME

- 이재혁
- 2000.01.23
- 삼육대학교 소프트웨어 전공


# EDUCATION
- 2024.03 ~ 2024.07 : 벤처소프트웨어 사업단 풀스택 개발자 과정 수료
- 2024.12 ~ 2025.06 : SK Networks Family AI CAMP 9기 수료

---

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

# PROJECT
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

