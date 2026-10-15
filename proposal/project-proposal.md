# Meme Coin Analysis Dashboard

## Next-Day Price Direction Prediction from Market and Social Data

**Course:** Data Engineering — IDSC501 / IMEN891K / AXGS-501

**Team members:** TBD

**Submission deadline and format:** TBD

**Design baseline:** 2026-10-15

## 1. Problem and motivation

Meme coin prices and community discussions are available through different sources and formats. We propose a dashboard that combines numerical price histories with social text to predict whether a coin’s next UTC daily close will rise or fall relative to the preceding close. Users will see the direction, a short model-generated rationale, supporting posts, and the outcomes of previous predictions. Whether social inputs improve predictions is an empirical question, not an established result.

The main contribution is a reproducible data engineering pipeline: collection, raw preservation, preprocessing, document and relational storage, batch aggregation, and application delivery. The course assigns the proposal to ideation, collection/preprocessing, and database management; the final project emphasizes a useful product backed by a large-scale database. Our initial price dataset is small, so we will report actual social volume and measured performance rather than claim that the prototype already meets an unspecified scale threshold. See the [course evidence](evidence-and-references.md#course-requirements).

## 2. Objectives and research questions

The project will answer three questions:

1. Does combining price and social inputs improve next-day direction accuracy relative to the same model using prices alone and a simple previous-direction baseline?
2. Can each prediction be traced to the exact prices, posts, observation times, prompt, and model version available when it was issued?
3. How do missing coverage, ambiguous coin references, and delayed collection affect prediction availability and errors?

The first prediction cohort is DOGE, SHIB, PEPE, BONK, and SPX, matching the planned social scope. All ten coins remain available for market exploration. OpenAI API inference is a core component. No forecasting performance has been measured yet.

## 3. Data sources and current readiness

The team collected price histories by **crawling CoinMarketCap Historical Data pages** and supplied the results as CSV files: **14,773 daily records for ten coins**, with data ending on **2026-10-07**. The dataset contains OHLC prices, volume, market capitalization, supply, and timestamps. The ten-coin common period begins on 2025-10-04. An independent file inspection found no blank cells, duplicate coin-days, or internal date gaps, but identified zero-valued indicators and eight rows with high/low timestamps outside the daily interval. These observations require flags and interpretation rather than automatic deletion. The accompanying notebook references collection scripts and manifests that were not attached, so acquisition reproducibility remains a dependency. Full findings appear in the [input audit](evidence-and-references.md#attached-price-data).

For X, we will begin with selected DOGE, SHIB, PEPE, BONK, and SPX accounts through FxTwitter. Its current API documents account timelines and individual post retrieval. Planning-session checks returned timeline data, but did not establish reliable keyword search or complete historical coverage. We therefore describe this source as **selected-account activity**, not platform-wide attention. [FxTwitter API](https://docs.fxembed.com/api/introduction/)

For Reddit, the proposed adapter uses Arctic Shift for relevant DOGE, SHIB, and PEPE communities. Technical reachability does not establish permission: Reddit's current researcher policy excludes research using data collected outside its researcher program. The adapter will remain disabled unless applicable access and use conditions are resolved; the core demonstration can operate with price and eligible X observations. [Arctic Shift](https://github.com/ArthurHeitmann/arctic_shift), [Reddit policy](https://support.reddithelp.com/hc/en-us/articles/42728983564564-Responsible-Builder-Policy)

MemeChain informed the initial ideation about relevant websites and social channels. We will collect project data from the selected sites and accounts through web crawling or APIs, after verifying their identities and access conditions. MemeChain remains a background reference; its dataset and archived websites are not inputs to the pipeline. [MemeChain reference](https://zenodo.org/records/18246856)

## 4. Collection and preprocessing

The pipeline will preserve response bodies and retrieval metadata before normalizing records. Each artifact will carry its provider, retrieval time, checksum, and ingestion-run identifier. Daily social jobs will revisit the previous seven days; monthly jobs will create manifests and reconcile recent history. Price acquisition uses web crawling of CoinMarketCap Historical Data pages. The current pipeline imports the resulting CSV files; updates initially depend on additional CSV deliveries or restoration of the team's crawler. Real-time streaming is outside the initial scope.

Preprocessing will normalize timestamps to UTC, deduplicate native platform IDs, distinguish original posts from reposts, handle deletion markers, and retain only supported language inputs for sentiment analysis. Coin matching will combine verified accounts, contract identifiers, cashtags, and contextual rules. A ticker alone will not identify an asset, and community membership alone will not prove an explicit coin mention.

July–September 2026 remains a historical exploration window, conditional on social availability. Prediction evaluation will primarily use newly issued, prospective forecasts. Inputs must satisfy both an event cutoff and an actual availability cutoff; later acquisitions, edits, and engagement observations cannot enter earlier predictions. Historical replay is labeled separately, because contemporary archive retrieval and a model’s prior knowledge can contaminate retrospective tests. Collection failure remains missing or partial rather than zero activity.

## 5. Database management and architecture

We will use PostgreSQL for asset identities, daily prices, source registries, run metadata, post-to-asset mappings, immutable prediction metadata, outcomes, and derived analytical tables. Relational keys and constraints will support consistent joins and time-series aggregation.

MongoDB will store social documents and observation history. X posts and Reddit submissions/comments have different fields and nested structures. A document model supports these variations and the application's post-detail retrieval pattern. Common identifiers, timestamps, and schema versions will still be validated. MongoDB uses BSON internally; JSON is the interchange representation, not a separate database category. [MongoDB modeling](https://www.mongodb.com/docs/manual/data-modeling/), [JSON and BSON](https://www.mongodb.com/resources/basics/json-and-bson)

**Why MongoDB instead of PostgreSQL JSON?** PostgreSQL can store and query JSON, and a single-database design would be simpler to operate. We select MongoDB to manage heterogeneous social documents separately from relational market data and analytical tables, and to implement an explicit document-to-relational ETL process as part of the course. This is a modeling and educational choice, not evidence that MongoDB is inherently faster or that text requires NoSQL. Images are not required. The trade-off is additional deployment, backup, and cross-store consistency work. [PostgreSQL JSON support](https://www.postgresql.org/docs/current/datatype-json.html)

Validated prices enter PostgreSQL; normalized social documents and observation histories enter MongoDB. Python computes numerical features and constructs a frozen input from both stores. OpenAI API returns a direction and a concise rationale; validation precedes relational storage and publication. MongoDB also retains the inference request/response documents. PostgreSQL stores normalized results, evidence links, and outcomes. Original CSV/JSON files remain a replay aid, rather than a large standalone platform. Charts and prediction cards read PostgreSQL; post details read MongoDB.

## 6. Prediction workflow and dashboard

After UTC day d closes, the daily job freezes inputs at 03:00 UTC on d+1 and publishes by 03:15. It predicts whether close[d+1] will exceed close[d]. Source events must precede d+1 00:00, and their selected observations must have been acquired and validated by the input cutoff. This is a daily-close comparison issued with about 21 hours remaining, not an exact 24-hour return forecast. Equal closes are recorded as FLAT and excluded from binary accuracy with counts disclosed.

Python and pandas calculate returns, volatility, volume changes, and source-specific social activity. The initial input uses 31 daily closes, seven days of social features, and up to 30 deterministically selected text excerpts. The model receives these frozen inputs through the OpenAI API. A structured output contract restricts direction and evidence references; schema validation does not establish predictive accuracy. Unsupported evidence IDs, refusal, missing data, or failed calls result in an explicit failure or abstention. [Structured Outputs](https://developers.openai.com/api/docs/guides/structured-outputs)

A fusion prediction requires eligible social coverage and at least one usable text item. A separately labeled price-only prediction can still run when its own input requirements hold. Stale prices block current forecasts; restoring price updates is therefore a launch dependency. We will not display uncalibrated model confidence as a probability of success. VADER provides an optional reproducible sentiment feature, distinct from financial direction. [VADER](https://github.com/cjhutto/vaderSentiment)

Streamlit and Plotly will provide a coin selector, historical price chart, next-close prediction, short rationale, related posts/topics, a meme preview, and past outcomes. Original images are display context, not initial inference inputs. Coverage, issuance time, target date, and example/replay status remain visible. Docker Compose will run the databases and application; scheduled Python jobs perform collection and inference. Refreshing the page only reads stored results.

OpenAI API usage adds a budget dependency. Model choice, version, token limits, retry limits, and a daily cost cap must be configured before enabling inference. The planned five-coin, two-variant experiment schedules at most ten logical predictions per day; retries share the same budget. No paid API calls have been made for this proposal.

## 7. Evaluation and course alignment

Engineering evaluation will cover idempotent reruns, recovery after interruptions, pagination boundaries, schema validation, cross-store traceability, deletion propagation, and correct handling of missing coverage. Aggregate SQL queries must avoid multiplying price rows when joining posts. A stratified sample of up to 200 eligible items will support manual assessment of coin matching and sentiment; smaller datasets will be reviewed in full and reported as such.

Prediction evaluation compares a previous-direction baseline, price-only inference, and price-plus-social inference on paired eligible coin-days. We will report accuracy, balanced accuracy where both classes exist, a confusion matrix, per-coin counts, abstentions, failures, and coverage. Settings will be developed on earlier periods and fixed before evaluation on a later contiguous period. Historical replays and illustrative UI predictions are excluded from prospective scores. Daily time ordering follows standard forecasting evaluation practice. [Forecasting: Principles and Practice](https://otexts.com/fpp3/toolbox.html)

When the next close arrives, a scoring job adds an outcome without altering the original prediction. The team reviews errors and versions changes to prompts or input construction for future runs; this is a feedback process, not automatic model retraining. Small samples will be reported without claiming established predictive value.

We will measure chart-query latency against a two-second target with hardware, row counts, and cache conditions recorded. Novelty is the testable fusion of market and social inputs; impact is a traceable prediction interface; technology includes collection, SQL/NoSQL modeling, ETL, model integration, and evaluation. A successful pipeline remains a useful course outcome even if fusion fails to outperform the baselines.

## 8. Six-week work plan

| Week | Deliverable |
|---|---|
| 1 | Validate sources and access conditions; finalize asset registry, prediction target, cutoffs, budget, and database contracts |
| 2 | Implement eligible collectors, raw archives, checkpoints, and import of existing prices |
| 3 | Implement ETL, frozen prediction inputs, structured inference, and start prospective logging |
| 4 | Build the prediction dashboard, evidence navigation, and outcome scoring |
| 5 | Run chronological prediction comparisons, correctness, recovery, and performance checks |
| 6 | Resolve findings; prepare the demonstration, report, and reproducibility instructions |

This schedule is a planning assumption. Team assignments and the official deadline remain TBD.

## 9. Limitations and future work

The selected coins introduce survivorship and selection bias. Selected accounts are not representative of all users; archived communities can be incomplete; memes, irony, spam, and multilingual text limit sentiment quality. Third-party services can change or become unavailable. The dashboard will expose these limits and retain earlier results with their original target dates when a source fails. An old prediction will never be relabeled as today’s forecast. API cost, model variation, historical knowledge contamination, and a potentially short prospective sample also limit evaluation.

Image-based inference, OCR, embeddings, additional communities, and managed object storage are possible extensions after the text-and-price prediction pipeline is validated and source conditions permit them. Telegram or Discord will require verified message access rather than an invitation link alone. No extension is necessary to demonstrate the initial SQL/NoSQL integration.

Detailed implementation contracts: [prediction and evaluation](../architecture/prediction-design.md), [data model](../architecture/data-model.md), and [operations](../architecture/operations-and-validation.md).
