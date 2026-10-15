# 7분 Proposal 발표 구성안

**편집 기준: 2026-10-15.** 본문 8장, 목표 6분 30초, 여유 30초. 영문 슬라이드와 한국어 발표 설명을 사용한다. 현재 PPT·PDF에 이 구성을 반영했다. 모든 배경은 흰색이며, 검정·회색 본문과 POSTECH #A61955 포인트를 사용한다.

## 평가 기준과 편집 방향

Lecture 1은 Proposal의 범위를 **Ideation, data collection & preprocessing, DB management**로 명시한다. 팀 프로젝트 공통 평가 항목은 Novelty, Impact, Technology, Presentation이다. 6주 일정은 확인된 제출 요구사항이 아니므로 발표 본문에서 제외한다. 상세 근거는 [평가 기준 기록](../evidence-and-references.md#course-requirements)을 따른다.

## 페이지별 내용

| 페이지 | 영문 제목 | 핵심 내용과 시각 자료 | 시간 |
|---|---|---|---|
| 1 | Meme Coin Analysis Dashboard | 프로젝트명·팀원과 Doge 이미지 | 10초 |
| 2 | Contents | Ideation / Data pipeline / Collection & preprocessing / Database & application | 15초 |
| 3 | Why meme coins? | TRUMP 일별 종가의 “what you want / after you buy”. 가격과 관측한 커뮤니티 맥락을 함께 탐색하려는 목적 | 55초 |
| 4 | Data pipeline | 수업 사례를 참고한 아키텍처 개요. 출처 로고 → 원본 파일 → Python 전처리에서 가격·소셜 경로가 분기. MongoDB → 일별 ETL → PostgreSQL을 순서대로 배치하고 가격과의 합류를 강조. 점선은 하나의 대시보드로 향하는 조회 경로 | 55초 |
| 5 | Data collection | 확보한 10개 코인·가격 14,773행과 수집 예정 소셜을 구분. CMC Historical Data 크롤링, FxTwitter API, 조건부 Arctic Shift API·아카이브. 웹사이트 검증과 갱신 방식 | 70초 |
| 6 | Data preprocessing | 가격 품질 검증, ID 중복 제거, UTC 정렬, 언어·재게시 구분, 코인 연결과 일별 지표. 실패와 실제 0건 구분 | 65초 |
| 7 | Why two databases? | MongoDB의 문서 모델과 PostgreSQL의 관계형 분석을 비교. PostgreSQL도 JSON을 지원함을 명시하고 SQL/NoSQL 모델링·ETL 학습 목적과 운영 비용 설명 | 70초 |
| 8 | Dashboard & expected outcome | 가격·소셜 비교, 날짜별 게시물 탐색, 수집 상태를 구상 화면으로 보여 줌 | 50초 |

총 390초. TRUMP 차트는 제공된 2025-01-18–2026-10-07 일별 종가이며, 현재 가격이나 실제 거래 수익률로 설명하지 않는다. 구상 화면의 미수집 소셜 데이터를 실제 관측처럼 표시하지 않는다.

## 수집 설명의 확정 사항

| 실제 출처 / 접근 제공자 | 방법 | 확보할 내용 |
|---|---|---|
| CoinMarketCap Historical Data 페이지 | 웹 크롤링. 팀원이 수집한 결과를 CSV로 전달 | 일별 OHLCV·시가총액 등. CSV import는 수집 이후 적재 단계 |
| 선정 X 계정 / FxTwitter | 제3자 API의 timeline·status 조회 | 게시물 본문·게시 시각·반응 수와 관측 시각 |
| 선정 Reddit 커뮤니티 / Arctic Shift | 조건 충족 시 아카이브 API, 필요 시 허용된 월별 파일 | 글·댓글과 메타데이터 |
| 선정 프로젝트 웹사이트 | HTML 크롤링·파싱 | 소셜 링크·계약 주소 등 수집 대상 검증 근거 |

**MemeChain의 역할:** 활용할 사이트·소셜 채널을 탐색하는 ideation 참고 자료다. 데이터셋과 웹사이트 스냅샷을 프로젝트 입력으로 사용하지 않는다. 데이터 소스 표나 실행 파이프라인에 MemeChain을 넣지 않고, 참고문헌에서 역할을 설명한다. 실제 데이터는 선정한 사이트·계정에서 크롤링 또는 API로 확보한다.

발표용 영문 문장:

> MemeChain informed our selection of relevant sites and social channels. We will collect data from the selected sources through web crawling and APIs.

X의 선정 계정 활동을 전체 플랫폼 언급량으로 표현하지 않는다. Reddit은 기존 설계의 접근·사용 조건을 충족한 경우에만 활성화한다. 가격 갱신은 추가 CSV 또는 팀원 크롤러 복원에 의존한다.

API 경로, 페이지 이동, 원본 보존과 갱신 절차는 [수집 설계](../../architecture/ingestion-and-archiving.md)에 둔다. 긴 URL, 전체 ERD, 성능 시험 세부사항은 7분 본문에서 제외한다.

## 전체 파이프라인 도식

[아키텍처의 발표용 Mermaid](../../architecture/system-design.md#발표용-전체-흐름)를 **4장**의 편집 가능한 기술 도형과 연결선으로 반영했다. Lecture 1의 Netflix 사례에서 데이터 이동·조회 구분을, Coupang Eats 사례에서 경로 분기와 목적지 표현을 참고했다. 기술 구성은 우리 프로젝트의 Python·PostgreSQL·MongoDB·Streamlit 범위를 따른다.

파일은 접힌 모서리 도형, 처리 작업은 사각형, DB는 원통으로 구분한다. 가격은 PostgreSQL로 직접 이어지고, 소셜은 MongoDB와 Python 일별 집계를 거쳐 합류한다. 실선은 데이터 처리 흐름, 점선은 화면에 제공하는 읽기 경로다. 대시보드 축소 이미지는 기존 구상 화면의 일부분이며, 구현 완료된 앱을 뜻하지 않는다.
