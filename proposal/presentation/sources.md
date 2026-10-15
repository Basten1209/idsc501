# Presentation sources and image credits

Presentation prepared on **2026-10-15**. The project evidence baseline is **2026-10-14**, recorded in commit `7aa8ba27b679cc058cec79e40544a6af5a1daa85`. URLs for specific claims and images also appear in the relevant slide's speaker notes.

## Project evidence

- [Project proposal](../project-proposal.md), [evidence and references](../evidence-and-references.md), and [architecture](../../architecture/README.md).
- Team-supplied `.context/attachments/ogAuZF/memecoin_price.zip` and `.context/attachments/lezAdu/memecoin_eda.ipynb`. The team confirmed on 2026-10-15 that the prices were collected by crawling CoinMarketCap Historical Data pages and delivered as CSV files. CSV import describes downstream ingestion, not the original collection method. These local inputs are not redistributed here.
- Slide 2 uses all 628 supplied TRUMP daily closes, from 2025-01-18 through 2026-10-07. Highest daily close: **$45.5030010328**, on 2025-01-19. Last supplied close: **$1.8772949946**. The intraday high of **$75.3518198764** is a different measure and is not the plotted peak. The green and red segments share the peak observation.
- Slide 12 uses supplied DOGE daily closes during July–September 2026. Social activity is explicitly unavailable. No synthetic social activity appears in the deck.
- The 10-coin price input contains **14,773 rows**. Source availability checks, the **31,811-row** MemeChain social metadata table, and course requirements are documented in the evidence file. Source spot checks do not establish continuous availability.

## Primary references

