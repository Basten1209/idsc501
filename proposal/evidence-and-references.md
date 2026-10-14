# Evidence and references

**Baseline and verification date: 2026-10-14.** This document separates local inspection, external documentation, planning-session spot checks, and proposed behavior. Live observations are point-in-time evidence, not availability guarantees. No full social corpus or production collector is delivered with this documentation.

## Course requirements

The local source set is `lecturenote/1_overview.html` through `lecturenote/10_nosql-II.html`. These files are available in the workspace but are not committed with the proposal. Later topics appear on the course agenda; later lecture files were not provided.

| Evidence | What it establishes | Project response |
|---|---|---|
| Lecture 1, “Team Project Milestones” | Proposal: 20%; “Ideation, data collection & preprocessing, DB management” | Sections 1–5 of the proposal explicitly cover these requirements |
| Lecture 1, final milestone | Final: 40%; a useful real-world product using a large-scale database, with potential LLM integration | Dashboard plus a measurable pipeline; LLM is optional; actual scale must be reported |
| Lecture 1, grading and evaluation | Midterm 40%; criteria: Novelty, Impact, Technology, Presentation; team size ≤5 | Evaluation section and six-week work plan |
| Lectures 2–5 | Collection, exploration, preprocessing | Source audit, raw preservation, normalization, quality flags |
| Lectures 6–8 | SQL, relationships, joins, aggregation | Keys, constraints, daily marts, safe joins |
| Lectures 9–10 | JSON, document databases, MongoDB, and model/engine trade-offs | Document-to-relational ETL and an explicit database decision |

Lecture 9 explicitly shows JSON inside relational databases. Lecture 10 uses the Guardian's move from MongoDB to PostgreSQL JSONB to distinguish document modeling from engine choice. These lectures support a reasoned comparison, **not a claim that both database engines are mandatory**.

No exact proposal length, submission format, deadline, or numerical “large-scale” threshold was found in the supplied lecture set. These remain unverified. The six-week schedule and approximately 1,200–1,600-word draft are project planning defaults.

## Attached price data

Inputs inspected without executing the notebook:

| Input | Identity |
|---|---|
| Notebook | `.context/attachments/lezAdu/memecoin_eda.ipynb`; 30 cells |
| Notebook SHA-256 | `dc4e7ad79f337c04f1b5e91c7d9f341cd710997520e71d3f70f10cd1800f398e` |
| Price ZIP | `.context/attachments/ogAuZF/memecoin_price.zip`; 10 CSV files |
| ZIP SHA-256 | `af85be8f3fcfe890bd7a3dd14c475c44c05624a96c07edb51c720cb588fa7026` |

Files use a semicolon delimiter and UTF-8 with BOM. Each has 13 columns: `timeOpen`, `timeClose`, `timeHigh`, `timeLow`, `name`, `open`, `high`, `low`, `close`, `volume`, `marketCap`, `circulatingSupply`, `timestamp`. The notebook explains `name=2781` as the USD quote-currency identifier; it is not the asset name. Preserve it as `source_name`, and map each file to an explicit asset ID.

| Coin / input file stem | CMC ID in notebook | Rows | First UTC day | Last UTC day |
|---|---:|---:|---|---|
| Dogecoin | 74 | 4,680 | 2013-12-15 | 2026-10-07 |
| Shiba Inu | 5994 | 2,259 | 2020-08-01 | 2026-10-07 |
| Pepe | 24478 | 1,273 | 2023-04-14 | 2026-10-07 |
| MemeCore | 35491 | 462 | 2025-07-03 | 2026-10-07 |
| Pudgy Penguins | 34466 | 660 | 2024-12-17 | 2026-10-07 |
| OFFICIAL TRUMP | 35336 | 628 | 2025-01-18 | 2026-10-07 |
| SPX6900 | 28081 | 1,148 | 2023-08-17 | 2026-10-07 |
| Bonk | 23095 | 1,378 | 2022-12-30 | 2026-10-07 |
| FLOKI | 10804 | 1,916 | 2021-07-10 | 2026-10-07 |
| 币安人生 | 38590 | 369 | 2025-10-04 | 2026-10-07 |
| **Total** | | **14,773** | | |

Independent CSV checks on 2026-10-14 used Python's CSV parser, exact decimal values, and UTC timestamp parsing:

