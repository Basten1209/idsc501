# Meme Coin Analysis Dashboard — 아키텍처

**설계 기준일: 2026-10-15.** 이 폴더는 구현할 시스템의 설계 명세다. 실행 가능한 수집기, DB 마이그레이션, Docker 환경, 대시보드는 아직 구현되지 않았다. 영문 제출 초안은 [Proposal](../proposal/project-proposal.md)에 있다.

## 읽는 순서

| 문서 | 주요 내용 |
|---|---|
| [전체 설계](system-design.md) | 목적, 범위, 시스템 흐름, PostgreSQL·MongoDB 선택 근거, 화면 구성 |
| [예측과 평가](prediction-design.md) | 다음 종가 목표, 시간 마감, 입력 구성, OpenAI API, 비교군과 피드백 |
| [데이터 모델](data-model.md) | 식별자, 관계형 테이블, MongoDB 문서, 인덱스, 파생 지표 |
| [수집과 아카이브](ingestion-and-archiving.md) | 소스별 크롤링·API 수집, 대상 사이트·계정 검증, 갱신 주기, 원본 보존 |
| [운영과 검증](operations-and-validation.md) | 배치 공개, 재시도·복구, 백업·삭제, 검증 기준, 6주 일정 |
| [근거와 출처](../proposal/evidence-and-references.md) | 강의 요구사항, 실제 데이터 점검, 외부 문서, 확인되지 않은 항목 |

## 확정한 기본값

- **PostgreSQL:** 코인·가격·관계·파생 지표·분석 테이블·예측·실제 결과·운영 메타데이터.
- **MongoDB:** 소셜 본문과 플랫폼별 문서, 관측 이력, 모델 요청·응답. JSON 형태로 다루되 내부 저장은 BSON이다.
- **원본 파일 아카이브:** API JSON 응답, CSV, HTML, 실행별 manifest. 파일 원문과 DB 문서는 별개다.
- **처리·화면:** Python, pandas, OpenAI API, Streamlit, Plotly. 배포 단위는 Docker Compose, 스케줄링은 호스트 cron과 단일 실행 잠금이다.
- **범위:** 가격 10종, X 선정 계정 5종, 조건부 Reddit 커뮤니티 3종. 가격+영어 텍스트의 방향 예측이 기본이며 이미지 추론은 확장이다.
- **예측:** 닫힌 UTC일의 다음 종가 UP/DOWN. 당시 입력과 원 예측을 고정하고 실제 종가로 평가한다. API 모델·예산은 구현 전에 확정한다.
- **갱신:** 소셜 일별 배치, 최근 7일 재조회, 월별 재검증과 아카이브. 실시간 스트리밍은 초기 범위에 포함하지 않는다.

DB 두 개를 선택한 이유와 비용은 [전체 설계의 결정 기록](system-design.md#3-db-선택-결정-기록)에서 관리한다. 소셜 전문의 정본은 MongoDB에 두고 PostgreSQL에는 식별·연결·분석용 투영만 둔다. 상세 설계를 수정하면 영문 Proposal의 관련 요약도 함께 갱신한다.

수집 가능 여부와 사용 조건은 별도 상태다. 특히 Reddit은 현재 정책 조건이 해결되기 전까지 비활성 상태로 설계한다. 미확보 데이터를 이미 확보한 것처럼 설명하거나, 문서에 정의한 검증을 이미 통과한 것처럼 표시하지 않는다.
