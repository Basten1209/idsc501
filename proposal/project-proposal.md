# Meme Coin Analysis Dashboard

## A Reproducible Batch Pipeline with Relational and Document Databases

**Course:** Data Engineering — IDSC501 / IMEN891K / AXGS-501

**Team members:** TBD

**Submission deadline and format:** TBD

**Design baseline:** 2026-10-14

## 1. Problem and motivation

Meme coin market activity is often discussed alongside online community activity, but price histories and social records differ in structure, availability, and observation time. A chart can imply a relationship even when its social sample covers only selected accounts or contains collection gaps. Our project addresses this problem through a dashboard that makes market and social observations comparable while displaying their provenance, coverage, and freshness.

The main contribution is a reproducible data engineering pipeline: collection, raw preservation, preprocessing, document and relational storage, batch aggregation, and application delivery. The course assigns the proposal to ideation, collection/preprocessing, and database management; the final project emphasizes a useful product backed by a large-scale database. Our initial price dataset is small, so we will report actual social volume and measured performance rather than claim that the prototype already meets an unspecified scale threshold. See the [course evidence](evidence-and-references.md#course-requirements).

## 2. Objectives and research questions

The dashboard will let users inspect market movements, compare them with observed social activity, and trace aggregated signals back to collected posts. It will address three questions:

1. How do daily returns, trading volume, and volatility vary across the selected coins?
2. How do selected-account activity and eligible community discussions co-vary with those market measures, including exploratory lead–lag relationships?
3. How do source coverage, reposts, collection delays, and ambiguous coin references affect the interpretation of those relationships?

These are descriptive questions. Correlation will not establish causation, and the project will not present trading recommendations or a validated price forecasting system.

## 3. Data sources and current readiness

The team collected price histories by **crawling CoinMarketCap Historical Data pages** and supplied the results as CSV files: **14,773 daily records for ten coins**, with data ending on **2026-10-07**. The dataset contains OHLC prices, volume, market capitalization, supply, and timestamps. The ten-coin common period begins on 2025-10-04. An independent file inspection found no blank cells, duplicate coin-days, or internal date gaps, but identified zero-valued indicators and eight rows with high/low timestamps outside the daily interval. These observations require flags and interpretation rather than automatic deletion. The accompanying notebook references collection scripts and manifests that were not attached, so acquisition reproducibility remains a dependency. Full findings appear in the [input audit](evidence-and-references.md#attached-price-data).

For X, we will begin with selected DOGE, SHIB, PEPE, BONK, and SPX accounts through FxTwitter. Its current API documents account timelines and individual post retrieval. Planning-session checks returned timeline data, but did not establish reliable keyword search or complete historical coverage. We therefore describe this source as **selected-account activity**, not platform-wide attention. [FxTwitter API](https://docs.fxembed.com/api/introduction/)

For Reddit, the proposed adapter uses Arctic Shift for relevant DOGE, SHIB, and PEPE communities. Technical reachability does not establish permission: Reddit's current researcher policy excludes research using data collected outside its researcher program. The adapter will remain disabled unless applicable access and use conditions are resolved; the core demonstration can operate with price and eligible X observations. [Arctic Shift](https://github.com/ArthurHeitmann/arctic_shift), [Reddit policy](https://support.reddithelp.com/hc/en-us/articles/42728983564564-Responsible-Builder-Policy)

MemeChain will provide candidate websites and social endpoints, not a corpus of social posts. We will verify links and asset identities before collection. Its publication date must not be mistaken for the time its websites were collected. [MemeChain dataset](https://zenodo.org/records/18246856)

## 4. Collection and preprocessing

The pipeline will preserve response bodies and retrieval metadata before normalizing records. Each artifact will carry its provider, retrieval time, checksum, and ingestion-run identifier. Daily social jobs will revisit the previous seven days; monthly jobs will create manifests and reconcile recent history. Price acquisition uses web crawling of CoinMarketCap Historical Data pages. The current pipeline imports the resulting CSV files; updates initially depend on additional CSV deliveries or restoration of the team's crawler. Real-time streaming is outside the initial scope.

Preprocessing will normalize timestamps to UTC, deduplicate native platform IDs, distinguish original posts from reposts, handle deletion markers, and retain only supported language inputs for sentiment analysis. Coin matching will combine verified accounts, contract identifiers, cashtags, and contextual rules. A ticker alone will not identify an asset, and community membership alone will not prove an explicit coin mention.

The initial historical comparison window is July–September 2026, conditional on obtainable social records. Missing history will remain visible. Engagement collected today will be labeled with its observation time instead of being treated as information available on the original posting date. Collection failure will produce an unavailable or partial status, not a zero-activity day.

## 5. Database management and architecture

We will use PostgreSQL for asset identities, daily prices, source registries, run metadata, post-to-asset mappings, and derived analytical tables. Relational keys and constraints will support consistent joins and time-series aggregation.

MongoDB will store social documents and observation history. X posts and Reddit submissions/comments have different fields and nested structures. A document model supports these variations and the application's post-detail retrieval pattern. Common identifiers, timestamps, and schema versions will still be validated. MongoDB uses BSON internally; JSON is the interchange representation, not a separate database category. [MongoDB modeling](https://www.mongodb.com/docs/manual/data-modeling/), [JSON and BSON](https://www.mongodb.com/resources/basics/json-and-bson)

**Why MongoDB instead of PostgreSQL JSON?** PostgreSQL can store and query JSON, and a single-database design would be simpler to operate. We select MongoDB to manage heterogeneous social documents separately from relational market data and analytical tables, and to implement an explicit document-to-relational ETL process as part of the course. This is a modeling and educational choice, not evidence that MongoDB is inherently faster or that text requires NoSQL. Images are not required. The trade-off is additional deployment, backup, and cross-store consistency work. [PostgreSQL JSON support](https://www.postgresql.org/docs/current/datatype-json.html)

The data flow is raw archive → MongoDB social documents → preprocessing and aggregation → PostgreSQL analytical tables → dashboard. Prices enter PostgreSQL after validation. Charts query PostgreSQL; post details query MongoDB by stable ID. The raw archive preserves exact acquired responses for replay, subject to deletion and retention obligations. A versioned, validated batch is published atomically in PostgreSQL so interrupted processing does not replace the last successful chart dataset.

## 6. Dashboard and expected outputs

Python and pandas will handle transformations; Streamlit and Plotly will provide four views: market overview, price–social comparison, post exploration, and data health. Docker Compose will define PostgreSQL, MongoDB, and application services; a simple scheduler will run batch jobs.

Measures will include daily returns, 30-day volatility, observed post counts, seven-day activity averages, and a basic English sentiment score. VADER is a proposed baseline whose interpretation will be checked manually; linguistic positivity is not equivalent to financial bullishness. [VADER](https://github.com/cjhutto/vaderSentiment)

Exploratory correlations will use paired valid observations, display sample counts, and require at least 30 paired days. Lead–lag views will examine a predefined ±7-day range. Every view will display the selected source cohort, available dates, and freshness. Expected outputs include the reproducible pipeline, database models, dashboard, validation report, and source-to-result lineage examples.

## 7. Evaluation and course alignment

Engineering evaluation will cover idempotent reruns, recovery after interruptions, pagination boundaries, schema validation, cross-store traceability, deletion propagation, and correct handling of missing coverage. Aggregate SQL queries must avoid multiplying price rows when joining posts. A stratified sample of up to 200 eligible items will support manual assessment of coin matching and sentiment; smaller datasets will be reviewed in full and reported as such.

We will measure chart-query latency against a two-second target, recording hardware, row counts, and cache conditions. Synthetic stress data, if used, will be separated from collected evidence. Novelty lies in integrating market and social observations with explicit coverage; impact comes from making comparisons interpretable; technology is demonstrated through collection, preprocessing, SQL/NoSQL modeling, and ETL; presentation is supported by clear charts and reproducible documentation.

## 8. Six-week work plan

| Week | Deliverable |
|---|---|
| 1 | Validate sources and access conditions; finalize asset registry, scope, and database contracts |
| 2 | Implement eligible collectors, raw archives, checkpoints, and import of existing prices |
| 3 | Implement normalization, coin matching, MongoDB-to-PostgreSQL ETL, and daily marts |
| 4 | Build the four dashboard views and provenance navigation |
| 5 | Run correctness, recovery, manual-label, and performance evaluations |
| 6 | Resolve findings; prepare the demonstration, report, and reproducibility instructions |

This schedule is a planning assumption. Team assignments and the official deadline remain TBD.

## 9. Limitations and future work

The selected coins introduce survivorship and selection bias. Selected accounts are not representative of all users; archived communities can be incomplete; memes, irony, spam, and multilingual text limit sentiment quality. Third-party services can change or become unavailable. The dashboard will expose these limits and retain the last eligible successful result when a source fails.

Images, OCR, embeddings, source-grounded summaries, additional communities, and managed object storage are possible extensions after the text pipeline is validated and source conditions permit them. Telegram or Discord will require verified message access rather than an invitation link alone. No extension is necessary to demonstrate the initial SQL/NoSQL integration.