| Check | Result | Interpretation |
|---|---:|---|
| Blank cells | 0 | Does not establish semantic correctness |
| Duplicate dates within a coin | 0 | Candidate primary key: asset and UTC date |
| Missing calendar days inside each file's range | 0 | Does not imply identical start dates across coins |
| Negative numeric values | 0 | Source zeros still need interpretation |
| OHLC ordering violations | 0 | Low ≤ open/close ≤ high |
| Rows with high/low timestamps outside open–close interval | 8 | Retain with a quality flag |
| Zero volume rows | 54 | Preserve source values; do not assume missing or genuine inactivity |
| Zero market-cap rows | 1,096 | Flag; avoid treating unknown values as economically meaningful zeros |
| Zero circulating-supply rows | 1,096 | Same conservative treatment |

All ten coins overlap from **2025-10-04 to 2026-10-07**. The price data occupy about 3.4 MB uncompressed; this is not itself a large-scale database. The selected ten coins are a fixed cohort, not historical top-ten membership.

The notebook describes a public CMC historical-page backend, not a verified official Pro API integration. It reports expansion from 3,960 to 14,773 rows and a sparse response for an overly broad Dogecoin request, followed by requests of at most 300 days. The referenced `collect_cmc_history.py`, `requirements.txt`, collection summary, request archive, and manifest were not attached. File-level findings are independently checked; the full acquisition process has not been rerun. No prices after 2026-10-07 are currently supplied.

## MemeChain inspection

