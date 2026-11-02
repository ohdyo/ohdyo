<div align="center">

<img src="https://capsule-render.vercel.app/api?section=header&type=wave&color=0:24324a,100:2f5fb3&height=220&text=Jaehyeok%20Lee&fontSize=54&fontColor=ffffff&fontAlignY=32&animation=fadeIn" width="100%" alt="Jaehyeok Lee" />

### 안녕하세요, 문서 데이터를 다루는 AI 서비스 개발자 이재혁입니다

`Document AI` · `RAG` · `LangGraph Agent` · `vLLM Serving` · `LoRA Fine-tuning`

<a href="mailto:leejh100984@gmail.com"><img src="https://img.shields.io/badge/Gmail-EA4335?style=for-the-badge&logo=gmail&logoColor=white" alt="Gmail" /></a>
<a href="https://github.com/ohdyo/kor_doc_parser"><img src="https://img.shields.io/badge/Featured-kor__doc__parser-0B7A75?style=for-the-badge&logo=github&logoColor=white" alt="대표 프로젝트 kor_doc_parser" /></a>

</div>

## 👋 About Me

**문서를 정확히 읽는 데서 시작해 검색·생성·서빙까지, AI 기능을 실제로 돌아가는 환경에 올리고 결과를 숫자로 확인하는 개발자입니다.**

두 회사에서 1년 동안 AI 솔루션을 고객사 폐쇄망과 사내 시스템에 올렸습니다. 사내 RAG 챗봇을 만들며 체감한 "RAG 답변 품질은 원본 문서를 얼마나 정확히 읽어 내느냐에 달려 있다"는 문제의식을 1인 프로젝트로 이어, 한국어 공공 문서 PDF를 RAG용 데이터로 바꾸는 **한국어 문서 파서**를 만들고 있습니다.

- **실제 환경에 올려서 확인합니다** — 사내 챗봇의 LangGraph 에이전트를 2026.06부터 기본 채팅 엔진으로 올렸고, 영상 비식별화 솔루션을 인터넷이 막힌 고객사 폐쇄망에 도커로 구축했습니다.
- **원인을 찾고 숫자로 확인합니다** — GPU 하나를 문서 인식과 LLM이 나눠 쓰며 초당 3토큰까지 떨어지던 LLM 생성을, 직접 재서 원인을 찾고 CUDA MPS로 문서 인식 몫을 30%로 묶어 367~727토큰으로 되살렸습니다.
- **결정과 결과를 기록합니다** — 모델 선정부터 튜닝까지 결정마다 근거와 버린 대안을 남기고, 미리 정한 통과 기준에 못 미친 결과도 기준을 바꾸지 않고 그대로 기록합니다.
- **데이터와 권한의 경계를 먼저 정합니다** — GPU 서버는 문서를 받아 계산만 하고 원본 저장소·DB에는 접근하지 않도록 역할을 나눴습니다.

## 💼 Experience & Education

| 기간 | 내용 |
| :-- | :-- |
| 2026.01 ~ 2026.08 | **컴앤휴먼** · 연구원 — 사내 RAG 챗봇 LLM 서버(FastAPI) · RAG 문서 적재 파이프라인 |
| 2025.09 ~ 2025.12 | **디비엔텍** · 연구원 — 영상 AI 비식별화 솔루션 고도화 |
| 2024.12 ~ 2025.06 | **SK네트웍스 Family AI 캠프 9기** 수료 — ML · DL · LLM · Fine-Tuning · RAG · Agent |
| 2024.03 ~ 2024.06 | **백엔드 SW 개발자 과정** 수료(삼육대학교) — Java · Spring Boot · JPA · MySQL · React |
| 2018.03 ~ 2025.07 | **삼육대학교** 컴퓨터공학부(소프트웨어전공) 졸업 |

## 🚀 Projects

### 회사 프로젝트

> 회사 코드는 비공개라, 맡은 업무와 문제 해결 과정을 정리했습니다.

#### 사내 RAG 멀티에이전트 챗봇 <sub>컴앤휴먼 · 2026.01 ~ 2026.08</sub>

