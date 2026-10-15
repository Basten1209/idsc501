# Presentation sources and image credits

Revision and verification date: **2026-10-15**. This file documents the eight-slide, white-background presentation, including the social-event ideation on slide 3, the prediction pipeline on slide 5 and the application mockup on slide 8. Each slide also contains source references in its speaker notes. It replaces the credits for the earlier 18-slide draft.

## Content and supplied data

| Slides | Evidence | Use and limits |
|---|---|---|
| 1–8 | [Project proposal](../project-proposal.md), [evidence and references](../evidence-and-references.md), [system design](../../architecture/system-design.md) | Proposed scope and implementation decisions; not evidence of an implemented service |
| 2, 4–7 | Local `lecturenote/1_overview.html` and `lecturenote/2_data-collection.html` | Proposal: ideation, collection/preprocessing and DB management. Six-week schedule is not a verified presentation requirement |
| 4, 8 | User-supplied `.context/attachments/ogAuZF/memecoin_price.zip` and `.context/attachments/lezAdu/memecoin_eda.ipynb` | Ten coins, 14,773 price rows. Team collected prices by crawling CMC Historical Data pages and delivered CSVs; crawler source is not supplied |
| 3 | Binance public spot 1-minute klines; original Musk post via FxTwitter; Trump post archived by UCSB | Two selected social-event windows and a subsequent TRUMP pullback. Details below; separate from the project’s CMC daily data |
| 8 | `memecoin_price/Dogecoin.csv` inside the ZIP | 92 daily closes, 2026-07-01–2026-09-30. Price chart and price summary use supplied observations; social content is explicitly illustrative |

The “When we buy…” caption and crying Pepe are comic commentary, not a record of team trades. All four charts are native editable objects with embedded workbook data. The slide 8 workbook is preserved from the preceding deck. Local supplied datasets and lecture files are not redistributed with this deck.