Primary sources: [Zenodo record](https://zenodo.org/records/18246856), [record README](https://zenodo.org/records/18246856/files/README.md?download=1), [metadata API](https://zenodo.org/api/records/18246856), and [paper](https://arxiv.org/html/2601.22185v1).

The record was published on 2026-01-14; its metadata declares CC BY 4.0. The paper places initial collection and website snapshots in October 2024, with lifecycle observations extending into January 2025. Publication time is not observation time. Dataset licensing does not grant rights to newly collected platform content.

During planning, selected CSV members were read through ZIP byte-range requests; the full approximately 710 MB archive was not downloaded or fully validated. The advertised ZIP MD5 is `ee341d14684471ce1da6a8599b422e6e`; a full-download checksum match has **not** been established.

| Inspected item | Finding |
|---|---|
| Master table | 34,988 rows; includes native/other-chain entries |
| Filtered/social table | 31,811 rows; columns `name`, `platform`, `website`, `twitter`, `telegram`, `discord`, `symbol`, `address` |
| Nonempty fields | Website 23,797; Twitter 22,343; Telegram 26,105; Discord 1,963 |
| X profile candidates across all four link columns | 20,504 distinct syntactically normalized candidates; not verified live accounts |
| X links in the Twitter column | 21,185 profile paths, 286 status paths, 202 community paths, 162 other X paths |
| HTML mapping table | 6,776 rows; a mapping-row count, not an independently verified count of usable website archives |

The link column names are unreliable classifiers: Telegram links can occur in Twitter/Discord columns, and Reddit links occur in several columns. Parse the URL host and path. Nonempty fields are neither valid-link counts nor message-access counts.

Asset matching examples from the inspected rows:

- Native DOGE appears in the master data but not the social subset.
- Canonical Ethereum SHIB has blank social fields; similarly named entries must not supply its account automatically.
- Canonical Ethereum PEPE links to `pepe.vip` and `pepecoineth`.
- Canonical Solana BONK links to `bonkcoin.com`, `bonk_inu`, and `Official_Bonk_Inu`.
- SPX's Ethereum row has a website and Telegram portal but no X value; wrapped-chain rows contain an X account. Cross-chain inheritance needs verification.

The master data contain repeated symbols, including DOGE, SHIB, PEPE, and SPX. Join through verified chain/contract identities, not symbol alone. MemeChain supplies endpoint and website evidence; its logos are not a corpus of social meme images and its CSV links are not social messages.

## Source feasibility and limits

The following are planning-session observations on 2026-10-14. They are recorded separately from documented API capabilities. Full raw response fixtures were not retained for every probe, so these are a feasibility log, not a reproducible benchmark.

| Source / probe | Observation | Design consequence |
|---|---|---|
| FxTwitter v2 selected-account timelines | DOGE, SHIB, PEPE, BONK, SPX returned HTTP 200; a two-page DOGE traversal returned older records | Use bounded account timelines, still measure coverage |
| FxTwitter individual status | A known status returned HTTP 200 | Can enrich discovered IDs |
| FxTwitter search | Sample keyword/date queries returned 404 with empty results | Search exists in documentation but is not a core dependency |
| FxTwitter timeline payloads | Some timelines include reposts from other authors; returned count can differ from requested count | Separate original author from collection endpoint; do not stop solely on page length |
| Arctic Shift | Narrow Dogecoin historical/current queries succeeded; broader queries timed out with 422; later requests returned 403 | No guaranteed backfill or daily availability; stop at access denial |
| BONK/SPX websites | Returned HTML; contained social links; SPX HTML exposed its Ethereum contract | Useful endpoint-verification evidence, not proof of platform access |
| Telegram landing links | BONK/SPX landing pages opened, without message-history widgets | A working landing page is not a readable message feed |
| DEX Screener metadata | Sample requests returned 403 | Optional enrichment only; no successful runtime integration claimed |
| 4chan /biz/ catalog | Public catalog responded | Possible future source, but a short-lived, noisy sample; not initial scope |

The current [FxTwitter API overview](https://docs.fxembed.com/api/introduction/) and [OpenAPI specification](https://github.com/FxEmbed/FxEmbed/blob/main/docs/specs/fxtwitter-openapi.json) document v2 timelines, status lookup, and search. An X consumer subscription is not evidence of API entitlement. The project does not assume paid X API access or a completeness guarantee from the proxy.

[Arctic Shift API documentation](https://github.com/ArthurHeitmann/arctic_shift/blob/master/api/README.md) describes time-filtered post/comment searches without an uptime guarantee. Its [field explanations](https://github.com/ArthurHeitmann/arctic_shift/blob/master/file_content_explanations.md) distinguish later retrieval/deletion metadata. [Download listings](https://github.com/ArthurHeitmann/arctic_shift/blob/master/download_links.md) provide archival alternatives, but full historical dumps are inappropriate for this small initial scope. None of these mechanisms resolves the source-use condition below.

### Source-use condition

Reddit's [Responsible Builder Policy](https://support.reddithelp.com/hc/en-us/articles/42728983564564-Responsible-Builder-Policy) states that research using data collected outside RFR violates that policy. Therefore Arctic Shift is a **conditional adapter, disabled by default for collection and analysis**, pending documented applicable authorization or policy change. Instructor approval alone does not replace platform conditions. If unresolved, the submitted prototype will label Reddit unavailable and use eligible price/X data; it will not silently substitute a different Reddit scraping route.

Public endpoints and dataset licenses do not automatically establish rights to all downstream content. Record source-specific access, attribution, retention, and display conditions. Do not bypass 403 responses, authentication, or rate limits. This is a source-readiness dependency, not a claim that permissions have been obtained.

## Alternatives and reference register

These references support technology facts or identify extension requirements. They do not establish successful integrations. Checked or reviewed during planning as of 2026-10-14; mutable documentation must be rechecked when implementing a collector.

| Reference | Role in this project |
|---|---|
| [PostgreSQL JSON types](https://www.postgresql.org/docs/current/datatype-json.html) | PostgreSQL supports both JSON and JSONB; a valid simpler alternative |
| [MongoDB data modeling](https://www.mongodb.com/docs/manual/data-modeling/) | Document design, nested fields, access patterns, validation |
| [MongoDB JSON and BSON](https://www.mongodb.com/resources/basics/json-and-bson) | Distinguishes interchange format and storage representation |
| [VADER](https://github.com/cjhutto/vaderSentiment) | Proposed English sentiment baseline, not a financial classifier |
| [DEX Screener API](https://docs.dexscreener.com/api/reference) | Optional token/social metadata; public metadata is not social text |
| [Telegram API](https://core.telegram.org/api), [history method](https://core.telegram.org/method/messages.getHistory), [API terms](https://core.telegram.org/api/terms) | Requires a suitable authenticated access route and use review; AI/ML restrictions also matter |
| [Discord messages](https://docs.discord.com/developers/resources/message#get-channel-messages) | Channel permissions and content access are required; invite links are insufficient |
| [4chan API](https://github.com/4chan/4chan-API) | Optional public discussion source with short-lived coverage and separate sampling limits |

No alternative community source is required for the first demonstration. Additional free sources are useful only if their identity, access, relevance, and coverage can be established without destabilizing the six-week plan.

## What remains to be verified

- Actual deadline, submission format, team responsibilities, and instructor expectations for scale.
- Restoration or replacement of the price acquisition workflow.
- Continued FxTwitter access, account identity verification, and obtainable historical range.
- Applicable Reddit authorization; current design does not assume it.
- Achieved social volume, matching/sentiment accuracy, dashboard latency, and recovery behavior. These are implementation evaluation targets, not completed results.