사내 문서 검색·요약·문서 생성을 자연어 한 번으로 처리하고, 주요 분기마다 사용자 승인을 받는 챗봇입니다. 그중 LLM 서버(FastAPI)를 맡았습니다.
- LangChain 체인을 LangGraph 에이전트로 바꿔 2026.06부터 기본 채팅 엔진으로 적용
- 권한별 검색 실패의 원인(순차 스캔)을 찾아 해결

<details>
<summary>맡은 업무 · 문제 해결 자세히</summary>
<br>

**맡은 업무** (Spring 중계 서버·웹 화면은 다른 팀원 담당)
- **LangGraph 에이전트** — 한 방향으로만 흐르던 LangChain 체인을 LangGraph StateGraph로 바꿨습니다. 의도 분류(자료 검색·요약·PPT 생성·입찰 조회) → 갈래별 처리 → 주요 분기마다 사용자 승인 게이트(PPT 생성·웹 검색·드라이브 저장)로 이어지고, 게이트에서 멈춘 그래프 상태는 응답으로 내보냈다가 다음 요청에서 돌려받아 이어 갑니다.
- **LLM 공급자·검색 전환 스위치** — 채팅·PPT 윤곽선(OpenAI ↔ vLLM), 웹 검색(Claude ↔ OpenAI), 사내 검색(pgvector ↔ LightRAG)을 기능별 설정값 하나로 바꾸게 했습니다. 웹 검색은 같은 질의로 직접 재서(Claude 6.5초, OpenAI 18.6초) 빠른 쪽을 기본값으로 정했습니다.
- **문서 파싱 파이프라인(팀 공동)** — 확장자 라우터·추출 프레임워크, Office(DOCX·PPTX·XLSX) 텍스트·이미지·표 추출기, PDF → 마크다운 변환(Marker)을 맡았습니다.
- **드라이브 AI 검색·PPT 자동 생성** — 질문을 임베딩(bge-m3)해 사용자 권한 폴더 안에서만 pgvector로 찾는 검색 API와, 프롬프트 → 윤곽선(JSON 생성·검증, 실패 시 재시도) → PPTX(Claude Agent Skills)로 이어지는 PPT 생성을 만들었습니다.
- **MCP 도구 연동** — Word·PowerPoint·HWPX·PDF·웹 검색 MCP 서버를 연결하고, 공공데이터 MCP 서버(나라장터·국회도서관·국가법령 API)를 직접 만들었습니다.
- **배포** — LLM 서버·Spring·Next.js 저장소의 GitHub Actions 배포 워크플로를 처음 만들었습니다.

**문제 해결 — 특정 권한 계정에서만 검색 결과가 비고 늦음**
- **문제** 드라이브 AI 검색에서 권한이 걸린 폴더의 문서가 적은 계정으로 물으면, pgvector 검색 결과가 돌아오지 않거나 늦었습니다.
- **원인** DB가 벡터 인덱스 대신 전체 행을 차례로 훑는 순차 스캔을 골라 시간 초과가 났습니다.
- **해결** 질의를 차원을 명시한 IVFFlat 표현식 인덱스에 맞게 고치고, 반복 탐색 상한(max_probes)을 정하고 순차 스캔을 꺼(enable_seqscan=off) 인덱스 탐색을 강제했습니다.

`FastAPI` `LangGraph` `vLLM` `OpenAI · Anthropic API` `PostgreSQL · pgvector` `MCP` `GitHub Actions`

</details>

#### RAG-Anything 문서 적재 파이프라인 <sub>업무 프로젝트 · 1인 설계·구축</sub>

PDF·Office·한글(HWP) 업무 문서를 표·이미지까지 읽어, 챗봇이 답의 근거로 찾아 쓸 벡터·지식그래프로 쌓는 파이프라인입니다.
- GPU 경합으로 초당 3토큰까지 떨어지던 LLM 생성을 CUDA MPS(GPU 하나를 여러 프로그램이 나눠 쓰는 기능)로 367~727토큰까지 회복
- GPU 서버까지 전달된 한글(HWP 5.0) 문서 80건 중 79건 적재

