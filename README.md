# GunsNPrices Firearm Datasets

Open reference data about the guns and ammunition sold by US online retailers, published by [GunsNPrices](https://gunsnprices.com), a price comparison site that tracks 40+ US gun and ammo stores.

| Dataset | What's inside | Files |
|---|---|---|
| [Calibers](calibers/) | 293 calibers with type, retailer spellings, listing counts and caliber pairs that stores list together | `calibers.csv`, `calibers.jsonl`, `caliber_pairs.csv` |
| [Models](models/) | 50 brands → 1,192 model families → 4,037 models, with listing counts and current price levels | `models.csv`, `brands.csv` |

Snapshot date: **2026-10-12**. The data comes from the live GunsNPrices catalog of 63,930 gun and ammo listings.

## Also available on

- Hugging Face: [gunsnprices](https://huggingface.co/gunsnprices)
- Kaggle: [gunsnprices](https://www.kaggle.com/gunsnprices)

## Quick start

```python
import pandas as pd

calibers = pd.read_csv("https://raw.githubusercontent.com/gunsnprices/firearm-datasets/main/calibers/calibers.csv")
models = pd.read_csv("https://raw.githubusercontent.com/gunsnprices/firearm-datasets/main/models/models.csv")

print(calibers[calibers["type"] == "handgun"].head())
print(models[models["family"] == "Glock 19"][["model", "listings", "price_min_usd"]])
```

## License

Data is released under [CC BY 4.0](LICENSE). Please credit **GunsNPrices (gunsnprices.com)** with a link.

## Contact

Corrections and requests: app@gunsnprices.com. Live prices: [gunsnprices.com](https://gunsnprices.com).
