# 7분 Proposal 발표 구성안

**편집 기준: 2026-10-15.** 본문 8장, 목표 6분 30초, 여유 30초. 영문 슬라이드와 한국어 발표 설명을 사용한다. 현재 PPT·PDF에 이 구성을 반영했다. 모든 배경은 흰색이며, 검정·회색 본문과 POSTECH #A61955 포인트를 사용한다.

## 평가 기준과 편집 방향

Lecture 1은 Proposal의 범위를 **Ideation, data collection & preprocessing, DB management**로 명시한다. 팀 프로젝트 공통 평가 항목은 Novelty, Impact, Technology, Presentation이다. 6주 일정은 확인된 제출 요구사항이 아니므로 발표 본문에서 제외한다. 상세 근거는 [평가 기준 기록](../evidence-and-references.md#course-requirements)을 따른다.

## 페이지별 내용

| 페이지 | 영문 제목 | 핵심 내용과 시각 자료 | 시간 |
|---|---|---|---|
| 1 | Meme Coin Analysis Dashboard | 프로젝트명과 팀원. 밈 이미지 하나로 주제 전달 | 10초 |
| 2 | Contents | Ideation / Data collection / Preprocessing / Database & application | 15초 |
| 3 | Why meme coins? | TRUMP 차트의 “what you want / after you buy”. 가격이 움직일 때 관측한 커뮤니티에서 어떤 이야기가 나왔는지 탐색하는 대시보드의 목적 | 55초 |
| 4 | Data overview | 가격의 숫자·날짜와 소셜의 텍스트·메타데이터를 비교. 확보한 10개 코인 가격 14,773행과 수집 예정 소셜 데이터 구분 | 40초 |
| 5 | Data collection | CMC Historical Data 페이지 크롤링, FxTwitter API, 조건부 Arctic Shift API·아카이브. 선정 프로젝트 사이트 HTML 크롤링은 링크·정체성 검증의 보조 경로. 일별 소셜 배치와 최근 7일 재조회 | 70초 |
| 6 | Data preprocessing | 가격 품질 점검, 게시물 ID 중복 제거, UTC 정렬, 언어·재게시 구분, 코인 연결. 게시물 한 건이 코인별 일별 지표로 변환되는 예시 | 65초 |
| 7 | Data pipeline & storage | 좌→우 전체 흐름도: 수집 → 원본 아카이브 → 전처리 → DB·ETL → 화면. 가격은 PostgreSQL, 소셜은 MongoDB로 적재하고 소셜 일별 집계는 PostgreSQL로 전달해 가격과 JOIN. SQL 차트·상태 조회와 MongoDB 게시물 상세 조회를 구분. PostgreSQL도 JSON을 지원하며 두 DB 선택 이유와 비용은 노트에서 설명 | 85초 |
| 8 | Dashboard & expected outcome | 구상 화면으로 가격·소셜 비교, 날짜별 게시물 탐색, 수집 상태 확인을 보여 줌. 상단의 짧은 흐름도로 수집부터 애플리케이션까지 연결 | 50초 |

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

[아키텍처의 발표용 Mermaid](../../architecture/system-design.md#발표용-전체-흐름)를 7장에 편집 가능한 도형과 화살표로 반영했다. 위쪽 가격 경로와 아래쪽 소셜 경로, MongoDB에서 PostgreSQL로 향하는 일별 ETL, 두 저장소의 화면 조회 경로를 함께 읽는다.