<details>
<summary>맡은 업무 · 문제 해결 자세히</summary>
<br>

**맡은 업무** — 도입 검토부터 적재 서버·GPU 게이트웨이·저장소 구성, 챗봇 검색 연계까지 혼자 맡았습니다(HWP→HWPX 변환기만 오픈소스를 가져다 씀).
- **한글 문서 처리 경로** — RAG-Anything이 지원하지 않는 한글 문서는 .hwp를 HWPX로 바꾼 뒤, 그 XML에서 본문·표·이미지를 바로 꺼내 적재합니다. OCR을 거치지 않아 원문이 그대로 남습니다.
- **GPU 게이트웨이** — 파싱·임베딩·LLM을 GPU 서버의 주소 하나(게이트웨이)로 묶어, 파일을 보내 결과만 받게 했습니다. 임대 GPU 주소가 이틀 새 다섯 번 바뀌어도 설정 한 줄만 고쳤습니다.
- **저장소를 한 곳에** — 벡터·지식그래프 저장소를 PostgreSQL 하나(pgvector + Apache AGE)로 모으고, 챗봇 사내 검색에 pgvector ↔ LightRAG(GraphRAG) 전환 스위치를 붙였습니다(권한 필터를 붙이기 전이라 전면 전환은 아직).

**왜 pgvector + Apache AGE** — 이미 bge-m3 벡터를 pgvector로 운영하고 있어, 그래프까지 같은 DB에 담아 백업·운영을 한 곳으로 모았습니다(버린 안: 그래프만 NetworkX 파일 — 규모 한계, Neo4j — 컨테이너 추가).

**문제 해결 — GPU 한 장을 문서 인식과 LLM이 나눠 쓰다 엔티티(개체) 추출 요청이 타임아웃**

| 방법 | LLM 생성(초당 토큰) | 결과 |
| :-- | :-- | :-- |
| 경합 없을 때(기준) | 525 | — |
| 같은 GPU에서 제한 없이 | 3까지 떨어짐 | 추출 호출 타임아웃 |
| 문서 인식(MinerU)을 CPU로 | 경합 사라짐 | 6분 넘게 문서 1건도 못 끝냄 |
| GPU + CUDA MPS로 문서 인식의 GPU 연산 몫을 30%로 제한 | **367~727** | 오류 0건 |

**문제 해결 — 파서 문제처럼 보인 적재 실패** — 형식 문제처럼 보이던 적재 실패를 원인별로 다시 세어, 99.4%가 파서가 아닌 GPU 연결 끊김임을 찾았습니다. 그래서 파서보다 인프라 실패 처리를 먼저 고칠 일로 정했습니다.

`RAG-Anything` `LightRAG` `MinerU` `vLLM` `Qwen2.5-VL-7B` `bge-m3` `FastAPI` `PostgreSQL` `pgvector` `Apache AGE` `CUDA MPS`

</details>

#### 영상 비식별화 솔루션 <sub>디비엔텍 · 2025.09 ~ 2025.12</sub>

영상·사진 속 얼굴·번호판을 AI로 찾아 가상 이미지로 바꾸는 솔루션(VisionON TAL)의 고도화와 인프라 구축을 맡았습니다.
- 트래킹을 걷어 내고 프레임 단위 탐지로 바꿔 잔상 오탐지 해결
- 인터넷이 막힌 고객사 폐쇄망에 도커로 구축

<details>
<summary>맡은 업무 · 문제 해결 자세히</summary>
<br>

**맡은 업무**
- **폐쇄망 인프라** — 외부 인터넷이 막힌 고객사 내부망(On-Premise)에서도 솔루션이 돌도록 아키텍처를 설계하고 환경을 설정했습니다.
- **도커 컨테이너화** — OS·하드웨어(CPU/GPU 드라이버 등)에 매이지 않고 어디서나 바로 돌도록 전체 시스템을 도커 컨테이너로 묶었습니다.
- **GUI·CLI 이원화** — 웹·데스크톱 화면에 더해, 서버에서 대량 데이터를 자동 처리하는 CLI 환경을 만들었습니다.