Slide 5 follows the [presentation Mermaid](../../architecture/system-design.md#발표용-전체-흐름), the [prediction contract](../../architecture/prediction-design.md), and the user's whiteboard (`.context/attachments/yYZEUO/IMG_8630.HEIC`). Its editable paths distinguish price/social storage, frozen input construction, OpenAI API inference, stored predictions, and outcome review for future versions. The small app image is a crop of slide 8.

Slide 8 is an editable UI mockup, not an implemented service. The DOGE close is $0.0944721408 on 2026-09-30, and its one-day change is +0.6562627897%; displayed numbers are rounded. The UP prediction, rationale, and topics are illustrative, not model outputs or measured social signals. No accuracy or calibrated confidence is shown. Prospective outcomes are pending.

## Slide 3: social posts and price windows

Verified **2026-10-15**. This slide replaces the long TRUMP daily-history chart. Its question remains whether price and social information together help predict the next UTC close. These intraday anecdotes are motivation, not evidence of causality or successful daily forecasting. Multiple Musk posts occurred during the DOGE window; the chart does not isolate a single post’s effect.

| Event | Verified text and timing | Source |
|---|---|---|
| Elon Musk / DOGE | “Doge”, 2021-02-04 **07:35:35 UTC**. The slide uses the actual text, rather than inventing a “Doge to the moon!” quotation | [Original post](https://x.com/elonmusk/status/1357231313376456708), [FxTwitter JSON](https://api.fxtwitter.com/status/1357231313376456708). The API response returned the post text and timestamp |
| Donald Trump / TRUMP | “I LOVE $TRUMP” is an excerpt. Posted 2025-03-23 **08:33 Pacific / 15:33 UTC** | [Original Truth Social URL](https://truthsocial.com/@realDonaldTrump/posts/114212473050636851), [UCSB American Presidency Project archive](https://www.presidency.ucsb.edu/documents/truth-social-posts-march-23-2025). The archive explicitly uses Pacific time; March 23 is PDT (UTC−7). Direct API content was unavailable |

[Decrypt’s report](https://decrypt.co/311294) gives a conflicting “10:33 AM ET” time. The slide uses the UCSB archived post’s time, rather than mixing that report with exchange data. The two times differ by one hour after timezone conversion.

Prices come from Binance **spot USDT pairs**, not a CMC USD aggregate. [Binance’s data definitions](https://github.com/binance/binance-public-data) identify the OHLC columns and the change from millisecond timestamps to microseconds for spot archives from 2025. Each downloaded file contains 1,440 unique one-minute bars; both official ZIP checksums were verified.

| Series | Download | ZIP SHA-256 |
|---|---|---|
| DOGEUSDT, 2021-02-04 | [1-minute ZIP](https://data.binance.vision/data/spot/daily/klines/DOGEUSDT/1m/DOGEUSDT-1m-2021-02-04.zip) | `d8738f696c09d495ef3b7c75750577d60ef4d59d22e76cf4f074f62fa5262795` |
| TRUMPUSDT, 2025-03-23 | [1-minute ZIP](https://data.binance.vision/data/spot/daily/klines/TRUMPUSDT/1m/TRUMPUSDT-1m-2025-03-23.zip) | `a4f5bd5162bc8855c240538f8b9a08478e333075238a5d0bef133c3d4d2e1985` |

The screenshots are visual evidence of the posts, not evidence of capture-time availability in a predictive pipeline. Their visible reaction counts are not used in the event-return calculation or model inputs. Original profile and verification colors are preserved, while the slide background remains white.

The event timestamp is rounded down to its minute. Define the baseline as the close of the last bar before that minute. The reported return is **the last close in the next 60 one-minute bars / baseline − 1**. These are minute-aligned event windows, not measurements from the exact post second. The left-hand plots include 30 preceding bars and those 60 following bars. X values show minute-end offsets from the event minute; the post marker is zero. Y values show change from the baseline, with separate scales for DOGE and TRUMP. Chart return fractions are rounded to 10 decimal places for Excel portability; headline percentages use the original calculation.

| Series | Baseline bar open / close, UTC | Final bar open / close, UTC | Calculated change | Slide |
|---|---|---|---|---|
| DOGEUSDT | 2021-02-04 07:34 / 0.0410273 USDT | 2021-02-04 08:34 / 0.0528923 USDT | +28.9197680569% | **+28.9% in 60 min** |
| TRUMPUSDT | 2025-03-23 15:32 / 10.98 USDT | 2025-03-23 16:32 / 11.97 USDT | +9.0163934426% | **+9.0% in 60 min** |

The right-hand chart contains **61 actual TRUMPUSDT minute closes**, from bar-open timestamps **16:09 through 17:09 UTC on 2025-03-23**. The first close is **12.34 USDT** and the last is **11.86 USDT**. It illustrates a selected pullback after the initial spike. “When we buy…” does not identify an executed trade or assert that prices always fall after retail purchases.

Binance and Truth Social are **presentation evidence only**. The project’s collection scope remains CMC historical-page crawling, selected X accounts through FxTwitter, and conditional Reddit access through Arctic Shift. The Trump event does not silently add TRUMP to the five forecast assets or add Truth Social ingestion.

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

- [POSTECH official logo/UI guide](https://postech.ac.kr/kor/university-introduction/logo.do): POSTECH RED, Pantone 215C, RGB 166/25/85, **#A61955**. Applied only to important phrases and event markers; every slide background is white.
- [shadcn/ui theming](https://ui.shadcn.com/docs/theming): restrained semantic colors and simple components informed the presentation layout.
- [Geist Design System](https://vercel.com/geist/introduction) and [Geist fonts](https://vercel.com/font): typography, spacing, monochrome hierarchy and light borders. These are design references; the PPT is not an official POSTECH, shadcn/ui or Vercel template.

## Web-sourced visual assets

| Slide | Asset | Source and downloaded file |
|---|---|---|
| 1 | Black-and-white Doge illustration | [Icons8 Doge collection](https://icons8.com/icons/set/doge), [image](https://img.icons8.com/ios7/1200/doge.jpg). Visible attribution on the cover and in notes |
| 3 | Elon Musk’s “Doge” reply screenshot | [Fabian Dablander original capture collage](https://fabiandablander.com/assets/img/Doge-2.png), [Know Your Meme mirror and attribution](https://knowyourmeme.com/photos/2933658-meme-coins), [downloaded PNG](https://i.kym-cdn.com/photos/images/original/002/933/658/83c.png). The lower-left reply is cropped as a native PPT image; account, date, reply context, full text and reaction row remain visible. Text independently checked against FxTwitter post `1357231313376456708` |
| 3 | Trump’s “I LOVE $TRUMP” screenshot | [The Block, 2025-03-23](https://www.theblock.co/post/347641/trump-boosts-trump-memecoin-by-400-million-with-truth-social-post-sending-price-careening), [original PNG](https://www.tbstat.com/wp/uploads/2025/03/Screenshot-2025-03-23-at-1.56.42%E2%80%AFPM.png). Entire screenshot retained. The image shows 11:33 AM without a timezone; chart alignment uses the independently verified UCSB archive time |
| 3 | Black-and-white crying Pepe | [UniqueDesignOK on Redbubble](https://www.redbubble.com/i/poster/Frog-funny-meme-pepe-crying-reaction-sad-icon-by-UniqueDesignOK/152477286.LVTDI), [source image](https://ih1.redbubble.net/image.5248600290.7286/fposter,small,wall_texture,square_product,600x600.u2.jpg). Cropped product setting, unchanged artwork. Source retained in notes; no claim of an open license |
| 4, 5 | CoinMarketCap icon | [Simple Icons](https://github.com/simple-icons/simple-icons), [original SVG](https://raw.githubusercontent.com/simple-icons/simple-icons/develop/icons/coinmarketcap.svg) |
| 4, 5 | X icon | [Simple Icons](https://github.com/simple-icons/simple-icons), [original SVG](https://raw.githubusercontent.com/simple-icons/simple-icons/develop/icons/x.svg) |
| 4, 5 | Reddit icon | [Simple Icons](https://github.com/simple-icons/simple-icons), [original SVG](https://raw.githubusercontent.com/simple-icons/simple-icons/develop/icons/reddit.svg) |
| 8 (preview on 5) | Doge Windows 10 illustration | [Icons8](https://icons8.com/icon/35007/doge), [image](https://img.icons8.com/win10/1200/doge.jpg). Unmodified black-and-white layout sample, not a collected post |
| 7 | MongoDB icon | [Simple Icons](https://github.com/simple-icons/simple-icons), [original SVG](https://raw.githubusercontent.com/simple-icons/simple-icons/develop/icons/mongodb.svg) |
| 7 | PostgreSQL icon | [Simple Icons](https://github.com/simple-icons/simple-icons), [original SVG](https://raw.githubusercontent.com/simple-icons/simple-icons/develop/icons/postgresql.svg) |

Assets identify the topic and services and do not imply endorsement. Simple Icons provides its collection under [CC0](https://github.com/simple-icons/simple-icons/blob/develop/LICENSE.md); brand marks remain subject to their respective owners' rights. Icons8 illustrations have visible and speaker-note attribution. Service icons intentionally repeat between collection and pipeline. The slide 5 miniature repeats the full application mockup from slide 8. No generated photographs, fabricated performance series, or reconstructed social UI are used. Slide 3 uses historical screenshot images, accompanied by enlarged typeset excerpts below each image. These were sourced from published captures; the project did not capture a new live X or Truth Social browser view. The meme illustrates a reaction and is not collected project data.
