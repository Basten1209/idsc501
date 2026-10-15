# Presentation sources and image credits

Revision and verification date: **2026-10-15**. This file documents the eight-slide, white-background presentation, including the prediction pipeline on slide 5 and the application mockup on slide 8. Each slide also contains source references in its speaker notes. It replaces the credits for the earlier 18-slide draft.

## Content and supplied data

| Slides | Evidence | Use and limits |
|---|---|---|
| 1–8 | [Project proposal](../project-proposal.md), [evidence and references](../evidence-and-references.md), [system design](../../architecture/system-design.md) | Proposed scope and implementation decisions; not evidence of an implemented service |
| 2, 4–7 | Local `lecturenote/1_overview.html` and `lecturenote/2_data-collection.html` | Proposal: ideation, collection/preprocessing and DB management. Six-week schedule is not a verified presentation requirement |
| 3, 4, 8 | User-supplied `.context/attachments/ogAuZF/memecoin_price.zip` and `.context/attachments/lezAdu/memecoin_eda.ipynb` | Ten coins, 14,773 price rows. Team collected prices by crawling CMC Historical Data pages and delivered CSVs; crawler source is not supplied |
| 3 | `memecoin_price/OFFICIAL TRUMP.csv` inside the ZIP | 628 daily closes, 2025-01-18–2026-10-07; maximum daily close $45.5030010328, last supplied close $1.8772949946. No claim about intraday ATH or live price |
| 8 | `memecoin_price/Dogecoin.csv` inside the ZIP | 92 daily closes, 2026-07-01–2026-09-30. Price chart and price summary use supplied observations; social content is explicitly illustrative |

The TRUMP labels “what you want / after you buy” are humorous annotations, not actual trade entries or returns. Charts preserve the supplied time series and have editable native chart data with embedded workbook snapshots. Local datasets and lecture files are not redistributed with this deck.

Slide 5 follows the [presentation Mermaid](../../architecture/system-design.md#발표용-전체-흐름), the [prediction contract](../../architecture/prediction-design.md), and the user's whiteboard (`.context/attachments/yYZEUO/IMG_8630.HEIC`). Its editable paths distinguish price/social storage, frozen input construction, OpenAI API inference, stored predictions, and outcome review for future versions. The small app image is a crop of slide 8.

Slide 8 is an editable UI mockup, not an implemented service. The DOGE close is $0.0944721408 on 2026-09-30, and its one-day change is +0.6562627897%; displayed numbers are rounded. The UP prediction, rationale, and topics are illustrative, not model outputs or measured social signals. No accuracy or calibrated confidence is shown. Prospective outcomes are pending.

## Prediction references

Reviewed 2026-10-15. These sources support interface or evaluation choices, not financial prediction performance.

- [OpenAI Structured Outputs](https://developers.openai.com/api/docs/guides/structured-outputs): schema-constrained responses and validation boundaries.
- [OpenAI evaluation best practices](https://developers.openai.com/api/docs/guides/evaluation-best-practices): task-specific evaluation and iteration.
- [Forecasting: Principles and Practice](https://otexts.com/fpp3/toolbox.html): chronological forecasting evaluation.

## Collection and database references

- [FxEmbed API overview](https://docs.fxembed.com/api/introduction/): JSON API and cursor pagination. Slide 4 proposes collecting selected account timelines; it does not claim comprehensive X coverage or guaranteed availability.
- [Arctic Shift API documentation](https://github.com/ArthurHeitmann/arctic_shift/blob/master/api/README.md) and [project README](https://github.com/ArthurHeitmann/arctic_shift): community/time search and archive access. Slide 4 retains the proposal's conditional activation.
- [Reddit Responsible Builder Policy](https://support.reddithelp.com/hc/en-us/articles/42728983564564-Responsible-Builder-Policy): access/use conditions informing the conditional Reddit plan; archive access does not by itself establish permission for every use.
- [MemeChain Zenodo record](https://zenodo.org/records/18246856): ideation reference for finding relevant sites and channels, **not a runtime data source, imported dataset or stored snapshot**. This distinction is stated in slide 4 notes.
- [PostgreSQL JSON types](https://www.postgresql.org/docs/current/datatype-json.html) and [MongoDB data modeling](https://www.mongodb.com/docs/manual/data-modeling/): support for the storage discussion in slide 7. MongoDB is a design/educational choice, not a requirement imposed by text or images and not an unconditional performance claim.
- [Detailed collection design](../../architecture/ingestion-and-archiving.md) and [data model](../../architecture/data-model.md): planned daily collection, seven-day overlap, provenance, observations and aggregate joins.

## Design references

- [POSTECH official logo/UI guide](https://postech.ac.kr/kor/university-introduction/logo.do): POSTECH RED, Pantone 215C, RGB 166/25/85, **#A61955**. Applied only to important phrases and the short rising TRUMP segment; every slide background is white.
- [shadcn/ui theming](https://ui.shadcn.com/docs/theming): restrained semantic colors and simple components informed the presentation layout.
- [Geist Design System](https://vercel.com/geist/introduction) and [Geist fonts](https://vercel.com/font): typography, spacing, monochrome hierarchy and light borders. These are design references; the PPT is not an official POSTECH, shadcn/ui or Vercel template.

## Web-sourced visual assets

| Slide | Asset | Source and downloaded file |
|---|---|---|
| 1 | Black-and-white Doge illustration | [Icons8 Doge collection](https://icons8.com/icons/set/doge), [image](https://img.icons8.com/ios7/1200/doge.jpg). Visible attribution on the cover and in notes |
| 4, 5 | CoinMarketCap icon | [Simple Icons](https://github.com/simple-icons/simple-icons), [original SVG](https://raw.githubusercontent.com/simple-icons/simple-icons/develop/icons/coinmarketcap.svg) |
| 4, 5 | X icon | [Simple Icons](https://github.com/simple-icons/simple-icons), [original SVG](https://raw.githubusercontent.com/simple-icons/simple-icons/develop/icons/x.svg) |
| 4, 5 | Reddit icon | [Simple Icons](https://github.com/simple-icons/simple-icons), [original SVG](https://raw.githubusercontent.com/simple-icons/simple-icons/develop/icons/reddit.svg) |
| 8 (preview on 5) | Doge Windows 10 illustration | [Icons8](https://icons8.com/icon/35007/doge), [image](https://img.icons8.com/win10/1200/doge.jpg). Unmodified black-and-white layout sample, not a collected post |
| 7 | MongoDB icon | [Simple Icons](https://github.com/simple-icons/simple-icons), [original SVG](https://raw.githubusercontent.com/simple-icons/simple-icons/develop/icons/mongodb.svg) |
| 7 | PostgreSQL icon | [Simple Icons](https://github.com/simple-icons/simple-icons), [original SVG](https://raw.githubusercontent.com/simple-icons/simple-icons/develop/icons/postgresql.svg) |

Assets identify the topic and services and do not imply endorsement. Simple Icons provides its collection under [CC0](https://github.com/simple-icons/simple-icons/blob/develop/LICENSE.md); brand marks remain subject to their respective owners' rights. Icons8 illustrations have visible and speaker-note attribution. Service icons intentionally repeat between collection and pipeline. The slide 5 miniature repeats the full application mockup from slide 8. No generated photographs, fabricated performance series, or purported real social screenshots are used.