**문제 해결**
- **잔상 오탐지** — 지나간 객체가 다음 프레임에 잔상처럼 남는 원인이 기존 멀티 객체 트래킹의 한계임을 찾았습니다. 비식별화는 연속 추적보다 매 순간의 정확한 가림이 중요하다는 점에 맞춰, 트래킹을 걷어 내고 프레임 단위 탐지로 구조를 바꿨습니다.
- **중복 탐지** — 같은 객체를 여러 번 잡은 박스를, 겹친 비율(IoU)이 임계값을 넘으면 지우도록 NMS(겹친 박스 정리)를 고쳐 대상 객체만 잡게 했습니다.

`Docker` `객체 탐지 후처리(IoU·NMS)` `CLI`

</details>

### 개인 프로젝트

#### [한국어 문서 파서](https://github.com/ohdyo/kor_doc_parser) <sub>1인 · 진행 중</sub>

공문서·보도자료 같은 한국어 PDF를 RAG에 바로 쓰는 JSON·마크다운으로 바꾸는 파서와, 그림 해석 모델 튜닝입니다.
- 공개 문서 2건에서 표 머리행(헤더 행) 16/16 — docling 9/16 (고친 뒤 잰 값)
- 그래프 해석 정답률 57% → 81% (LoRA 튜닝, 통과 기준 미달)

<details>
<summary>맡은 일 · 결과 자세히</summary>
<br>

머리말·쪽번호·로고 같은 잡요소를 거르고, 표·제목·항목 계층·그림 속 글자까지 구조를 살립니다.

**맡은 일**
- **학습 없이 규칙으로** — 문서 인식 모델은 그대로 둔 채 PDF 글자층(텍스트 레이어)·PaddleX 내부를 감싸는 훅 4개·한국어 문서 규칙(□·○·- 개조식, '붙임' 제목)을 더했습니다.
- **비교군 셋과 나란히** — 고정 정답으로 텍스트 레이어만 뽑은 바닥선·docling·같은 모델 기본 설정과 같은 지표로 재고, docling보다 낮은 지표는 원인을 고쳐 다시 쟀습니다.
- **그림 해석 튜닝** — 정답을 코드가 계산하는 합성 그림 3,000장과, AI 라벨러 둘이 따로 매겨 합의한 실제 그림 정답 745개(학습 605 · 시험 140, 사람 확인 전)로 학습·시험 자료를 만들어, 국내 시각 언어 모델(A.X-4.0-VL-Light)을 A40 한 장에서 LoRA 튜닝했습니다.
- **운영** — 클라우드 GPU(A40)에 vLLM으로 모델을 띄워 페이지당 2~3초에 파싱하고, 켤 때마다 초기화되는 서버의 설치·서빙·학습·평가를 스크립트로 묶었습니다.

**왜 PaddleOCR-VL-1.6** — 후보 약 30개를 한국어 성능(가중치 35%)·속도 등 다섯 기준과 라이선스로 견줘, 탈락 사유가 없던 유일한 후보였습니다(문서 인식 벤치마크 MDPBench 한국어 86.3, Apache-2.0).

**결과** — 공개 정부 문서 2건 합계이며, 처음 잰 뒤 찾은 문제를 고쳐 다시 잰 값입니다. 텍스트 레이어 바닥선은 구조를 내지 않아(모두 0) 표에서 뺐습니다.

| 지표 | **이 파서** | docling | PaddleOCR-VL 기본 |
| :-- | :-: | :-: | :-: |
| 표 머리행(헤더 행) 일치 | **16/16** | 9/16 | 2/16 |
| 제목 찾음 | **29/30** | 27/30 | 15/30 |
| 항목 수준 일치 | **41/41** | 34/41 | 0/41 |
| 잡요소 제거(머리말·로고·쪽번호) | **50/50** | 24/50 | 47/50 |
| 그림 제목 연결 | **12/12** | 7/12 | 0/12 |

