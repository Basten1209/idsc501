# 7분 Proposal 발표 구성안

**편집 기준: 2026-10-15.** 영문 8장, 한국어 발표 노트, 목표 6분 30초. 흰 배경과 검정·회색 본문, POSTECH #A61955 강조를 유지한다. 핵심 애플리케이션은 가격·소셜 데이터를 함께 사용하는 다음 UTC 종가 방향 예측이다.

## 평가 기준과 구성

Lecture 1의 Proposal 범위인 **Ideation, data collection & preprocessing, DB management**를 중심으로 구성한다. 예측은 구현할 기능과 검증할 가설이며 성능을 입증한 결과로 발표하지 않는다. 팀 프로젝트 기준 Novelty, Impact, Technology, Presentation은 [근거 문서](../evidence-and-references.md#course-requirements)와 연결한다. 6주 일정은 확인된 발표 요구가 아니므로 본문에서 제외한다.

| 장 | 제목 | 내용 | 시간 |
|---|---|---|---|
| 1 | Meme Coin Analysis Dashboard | 가격+소셜로 다음 가격 방향을 예측하는 프로젝트 | 10초 |
| 2 | Contents | Ideation, 수집, 파이프라인·전처리, DB·화면 | 15초 |
| 3 | Why meme coins? | 머스크·트럼프 실제 게시와 분봉 상승 사례. “When we buy…” + 우는 페페·되돌림으로 동기 제시 후 예측 질문 | 50초 |
| 4 | Data collection | CMC Historical Data 크롤링, X/FxTwitter API, 조건부 Reddit/Arctic Shift. 확보한 가격 14,773행·10종과 수집 예정 소셜 구분 | 60초 |
| 5 | Data pipeline | 가격/소셜 저장 경로 → Python 동결 입력 → OpenAI API → PostgreSQL 예측 → 화면. 실제 종가 평가와 다음 버전 개선 피드백 | 80초 |
| 6 | Data preprocessing | 전처리 후 가격·소셜 특성과 가용 시각을 고정. 미래 정보 혼입 방지, 누락/실패 구분 | 60초 |
| 7 | Why two databases? | MongoDB 문서·관측·모델 artifact와 PostgreSQL 관계·가격·예측·결과. JSON 대안과 운영 비용 | 55초 |
| 8 | Prediction dashboard | 코인 선택, 가격 차트, 다음 종가 방향, 근거/주제·밈, 과거 결과. 가격은 실제 제공 데이터, 예측과 소셜 텍스트는 예시 | 60초 |

총 **390초**. 30초의 여유를 두며 실제 발표 시간은 리허설에서 확인한다. 팀원 이름은 TBD다.

## 설계와 화면의 의미

3장은 “When they post”와 “When we buy…”를 한 화면에 배치한다. 왼쪽은 실제 게시 문구·플랫폼·날짜와 분봉 차트, 오른쪽은 흑백 우는 페페와 실제 TRUMP 되돌림이다. 두 사건은 선택 사례이며 게시물의 인과 효과나 다음 날 예측력을 입증하지 않는다. Binance 분봉과 Truth Social은 발표 동기 자료이고 수집 대상 확대를 뜻하지 않는다. 머스크의 검증된 문구는 “Doge”이며 “Doge to the moon!”을 직접 인용으로 만들지 않는다.

5장은 [발표용 Mermaid](../../architecture/system-design.md#발표용-전체-흐름)와 사용자의 화이트보드 스케치를 반영한다. 큰 Raw archive 상자는 없애고 CSV·JSON을 수집 결과 형식으로 표시한다. 원본 파일 보관은 재처리용 보조 설명으로 남긴다. 가격 이력을 읽는 화살표와 예측을 저장하는 화살표를 구분하고, 점선은 실제 결과 검토에 따른 향후 입력·프롬프트 개선이다. 자동 학습이나 주문을 뜻하지 않는다.

8장은 실제 앱처럼 구성한 편집 가능한 목업이다. DOGE 가격 차트와 종가·전일 대비 변화는 제공 데이터에서 계산한다. UP, 주요 주제와 설명은 **EXAMPLE**, 전체 화면은 **ILLUSTRATIVE UI**로 표시한다. 밈 패널은 웹에서 확보한 Icons8 일러스트를 이용한 배치 예시다. 예측 경로에 이미지 분석을 추가한 것은 아니다. 원 예측에 대한 실제 평가 기록이 아직 없어 성과 수치 대신 대기 상태를 표시한다.

## 반드시 유지할 구분

- CMC는 Historical Data 페이지 **크롤링**이며 CSV는 전달 형식이다. 최신 예측은 추가 가격 수집에 의존한다.
- MemeChain은 대상 사이트·채널을 찾은 참고 자료다. 입력 데이터셋으로 표시하지 않는다.
- X는 선정 계정 범위, Reddit은 조건 충족 후 활성화한다.
- 다음 UTC 종가의 UP/DOWN을 예측한다. 사건 마감과 실제 확보 마감을 별도로 적용하고, 가격·소셜 입력을 고정한다.
- 가격 전용/가격+소셜과 직전 방향 기준선을 같은 코인·날짜로 비교한다. 과거 재생과 신규 발행 예측은 구분한다.
- LLM 출력의 형식 검증과 예측 정확도는 별개다. 예시 예측, 가짜 적중률, 보정하지 않은 확률을 실적처럼 표시하지 않는다.

세부 구현은 [예측 계약](../../architecture/prediction-design.md), [수집 설계](../../architecture/ingestion-and-archiving.md), [데이터 모델](../../architecture/data-model.md)에 둔다.