| Topic | Reference | Slides |
|---|---|---|
| Link discovery | [MemeChain dataset](https://zenodo.org/records/18246856), [paper](https://arxiv.org/html/2601.22185v1) | 7 |
| X collection | [FxEmbed API](https://docs.fxembed.com/api/introduction/) | 6, 17 |
| Reddit collection | [Arctic Shift](https://github.com/ArthurHeitmann/arctic_shift), [Reddit Responsible Builder Policy](https://support.reddithelp.com/hc/en-us/articles/42728983564564-Responsible-Builder-Policy) | 6, 17 |
| Relational JSON support | [PostgreSQL JSON types](https://www.postgresql.org/docs/current/datatype-json.html) | 9 |
| Document modeling | [MongoDB data modeling](https://www.mongodb.com/docs/manual/data-modeling/), [JSON and BSON](https://www.mongodb.com/resources/basics/json-and-bson) | 9, 16 |
| Sentiment baseline | [VADER](https://github.com/cjhutto/vaderSentiment) | 14, 17 |
| Visual principles | [shadcn/ui](https://ui.shadcn.com/docs), [theming](https://ui.shadcn.com/docs/theming) | 1, 12 |
| Typography and color | [Geist introduction](https://vercel.com/geist/introduction), [typography](https://vercel.com/geist/typography), [colors](https://vercel.com/geist/colors), [fonts](https://vercel.com/font) | Throughout |

The design references and image URLs were consulted on 2026-10-15. The technical source checks are inherited from the dated project evidence. The presentation preserves the conditional Reddit plan and the distinction between selected X accounts and platform-wide activity.

## Images

Images are embedded in the PPTX. The following hashes identify the downloaded source bytes, before any PowerPoint format conversion. Source access does not imply a general redistribution license. Credits identify the original publisher or trademark holder and do not imply endorsement.

Kabosu photograph: Atsuko Sato, as reproduced by KAI-YOU. TRUMP promotional artwork: GetTrumpMemes. Coin symbols: the respective projects, distributed by CoinMarketCap. PostgreSQL elephant: PostgreSQL Global Development Group. MongoDB wordmark: MongoDB, served by its partner Globant.

| Slide | Asset | Source page | Downloaded image | SHA-256 |
|---|---|---|---|---|
| 1 | `doge-photo.jpg` | [Publisher](https://kai-you.net/article/89682) | [Image](https://img.kai-you.net/storage/2024/05/09dbb0db-79b4-4396-b9fc-53ace8009748/jpg/origin.jpg) | `6ad8456543eae61ab29e8ea51c9046ab76249de9f382d1ce4e88af7dd22949f9` |
| 3 | `trump-banner.webp` | [Publisher](https://gettrumpmemes.com/) | [Image](https://cdn.prod.website-files.com/69cde7380ef3080411ab6b07/69cdedf4bdbcdf30ccd01e55_New-tump-meme-1200x630.webp) | `e615fc4f8287d0d85c7a5eebb09c1046d7a2c5bcc7e3d75e2bed5ca865111353` |
| 9 | `postgres.png` | [Publisher](https://www.postgresql.org/about/press/) | [Image](https://www.postgresql.org/media/img/about/press/elephant.png) | `ff92fb5d12a7fa6b23c2eb403d2edf4fbc58ced802021e1f02c3583cb70957be` |
| 9 | `mongo.png` | [Publisher](https://www.globant.com/about/partners) | [Image](https://statics.globant.com/production/public/2021-01/MongoDB_Logo_FullColorBlack_RGB-4td3yuxzjs.png) | `0d4f955a0519fc4a40488b3888866110bd9eb4d78f31e00109250c1fd3958f3d` |
| 5 | `doge.png` | [Publisher](https://coinmarketcap.com/currencies/dogecoin/) | [Image](https://s2.coinmarketcap.com/static/img/coins/128x128/74.png) | `18da6a7739ba52b92b631e1e39b3a45e7e7b2e27d7d6dbaf14bbefce2dd393f2` |
| 5 | `shib.png` | [Publisher](https://coinmarketcap.com/currencies/shiba-inu/) | [Image](https://s2.coinmarketcap.com/static/img/coins/128x128/5994.png) | `46736c95de23a9e2a1c29d5e66931b3a0ec1727e6eb4011d2072624b73aead12` |
| 5 | `pepe.png` | [Publisher](https://coinmarketcap.com/currencies/pepe/) | [Image](https://s2.coinmarketcap.com/static/img/coins/128x128/24478.png) | `9ba2544ee18e4929806096072204d7b92e19d5bd0c23efa06510e8018c4c66fe` |
| 5 | `m.png` | [Publisher](https://coinmarketcap.com/currencies/memecore/) | [Image](https://s2.coinmarketcap.com/static/img/coins/128x128/35491.png) | `3f6e33518b7bc11433420bf5b9d614a04d999efda66dd961b3c3badeebd754c5` |
| 5 | `pengu.png` | [Publisher](https://coinmarketcap.com/currencies/pudgy-penguins/) | [Image](https://s2.coinmarketcap.com/static/img/coins/128x128/34466.png) | `26cae9a98f2a88aacd4d77adefcda1468e2631c075516e2ff51d5a3ab0958e8c` |
| 5 | `trump.png` | [Publisher](https://coinmarketcap.com/currencies/official-trump/) | [Image](https://s2.coinmarketcap.com/static/img/coins/128x128/35336.png) | `ebb222b7ff62c75267eba92c2d58dfd6f8f0e3802814ea481c81dc1d07705275` |
| 5 | `spx.png` | [Publisher](https://coinmarketcap.com/currencies/spx6900/) | [Image](https://s2.coinmarketcap.com/static/img/coins/128x128/28081.png) | `2355c06f5b5a4e089b71139c23118c6b525335addccc1c172f4dacde23febbdd` |
| 5 | `bonk.png` | [Publisher](https://coinmarketcap.com/currencies/bonk1/) | [Image](https://s2.coinmarketcap.com/static/img/coins/128x128/23095.png) | `ae513c3d47bade4c49ac2a5548c0cb8724fa5d17c5b624277f424819d60508bd` |
| 5 | `floki.png` | [Publisher](https://coinmarketcap.com/currencies/floki-inu/) | [Image](https://s2.coinmarketcap.com/static/img/coins/128x128/10804.png) | `1f0ab94ff6b85552b1a7d38b7f8cf7f0cf628154575c59c59d7b3ed911045d26` |
| 5 | `bian.png` | [Publisher](https://coinmarketcap.com/currencies/bianrensheng/) | [Image](https://s2.coinmarketcap.com/static/img/coins/128x128/38590.png) | `6a380b12f12d471811e49a4b73c03ed9ba2c535d1680c1330469ef74941904af` |

## Fonts

Geist Regular, Geist Bold, and Geist Mono Regular came from the [official Geist font repository](https://github.com/vercel/geist-font), under its SIL Open Font License. The PPTX references these fonts but does not embed font files. Install them for consistent editing on another machine, or use the PDF for viewing.