그림 해석 LoRA 튜닝으로 실제 문서 그래프 해석 정답률이 57%에서 **81%로** 올랐지만(물음 167개), 지도 방위는 19% 그대로였습니다. 판정용 개발 물음 25개에서는 어떤 확신 문턱을 잡아도 답한 것의 정답률이 미리 정한 기준(90%)에 닿지 않아(그래프만 봐도 최고 67%) 통과하지 못했고, 결과를 본 뒤에도 기준을 바꾸지 않고 그대로 기록했습니다.

`Python` `PaddleOCR-VL-1.6` `vLLM` `pypdfium2` `transformers` `peft(LoRA)` `NVIDIA A40`

</details>

#### [요깃거리](https://yogitgeori.moyeobom.com) <sub>공모전 팀 · 백엔드 · 2026.06 ~ 2026.09</sub>

관광지를 골라 주고 방문 기록·여행 영상으로 남겨 주는 웹 서비스입니다. 그중 여행 영상 제작·방문 기록·사진/영상 저장 백엔드를 맡았습니다.
- 운영 VM 인코딩 시간을 먼저 재어(출력 1초당 약 0.6~1초) 영상 합성을 비동기 구조로 설계
- 여행 영상 단위 테스트 47건

<details>
<summary>맡은 일 · 문제 해결 자세히</summary>
<br>

2026 관광데이터 활용 공모전 웹·앱 개발 부문 출품작입니다. 개발 4명(백엔드 3 · 프론트엔드 1)과 디자인 담당이 함께했고, 추천·도착 인증·스토리·퀴즈·알림·로그인, 방문 기록 자동 생성·지역별 목록, 화면은 팀원 몫입니다.

**맡은 일**
- **여행 영상 제작** — 방문한 지역의 사진·영상(1~20개)을 고르면 지역명이 들어간 세로 영상(1080×1920, 최대 90초)을 FFmpeg로 합성해 저장합니다.
- **사진·영상 일괄 업로드** — 파일마다 따로 검증·저장해 하나가 잘못돼도 나머지는 저장되고, MIME·확장자 교차 검증과 저장 경로 이탈 차단을 두었습니다.
- **저장소 전환** — 설정값 하나로 로컬 ↔ Cloud Storage를 바꾸게 해, 방문 기록 코드를 한 줄도 바꾸지 않고 옮겼습니다.

**왜 FFmpeg 직접 호출** — 래퍼 라이브러리는 JDK 21 지원이 불확실해, 셸 없이 ProcessBuilder로 직접 부르고 명령은 순수 Java로 조립했습니다. 그래서 필터 그래프를 단위 테스트로 검증할 수 있습니다.

**문제 해결 — VM 한 대에서 수십 초짜리 영상 합성을 API와 함께 돌려야 함**
- 만들기 전에 운영 VM(4 vCPU)에서 인코딩 시간을 직접 재어(출력 1초당 약 0.6~1초), 요청은 검증까지만 하고 202(접수)로 바로 돌려주는 비동기 구조로 설계했습니다.
- 합성은 전용 워커 1개(대기열 20, 넘치면 429)가 인코더 스레드 3개로 처리해 코어 하나는 API 몫으로 남기도록 했고, 운영 VM에서 사진 20장 → 63초 영상을 60초에 인코딩했습니다(당시 연출: 사진 3초 + 엔딩 카드 3초).

**문제 해결 — 인코딩 중 회원이 탈퇴하면 영상이 남을 수 있음** — 업로드 직전에 작업 기록을 다시 확인해 멈추고, 업로드 뒤 저장이 실패하면 올린 영상을 되돌려 지우도록(보상 삭제) 워커 테스트로 고정했습니다.

`Java 21` `Spring Boot 4` `JPA` `MySQL 8` `Google Cloud Storage` `FFmpeg` `JUnit 6` `Mockito` `GCP`

</details>

#### [워라벨 메이트](https://github.com/ohdyo/WLB_MATE) <sub>AI 캠프 · 팀 5명 · 2025.04 ~ 2025.06</sub>

