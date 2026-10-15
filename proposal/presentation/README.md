# Meme Coin Analysis Dashboard 발표 자료

**2026-10-15 예측 중심 개정본.** 영문 8장과 한국어 발표 노트로 구성했다. 목표 **6분 30초**, 발표 제한 7분이다.

[PowerPoint](memecoin-analysis-dashboard-prediction.pptx) · [PDF](memecoin-analysis-dashboard-prediction.pdf) · [한국어 대본](presenter-notes.ko.md) · [페이지 구성](slide-outline.md) · [출처](sources.md)

## 발표 흐름

| 장 | 내용 | 시간 |
|---|---|---|
| 1 | 표지 | 10초 |
| 2 | 목차 | 15초 |
| 3 | Ideation: 가격+소셜로 다음 방향을 예측할 수 있을까? | 50초 |
| 4 | Data collection: 확보 데이터와 크롤링/API | 60초 |
| 5 | Data pipeline: 저장·예측·결과 평가 | 80초 |
| 6 | Data preprocessing: 특성 구성·시점 고정 | 60초 |
| 7 | PostgreSQL·MongoDB 선택 | 55초 |
| 8 | 예측 대시보드 목업과 비교 평가 | 60초 |

강의 Proposal 범위는 ideation, collection/preprocessing, DB management다. 6주 일정은 확인된 발표 요구가 아니므로 본문에서 제외한다. 시간은 대본 배분이며 리허설 전 실제 측정값은 아니다. 팀원은 TBD다.

## 이번 설계의 기준

- 핵심 기능은 가격+소셜 텍스트를 사용하는 **다음 UTC 종가 방향 예측**이다. OpenAI API 추론은 기본 범위이며 정확도는 아직 측정하지 않았다.
- 4장에서 출처를 먼저 설명하고, 5장은 사용자 화이트보드와 [Mermaid](../../architecture/system-design.md#발표용-전체-흐름)를 바탕으로 가격/소셜 저장, Python 입력 구성, API 추론, 결과 저장, 실제 종가 평가를 연결한다.
- 원본 CSV·JSON 파일은 재처리를 위해 보관한다. 큰 Raw archive 단계를 발표의 본선에 두지 않는다.
- PostgreSQL은 관계·가격·예측·실제 결과, MongoDB는 소셜 문서·관측 이력·모델 artifact를 담당한다. PostgreSQL JSON도 가능한 대안이며 두 DB 선택에는 운영 비용이 따른다.
- 가격 CMC 크롤링, 소셜 FxTwitter/조건부 Arctic Shift, MemeChain의 ideation 참고 역할은 유지한다. 최신 예측에는 가격 갱신과 예측 시점에 확보된 소셜이 필요하다.

## 화면과 데이터

TRUMP 차트는 제공된 2025-01-18–2026-10-07 종가 628개다. “what you want / after you buy”는 실제 거래가 아닌 발표용 농담이며, 강조 구간은 최고 일별 종가까지다.

마지막 화면의 DOGE 차트는 2026년 3분기 종가 92개다. 9월 30일 종가 **$0.0944721408**, 직전 일 대비 **+0.6562627897%**를 화면에서 반올림한다. UP 예측·주제·근거는 **예시**이며 실제 모델 출력이 아니다. 적중률은 만들지 않았다. 밈 영역은 출처를 표시한 UI 배치 샘플이다. 구현된 서비스 화면으로 소개하지 않는다.

## 디자인과 편집

모든 배경은 흰색이다. 검정·회색과 [POSTECH #A61955](https://postech.ac.kr/kor/university-introduction/logo.do)를 사용한다. Geist의 글꼴·여백과 shadcn/ui의 단순한 경계·상태 표현을 참고했다. 목업의 화면 구성 요소와 파이프라인은 PowerPoint 도형/텍스트이고 가격 차트 2개와 수집 표도 편집 가능하다. 5장의 작은 화면만 8장 렌더 이미지다.

Geist와 Geist Mono는 PPT에 내장하지 않았다. 편집할 기기에 [공식 폰트](https://vercel.com/font)를 설치하거나 열람용 PDF를 사용한다. 각 장의 노트와 [크레딧](sources.md)에 출처를 기록했다. PPT 구조·차트 데이터 검사와 LibreOffice PDF 8장의 시각 검수를 완료했다. PowerPoint 앱에서 직접 검사했다고 주장하지 않는다.

기준 문서: [영문 Proposal](../project-proposal.md), [예측 설계](../../architecture/prediction-design.md), [전체 아키텍처](../../architecture/README.md). 이전 버전은 Git 이력에 남긴다.
