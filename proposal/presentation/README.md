# Meme Coin Analysis Dashboard 발표 자료

**현재 버전: 2026-10-15 재제작본.** 영문 슬라이드 8장과 한국어 발표자 노트로 구성했다. 발표 목표는 **6분 30초**, 제한 시간 7분에서 30초의 여유를 둔다.

[PowerPoint 열기](memecoin-analysis-dashboard-white.pptx) · [PDF 보기](memecoin-analysis-dashboard-white.pdf) · [한국어 발표 대본](presenter-notes.ko.md) · [페이지별 구성과 평가 기준](slide-outline.md) · [출처와 이미지 크레딧](sources.md)

## 발표 구성

| 장 | 내용 | 목표 시간 |
|---|---|---|
| 1 | 표지 | 10초 |
| 2 | 목차 | 15초 |
| 3 | 주제 선정: TRUMP 차트와 커뮤니티 맥락 | 55초 |
| 4 | 확보한 가격 데이터와 수집 예정 소셜 데이터 | 40초 |
| 5 | CMC 크롤링, FxTwitter API, Arctic Shift | 70초 |
| 6 | 전처리와 일별 지표 생성 | 65초 |
| 7 | PostgreSQL·MongoDB·원본 아카이브와 ETL | 85초 |
| 8 | 대시보드 구상과 기대 결과 | 50초 |

강의자료가 명시한 Proposal 범위인 **Ideation, data collection & preprocessing, DB management**를 중심으로 구성했다. 6주 일정은 확인된 제출 요구사항이 아니므로 발표에서 제외했다. 팀원 이름은 아직 **TBD**다. 목표 시간은 대본 배분이며 실제 발표 시간은 리허설에서 확인해야 한다.

## 반영한 데이터 설명

- CMC 가격은 팀원이 **Historical Data 페이지를 크롤링한 결과**다. CSV는 수집 결과의 전달·적재 형식이다.
- X는 선정 계정의 FxTwitter API 조회, Reddit은 접근·사용 조건을 충족할 때 Arctic Shift API 또는 허용된 아카이브 파일을 이용할 계획이다.
- MemeChain은 수집할 사이트와 채널을 찾는 ideation 참고 자료다. 데이터셋을 파이프라인에 적재하지 않는다. 실제 자료는 선정한 사이트·계정에서 크롤링/API로 수집한다.
- PostgreSQL도 JSON을 지원한다. MongoDB는 플랫폼별 문서의 저장·조회 책임을 분리하고 SQL/NoSQL 사이의 ETL을 구현하기 위한 선택이다. 두 DB의 백업·일관성 관리 비용도 설명한다.

TRUMP 차트는 첨부 가격 데이터의 전체 제공 기간인 **2025-01-18–2026-10-07**, 일별 종가 628개를 사용한다. “what you want / after you buy”는 발표용 농담이다. 빨간 상승 구간의 끝은 장중 최고가가 아닌 **최고 일별 종가**다. 실시간 가격이나 실제 거래 결과가 아니다.

8장 대시보드는 구현 예정 화면이다. DOGE 차트는 제공 자료의 2026년 3분기 종가 92개이며, 미수집 소셜 데이터는 가짜 수치 대신 부재 상태로 표시했다.

## 디자인과 편집

모든 슬라이드 배경은 **흰색**이다. 본문은 검정·회색, 핵심 표현만 [POSTECH 공식 색상](https://postech.ac.kr/kor/university-introduction/logo.do) **#A61955**로 강조했다. shadcn/ui의 단순한 컴포넌트와 Geist의 타이포그래피·여백·얇은 경계선을 참고했다. 특정 UI 라이브러리로 PPT를 렌더링했다는 의미는 아니다.

웹에서 가져온 흑백 Doge 일러스트 1개와 소스·DB 아이콘 5개를 사용했다. 출처는 각 장의 노트와 [크레딧 문서](sources.md)에 있다.

본문 글꼴은 Geist, 코드와 일부 보조 표기는 Geist Mono다. 다른 기기에서 편집할 때는 [공식 Geist 폰트](https://vercel.com/font)를 설치하면 글꼴 대체를 피할 수 있다. 폰트 파일은 PPT에 내장하지 않았으며, PDF는 공유·열람용이다.

차트 2개, 수집 방법 표 1개와 흐름도는 편집 가능한 PowerPoint 개체다. 차트에는 사용한 값의 Excel 통합문서 스냅샷이 포함되어 있다. 외부 시세와 자동 동기화하는 연결은 없다. 최종 PPT의 구조·폰트·차트 데이터 검사를 통과했고, LibreOffice로 변환한 PDF의 8장을 모두 시각 검수했다. Microsoft PowerPoint 앱에서 직접 검사한 것은 아니다.

기준 문서는 [영문 Proposal](../project-proposal.md), [근거 문서](../evidence-and-references.md), [한국어 아키텍처](../../architecture/README.md)다. 이전 18장 초안은 Git 이력에 남아 있다.