회의록·문서 질의·민원 답변·상담 녹취 분석을 LLM으로 돕는 웹 서비스입니다. 상담 녹취 분석(CallMate)의 화면·LLM 피드백·저장 API, DB API 서버, 배포를 맡았습니다.
- 서버 3개를 nginx 한 주소로 묶어 AWS EC2·RunPod에 배포
- DB API 엔드포인트 17개 중 10개 직접 구현

<details>
<summary>맡은 일 · 문제 해결 자세히</summary>
<br>

<img src="https://github.com/user-attachments/assets/79db4aad-9fa6-44b0-bbed-713088275a65" width="110" alt="WLB MATE 로고" />

SK네트웍스 Family AI 캠프 9기 최종 프로젝트입니다. 음성 인식·Q&A 추출·근거 검색, 회의록 요약·문서 질의·민원 자동 답변은 팀원 몫입니다.

**맡은 일**
- **CallMate 피드백·저장** — LangChain으로 Qwen2.5를 불러 답변마다 피드백을 만들고, 통화 1건과 Q&A 여러 건을 한 트랜잭션으로 저장했습니다.
- **DB API 서버** — FastAPI로 controllers·services·models 3층 구조를 처음 만들고, 세션 로그인을 포함한 엔드포인트 17개 중 10개를 만들었습니다.
- **배포** — Dockerfile 4개와 설치 스크립트로 프론트·DB API는 AWS EC2에, GPU가 필요한 모델 서버는 RunPod에 올렸습니다.
- **그 밖에** — 모델 서버의 첫 구조와 Ollama 서빙, LangGraph 대화 기억 시제품을 만들었습니다.

**문제 해결**
- **LLM 피드백이 Q&A와 어긋남** — 출력 형식이 매번 달라 개수·순서가 맞지 않던 것을, 한 번 호출 + 출력 예시로 형식 고정 + 후처리로 풀어 Q&A 순서대로 붙였습니다(Q&A 수만큼 하던 LLM 호출을 1회로 줄임).
- **배포 뒤 CORS 오류** — nginx가 경로(/api, /model)별로 각 서버에 넘기게 하고, 제가 맡은 화면의 호출을 상대경로로 바꿨습니다(다른 화면은 팀원들이 같은 방식으로).

`Python` `FastAPI` `React` `MySQL` `Ollama(Qwen2.5)` `LangChain` `Docker` `nginx` `AWS EC2` `RunPod`

</details>

#### [RAG 구축과 검증 (LightRAG)](https://github.com/ohdyo/kor_doc_parser) <sub>1인 · 한국어 문서 파서 후속 · 진행 중</sub>

직접 만든 파서의 출력으로 LightRAG를 세우고, LightRAG · Naive RAG · RAG 없음 세 가지 답을 정답과 대조하고 있습니다.
- 답이 있는 질문 42개 중 맞히거나 일부 맞힌 답: RAG 없음 3 → Naive RAG 33 · LightRAG 32 (두 방식 차이 없음)

## 🛠 Tech Stack

