# Firearm Models by Brand

A brand → family → model taxonomy of the firearms sold by US online retailers, with listing counts and current price levels for each model. Built by [GunsNPrices](https://gunsnprices.com), a price comparison site for guns and ammo.

**As of 2026-10-12:** 50 brands, 1,192 model families and 4,037 models, from 32,316 firearm listings across 40 US stores.

Example rows:

| brand | family | model | listings | in_stock | price_min_usd | price_median_usd |
|---|---|---|---|---|---|---|
| Glock | Glock 19 | Glock 19 Gen 5 | 137 | 34 | 500.99 | 757.49 |
| Glock | Glock 19 | Glock 19 Gen 3 | 107 | 15 | 498.94 | 679.99 |
| Glock | Glock 19 | Glock 19 V | 12 | 12 | 539.00 | 599.50 |

## Files

### `models.csv`

| Column | Meaning |
|---|---|
| `brand` | Brand, e.g. `Glock`, `Sig Sauer`, `Ruger` |
| `family` | Model family, e.g. `Glock 19`, `Sig Sauer P365` |
| `model` | The most specific model we recognize: generation, size and key configuration, e.g. `Glock 19 Gen 5 MOS` |
| `type` | `handgun`, `rifle`, `shotgun` or `other` |
| `calibers` | Up to 3 most common calibers among the listings, separated by ` \| ` (names match the [calibers dataset](https://huggingface.co/datasets/gunsnprices/firearm-calibers)) |
| `listings` | Product listings matched to this model |
| `in_stock` | Listings in stock at one or more stores on the build date |
| `price_min_usd` | Lowest in-stock price among the listings |
| `price_median_usd` | Median of the lowest in-stock price of each listing |
| `model_url` | GunsNPrices page comparing prices for this model, if one exists |
| `family_url` | GunsNPrices page for the whole family, if one exists |
| `brand_hub_url` | GunsNPrices "All [Brand] Models" guide |

### `brands.csv`

One row per brand: number of families, models, listings and in-stock listings, plus the brand guide URL.

## How it was built

- **Models.** GunsNPrices keeps a hand-built model dictionary for each of these 50 brands: ordered patterns for families, generations and configurations, plus manufacturer part number rules (Glock's PA/UA/PI codes give the generation).
  - The dictionary assigns every firearm listing of a brand to its most specific model.
  - Matching quality was checked with blind reviews: about 3.5% of assignments were judged wrong in the October 2026 audit.
- **Prices.** Prices come from the daily price sync of the 40 stores. A listing can be a bundle, a color variant or a distributor exclusive, so the median is a better guide than the minimum.
- Models with zero listings are not included.

## Limitations

- Only 50 brands so far. Brands without a model dictionary are not in this file.
- US online retail only. Prices are a one-day snapshot in USD, before shipping, tax and FFL transfer fees.
- `listings` reflects how many store products exist, not sales or popularity.

## License and attribution

[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). Credit **GunsNPrices (gunsnprices.com)** with a link when you use or republish this data.

## Updates and contact

- Rebuilt from live catalog data.
- Corrections: app@gunsnprices.com.
- Live prices for every model: [gunsnprices.com](https://gunsnprices.com).
