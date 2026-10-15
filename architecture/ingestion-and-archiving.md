# 데이터 수집과 아카이브

소스별 기술 문서는 2026-10-14 기준으로 검토했다. 상세한 실제 점검 결과는 [근거 문서](../proposal/evidence-and-references.md)에 구분해 기록했다. 아래 절차는 구현 명세이며 지속적인 수집 성공이나 전체 역사 데이터 확보를 뜻하지 않는다.

## 1. 소스별 계약

| 소스 | 가져올 데이터 | 초기 수집 경로 | 갱신과 제약 |
|---|---|---|---|
| CoinMarketCap 가격 | 일별 OHLCV·시가총액·공급량 | Historical Data 페이지 크롤링으로 수집한 CSV를 import | 추가 CSV 수령 시 import; 크롤러 복원 전에는 자동 최신화 불가 |
| X / FxTwitter API | 선정 계정의 게시물과 관측 시점 반응 수 | API v2 timeline, known-status lookup | 일별; 선정 계정 범위와 제공자 한계 표시 |
| Reddit / Arctic Shift | Reddit submission/comment | API community/time search, 필요 시 허용된 월별 archive | 접근·사용 조건 해결 전 disabled |
| 검증된 프로젝트 웹사이트 | 소셜 링크, 계약 주소 확인 근거 | 허용된 URL의 HTML 크롤링·파싱 | 소수 선정 사이트 일별 조건부 요청; 링크 변화는 검토 후 반영 |

초기 X 후보 handle은 `dogecoin`, `Shibtoken`, `pepecoineth`, `bonk_inu`, `SPX6900`이다. 응답이 온다는 사실만으로 공식 계정이라 확정하지 않는다. 플랫폼의 안정적인 계정 ID와 검증된 프로젝트 사이트의 역링크 등을 확인하고 선정 계정 registry에 등록한다. handle 변경 이력은 보존한다.