| 분야 | 기술 |
| :-- | :-- |
| **LLM&nbsp;·&nbsp;Agent** | <img src="https://img.shields.io/badge/LangGraph-1C3C3C?style=flat-square&logo=langgraph&logoColor=white" alt="LangGraph" /> <img src="https://img.shields.io/badge/LangChain-1C3C3C?style=flat-square&logo=langchain&logoColor=white" alt="LangChain" /> <img src="https://img.shields.io/badge/vLLM-30A2FF?style=flat-square&logo=vllm&logoColor=white" alt="vLLM" /> <img src="https://img.shields.io/badge/Ollama-555555?style=flat-square&logo=ollama&logoColor=white" alt="Ollama" /> <img src="https://img.shields.io/badge/OpenAI%20API-412991?style=flat-square" alt="OpenAI API" /> <img src="https://img.shields.io/badge/Anthropic%20API-D97757?style=flat-square&logo=anthropic&logoColor=white" alt="Anthropic API" /> <img src="https://img.shields.io/badge/MCP-555555?style=flat-square&logo=modelcontextprotocol&logoColor=white" alt="MCP" /> |
| **Document&nbsp;AI&nbsp;·&nbsp;ML** | <img src="https://img.shields.io/badge/PaddleOCR--VL-0062B0?style=flat-square&logo=paddlepaddle&logoColor=white" alt="PaddleOCR-VL" /> <img src="https://img.shields.io/badge/Hugging%20Face-FFD21E?style=flat-square&logo=huggingface&logoColor=black" alt="Hugging Face" /> <img src="https://img.shields.io/badge/PyTorch-EE4C2C?style=flat-square&logo=pytorch&logoColor=white" alt="PyTorch" /> <img src="https://img.shields.io/badge/NVIDIA%20CUDA-76B900?style=flat-square&logo=nvidia&logoColor=white" alt="NVIDIA CUDA" /> |
| **RAG&nbsp;·&nbsp;DB** | <img src="https://img.shields.io/badge/PostgreSQL%20%C2%B7%20pgvector-4169E1?style=flat-square&logo=postgresql&logoColor=white" alt="PostgreSQL · pgvector" /> <img src="https://img.shields.io/badge/Apache%20AGE-D22128?style=flat-square&logo=apache&logoColor=white" alt="Apache AGE" /> <img src="https://img.shields.io/badge/LightRAG%20%C2%B7%20RAG--Anything-6A46B8?style=flat-square" alt="LightRAG · RAG-Anything" /> <img src="https://img.shields.io/badge/MySQL-4479A1?style=flat-square&logo=mysql&logoColor=white" alt="MySQL" /> |
| **Backend** | <img src="https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white" alt="Python" /> <img src="https://img.shields.io/badge/FastAPI-009688?style=flat-square&logo=fastapi&logoColor=white" alt="FastAPI" /> <img src="https://img.shields.io/badge/Java-ED8B00?style=flat-square&logo=openjdk&logoColor=white" alt="Java" /> <img src="https://img.shields.io/badge/Spring%20Boot-6DB33F?style=flat-square&logo=springboot&logoColor=white" alt="Spring Boot" /> <img src="https://img.shields.io/badge/JPA-59666C?style=flat-square&logo=hibernate&logoColor=white" alt="JPA" /> <img src="https://img.shields.io/badge/JUnit-25A162?style=flat-square&logo=junit5&logoColor=white" alt="JUnit" /> |
| **Frontend** | <img src="https://img.shields.io/badge/React-61DAFB?style=flat-square&logo=react&logoColor=black" alt="React" /> <img src="https://img.shields.io/badge/JavaScript-F7DF1E?style=flat-square&logo=javascript&logoColor=black" alt="JavaScript" /> |
| **Infra&nbsp;·&nbsp;DevOps** | <img src="https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white" alt="Docker" /> <img src="https://img.shields.io/badge/NGINX-009639?style=flat-square&logo=nginx&logoColor=white" alt="NGINX" /> <img src="https://img.shields.io/badge/GitHub%20Actions-2088FF?style=flat-square&logo=githubactions&logoColor=white" alt="GitHub Actions" /> <img src="https://img.shields.io/badge/AWS%20EC2-FF9900?style=flat-square" alt="AWS EC2" /> <img src="https://img.shields.io/badge/Google%20Cloud-4285F4?style=flat-square&logo=googlecloud&logoColor=white" alt="Google Cloud" /> <img src="https://img.shields.io/badge/Linux-FCC624?style=flat-square&logo=linux&logoColor=black" alt="Linux" /> <img src="https://img.shields.io/badge/Git-F03C2E?style=flat-square&logo=git&logoColor=white" alt="Git" /> |

## 📫 Contact

채용·협업 문의는 메일로 연락 주세요 — [leejh100984@gmail.com](mailto:leejh100984@gmail.com)

<img src="https://capsule-render.vercel.app/api?section=footer&type=wave&color=0:2f5fb3,100:24324a&height=110" width="100%" alt="" />