Reddit 후보는 `dogecoin`, `SHIBArmy`, `Pepecryptocurrency`다. 현재 Reddit 정책상 RFR 외 수집 데이터의 연구 사용 제한이 있으므로, Arctic Shift를 무료라는 이유만으로 활성화하지 않는다. 사용 조건이 해결되면 계약은 유지하면서 adapter를 활성화하고, 해결되지 않으면 unavailable을 표시한다. [정책 근거](https://support.reddithelp.com/hc/en-us/articles/42728983564564-Responsible-Builder-Policy)

## 2. 수집 대상 사이트·계정 선정과 검증

MemeChain은 관련 사이트와 소셜 채널을 탐색하는 ideation 단계에서 참고했다. 실제 파이프라인은 선정한 사이트·계정에 크롤링 또는 API 요청을 보내 데이터를 확보한다. MemeChain 데이터셋·웹사이트 아카이브를 적재하거나 Zenodo release를 정기 확인하는 작업은 두지 않는다. 참고 자료의 과거 점검 결과는 [근거 문서](../proposal/evidence-and-references.md#memechain-inspection)에 남긴다.

1. 분석할 코인별 프로젝트 사이트·X 계정·Reddit 커뮤니티 후보를 팀이 선정한다. 후보를 찾은 참고 자료와 실제 수집 대상 URL을 구분해 기록한다.
2. 프로젝트 사이트의 계약 주소, 소셜 링크, 계정 ID 등을 대조해 자산과 계정의 연결을 검증한다. native DOGE는 계약 주소 대신 chain·native identifier를 사용한다. ticker만으로 후보를 병합하지 않는다.
3. 실제 hostname과 path로 X profile/status/community, Reddit community/post, 일반 웹사이트 등을 구별한다. 사이트별로 API 조회 또는 HTML 크롤링·파싱 경로와 수집할 필드를 정한다.
4. 도메인 정규화·추적 query 제거·허용된 redirect 확인을 수행한다. 경로·주소의 대소문자를 무조건 낮추지 않는다. 원문 URL과 정규화 URL을 모두 저장한다.
5. 검증된 대상만 `source_endpoints`에 등록한다. 식별 근거, `first_seen_at`, `last_checked_at`, registry version, verification 상태와 access 상태를 기록한다. 새 링크가 생겨도 이전 링크를 덮어쓰지 않는다.
6. 수집기는 이 목록에 따라 선정 사이트를 크롤링하거나 해당 소셜 API를 호출한다. 수집 응답의 provenance는 실제 요청 URL·provider·관측 시각을 기준으로 남긴다.

계약 주소 검증의 초기 예시는 다음과 같다. 아래 주소는 검토 시작점이며 소셜 계정 소유권을 증명하지 않는다.

| 자산 | 식별 후보 | 주의점 |
|---|---|---|
| SHIB | Ethereum `0x95ad61b0a150d79219dcf64e1e6cc01f0b64c4ce` | 선정 사이트의 계약 주소와 소셜 링크 대조 |
| PEPE | Ethereum `0x6982508145454ce325ddbe47a25d4ec3d2311933` | `pepecoineth` 링크와 프로젝트 근거 대조 |
| BONK | Solana `DezXAZ8z7PnrnRJjz3wXBoRgixCa6xjnB7YaB1pPB263` | 주소 대소문자 유지 |
| SPX | Ethereum `0xe0f63a424a4439cbe457d80e4f4b51ad25b2c56c` | 다른 체인의 계정 연결을 자동 상속하지 않음 |

초기 범위의 검증된 코인·계정·사이트만 활성화한다. 미확인 계정은 후보로 남기고 공식 계정으로 표시하지 않는다.

## 3. FxTwitter 수집

기준 문서: [API 개요](https://docs.fxembed.com/api/introduction/), [OpenAPI 명세](https://github.com/FxEmbed/FxEmbed/blob/main/docs/specs/fxtwitter-openapi.json). 구현 시 사용한 명세 snapshot과 해시를 보관한다.

| 목적 | HTTP 경로 |
|---|---|
| 계정 타임라인 | `GET https://api.fxtwitter.com/2/profile/{handle}/statuses` |
| 알려진 게시물 조회 | `GET https://api.fxtwitter.com/2/status/{id}` |
| 키워드 탐색 | `/2/search`는 문서상 존재하나 MVP 필수 경로로 사용하지 않음 |

원본 응답은 먼저 저장하고 HTTP 상태와 JSON의 `code`를 함께 확인한다. list 결과의 `results`와 `cursor.bottom`을 사용해 진행한다. 요청 count와 응답 길이가 항상 같다고 가정하지 않는다. 단일 게시물은 status 응답 구조를 사용하는 별도 parser로 처리한다.

초기 backfill은 목표 구간 끝보다 최근인 페이지에서 시작해 필요한 기간까지 이동한다. 일별 job은 UTC 오늘 00:00을 상한으로 최근 7일을 다시 조회한다. `since` 등 최적화 옵션은 고정한 명세에 맞게 사용하되 cursor와의 호환성을 확인하고, 최종 범위 필터는 정규화된 사건 시각으로 적용한다.

페이지 정렬·고정 게시물·재게시의 영향 때문에 오래된 item 하나를 만났다는 이유로 종료하지 않는다. API의 정상 종료, 검증된 시간 경계, 또는 예산 소진을 구분한다. cursor 반복, 새 ID가 없는 반복 페이지, 불명확한 404는 진단 후 partial로 남긴다. 과거 구간까지 내려가지 못했으면 수집 가능한 구간만 표시한다.

원작성자가 선정 계정인지 확인해 `selected_account_original`을 계산한다. 타임라인에 노출된 다른 사람의 게시물은 참조 경로를 보존하되 선정 계정의 직접 작성 건수에 넣지 않는다. 재게시 시각을 확보하지 못하면 원게시 시각을 재게시 활동일로 대체하지 않는다.

기본 프로젝트 제한은 provider당 동시 요청 1개, 요청 사이 최소 2초, 실행당 최대 200개 요청이다. 이는 보장된 공급자 한도가 아닌 보수적인 초기 운영 설정이다. 역사 backfill은 여러 실행으로 나누며 예산 소진을 완료로 처리하지 않는다. 이용 조건이나 응답 헤더가 더 엄격하면 그 조건을 따른다.

API가 존재하거나 X 소비자 구독을 보유했다는 사실로 공식 X API 권한·완전한 검색·영구 접근을 가정하지 않는다. 검색이 안정화되더라도 새 cohort와 sampling 설명을 정의한 이후 추가한다.

## 4. Arctic Shift 수집 — 조건부 adapter

기술 기준: [API README](https://github.com/ArthurHeitmann/arctic_shift/blob/master/api/README.md). provider base는 `https://arctic-shift.photon-reddit.com`이며 post와 comment는 각각 `/api/posts/search`, `/api/comments/search`를 사용한다. 전체 텍스트를 키워드로 무차별 탐색하기보다 검증된 subreddit과 시간 구간을 먼저 좁힌다.

권한 조건을 해결한 이후의 초기 알고리즘은 다음과 같다.

1. 내부 시간 구간을 UTC 반열림 구간 `[start, end)`으로 정의한다. API `after`·`before`의 경계 의미는 작은 fixture로 확인하고, 경계 주변을 중복 요청한 뒤 로컬에서 반열림 필터를 적용한다.
2. 하루 구간, 오름차순, 명시적 limit=100으로 시작한다. posts와 comments를 독립적으로 수집한다.
3. 결과가 한도에 도달하거나 timeout이면 시간을 절반으로 나눠 재요청한다. 마지막 레코드의 초 단위 시각에 1초를 더해 이동하지 않는다. 같은 초에 여러 item이 있으면 누락될 수 있다.
4. 최소 구간에서도 포화되면 partial로 기록한다. 사용이 허용된 해당 월 archive가 있다면 별도 adapter로 재처리하고, 없으면 누락 구간을 남긴다.
5. provider·native ID·run provenance를 유지하며 중복을 제거한다. 모두 완료한 하위 구간만 체크포인트에 반영한다.

아카이브는 수집 시점 이후의 점수나 삭제 상태를 담을 수 있다. `retrieved_on`, `_meta.retrieved_2nd_on`, `_meta.was_deleted_later` 등 실제 응답의 필드를 parser version에 맞게 처리하고, 점수 관측 시각을 게시일로 대체하지 않는다. 삭제 표지와 `[deleted]`/`[removed]` 본문은 분석·공개 대상에서 제외한다. [필드 설명](https://github.com/ArthurHeitmann/arctic_shift/blob/master/file_content_explanations.md)

[월별 다운로드 목록](https://github.com/ArthurHeitmann/arctic_shift/blob/master/download_links.md)을 사용하는 경우 필요한 월만 선택하고 압축 해제는 streaming 방식으로 수행한다. 파일 크기·checksum·저장 여유를 먼저 확인한다. 전체 다년 dump를 기본 입력으로 삼지 않는다. 월별 dump의 공개 시점은 우리가 통제하지 못하므로 최근 월이 없을 수 있다.

403이면 해당 source job을 중단한다. 프록시나 User-Agent 교체로 차단을 우회하지 않는다. 광범위한 쿼리가 실패한 사실을 빈 커뮤니티로 해석하지 않는다. 무인 수집 전에 기술 접근과 사용 조건을 모두 충족해야 한다.

## 5. 가격 크롤링 결과의 import와 전처리

가격의 원래 수집 방식은 **CoinMarketCap Historical Data 페이지 크롤링**이다. 팀원이 크롤링한 결과를 CSV로 전달했으며, 현재 파이프라인은 이 파일을 입력으로 사용한다. 크롤링은 데이터 확보 단계이고 CSV import는 확보한 데이터의 적재 단계다.

ZIP 내부 파일 목록과 SHA-256을 확인한 뒤 세미콜론·UTF-8 BOM을 명시해 읽는다. 파일명은 사전에 정의한 CMC asset 매핑으로 변환한다. `name=2781`은 `source_name`으로 보존한다.

시간 파싱·UTC 날짜·Decimal 숫자 변환 후 PK 중복, 날짜 간격, OHLC 순서, 음수, 0, high/low 시각을 점검한다. 파싱 실패는 quarantine, 해석상 의심 값은 품질 플래그로 구분한다. 가격은 보간하지 않고 source row와 파일 checksum을 남긴다. 더 최신 제공본이 같은 날짜를 수정하면 이전 파일을 보존하고 새 run의 값으로 current price_daily를 갱신한다. 공개 mart는 별도 build로 다시 만든다.

첨부 notebook은 수집 코드가 있다는 설명을 포함하지만 그 코드와 관련 manifest는 제공되지 않았다. 자동 수집을 이미 복원했다고 표현하지 않는다. 추가 CSV 수령이 초기 운영 경로이며, 수집기 복원 후에는 날짜 구간별 요청과 연속성 검증을 다시 수행한다.

## 6. 원본 파일과 manifest

예시 경로는 향후 데이터 볼륨 내부 구조이며 저장소에 커밋할 파일이 아니다.

```text
archive/
  cmc/import_date=YYYY-MM-DD/<run_id>/input.csv.gz
  fxtwitter/fetch_date=YYYY-MM-DD/<run_id>/page-0001.json.gz
  arctic_shift/fetch_date=YYYY-MM-DD/<run_id>/page-0001.json.gz
  websites/fetch_date=YYYY-MM-DD/<run_id>/response.html.gz
  manifests/<run_id>.json
  builds/<build_id>.json
  monthly/YYYY-MM/manifest.json
quarantine/
  <run_id>/errors.jsonl
```

파일은 수신한 body bytes를 재직렬화하지 않고 압축한다. SHA-256은 압축 전 원문 기준으로 계산하고 checksum 기준을 manifest에 명시한다. artifact는 임시 파일로 기록·검증한 다음 atomic rename으로 확정한다. 정상 확정 후에만 MongoDB가 해당 URI를 참조한다.

실행 manifest의 필수 필드는 `run_id`, provider, endpoint, 인증 정보가 제거된 요청 조건, window, fetched_at, HTTP 상태, content type, raw URI·해시·바이트 수, parser/schema version, 이전·다음 checkpoint, run outcome이다. JSONL 파생 파일은 정확한 HTTP 원문과 구분한다. 동일 응답은 hash로 중복 저장을 줄일 수 있지만 관측 이력은 따로 유지한다.

월별 manifest는 해당 시점의 run·artifact·build·해시 목록을 고정한다. 기록이 같으면 파일 전체를 매월 복사하지 않는다. 삭제 처리 시에는 manifest에 제거 사실을 남기고 관련 원문을 복원하지 않는다. 로컬 파일 볼륨으로 시작하고, 나중에 같은 URI 계약을 객체 저장소 adapter로 확장한다.

## 7. 웹사이트와 링크의 일별 갱신

선정된 검증 사이트만 요청한다. ETag 또는 Last-Modified를 받았다면 다음 요청에 조건부 헤더를 사용한다. 304도 확인 시각과 상태를 남긴다. 해당 헤더가 없으면 HTML을 받아 해시로 변경 여부를 판단한다. HTML에서 링크를 추출하고 이전 endpoint 버전과 비교한다.

새 링크는 자동 승인하지 않고 검토 후보로 둔다. 웹사이트 200, Telegram landing page 200, Discord invite 유효성은 메시지 수집 가능성을 증명하지 않는다. 정적 HTML은 원문 증거로 보관하며 링크된 모든 자산을 포함한 완전한 웹사이트 재생을 보장하지 않는다.

외부 HTML과 JSON은 신뢰하지 않는 입력이다. 수집 대상 도메인을 명시하고 사설·로컬 주소 및 그쪽으로 향하는 redirect를 거부한다. HTML을 대시보드에서 실행하지 않고 게시물 텍스트를 이스케이프해 표시한다. 이미지·첨부 파일의 대량 다운로드는 초기 범위에서 제외한다. 대시보드의 소수 밈 미리보기는 표시 조건을 확인한 URL/별도 승인 자산을 사용하고, 초기 예측에는 이미지 입력을 넣지 않는다.

## 8. 정기 갱신과 확장 조건

일별로 최근 7일을 재조회하고, 매월 15일에는 직전 두 달의 가능한 archive·링크 상태·보관 중 콘텐츠의 삭제 상태를 재검증한다. 공개된 데이터만으로 삭제 여부를 확인할 수 없으면 확인 불가 상태를 표시하고 더 짧은 소스 정책이 있다면 그 정책을 우선한다. 정확한 실행 시각과 실패 처리는 [운영 명세](operations-and-validation.md)를 따른다.

Telegram은 API 계정 인증과 실제 채널 이력 접근, Discord는 봇·채널 권한과 메시지 본문 접근이 필요하다. 4chan /biz/는 공개 API 후보지만 빠르게 소멸하는 표본과 잡음 문제가 있다. DEX Screener는 소셜 링크 metadata 보완 경로이지 게시물 원문 대체재가 아니다. 어느 것도 초기 파이프라인의 필수 의존성으로 추가하지 않는다.

## 9. 예측 입력의 가용 시각

일별 예측에는 사건 시각 외에 실제 확보·검증한 `available_at`이 필요하다. 원본 CSV가 과거 날짜를 담더라도 오늘 받은 파일이면 그 기록의 최초 가용 시각은 오늘이다. 가격 정정은 price_observations에 새 버전으로 추가하고 현재 price_daily를 갱신한다. 과거 예측에 연결된 버전은 교체하지 않는다.

소셜의 최근 7일 재조회는 탐색 자료를 보완하지만 이전 예측의 입력을 갱신하지 않는다. 과거 게시물을 나중에 확보한 경우 역사 탐색과 재생에는 사용할 수 있어도 과거 prospective 입력으로 인정하지 않는다. `source_observed_at`이 오래되었더라도 우리 시스템의 가용 시각과 구별한다. 수집·삭제 조건은 OpenAI API로 텍스트를 전송하는 사용 목적까지 확인한다. 구체적인 마감과 조건은 [예측 설계](prediction-design.md)에 있다.

원본 파일 보관은 재처리와 lineage를 위한 보조 기능이다. 발표 도식에서는 CSV/JSON 형식과 DB 적재를 중심으로 표현하며, 별도 대형 아카이브 시스템을 구현한다는 뜻이 아니다.
