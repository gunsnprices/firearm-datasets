# Firearm & Ammunition Calibers

A clean dictionary of the firearm calibers (cartridges) that are actually sold by US online retailers, with the spellings retailers use for each one. Built and maintained by [GunsNPrices](https://gunsnprices.com), a price comparison site for guns and ammo.

Use it to normalize messy caliber fields in product feeds, to group listings by caliber, or as a reference list of cartridges in current US retail.

**As of 2026-10-12:** 293 calibers, 326 retailer spellings, 34 caliber pairs, built from 63,930 gun and ammo listings across 40 US stores.

## Files

### `calibers.csv` (also `calibers.jsonl`)

| Column | Meaning |
|---|---|
| `id` | Stable lowercase id, e.g. `9x19mmparabellum` |
| `name` | Display name, e.g. `9×19mm Parabellum`, `.30-06 Springfield` |
| `type` | `handgun`, `rifle`, `rimfire`, `shotgun`, `airgun` or `muzzleloader` |
| `aliases` | Spellings retailers use for this caliber, separated by ` \| ` (a list in the JSONL file) |
| `products_guns` | Firearm listings chambered in this caliber on GunsNPrices |
| `products_ammo` | Ammunition listings in this caliber |
| `products_in_stock` | Listings in stock at one or more stores on the build date |
| `ammo_url` | GunsNPrices page comparing ammo prices for this caliber, if one exists |
| `guns_url` | GunsNPrices page comparing gun prices for this caliber, if one exists |
| `site_name` | The exact value used in GunsNPrices filters |

### `caliber_pairs.csv`

Calibers that retailers list together on one product, such as rifles marked `.223 Rem/5.56×45mm NATO` or revolvers that take both `.357 Mag` and `.38 Special`. `listings` is the number of products with that combined marking.

| caliber_a | caliber_b | listings |
|---|---|---|
| .223 Rem | 5.56×45mm NATO | 1,141 |
| .308 Win | 7.62x51mm NATO | 990 |
| .357 Mag | .38 S&W Special | 129 |
| .22 LR | .22 WMR | 81 |
| .410 Bore | .45 Colt | 67 |

A pair means "listed together", not "safe to interchange". Always follow the markings on the firearm and the ammunition maker's guidance.

## How it was built

1. **Calibers** are the values used in the caliber filter of gunsnprices.com.
   - Junk values like `MULTI` or `All Calibers` are dropped, and so are bare bore diameters such as `.355` or `6mm`.
   - Pressure variants (`+P`) are folded into the base caliber.
   - Combined markings are split into single calibers and kept in `caliber_pairs.csv`.
2. **Aliases** come from about 6,000 raw caliber strings seen in retailer data that our team mapped by hand. For this public list only high-confidence spellings were kept, and 3,374 were rejected. A spelling is kept only when:
   - it appears at two or more stores;
   - its numbers agree with the caliber (`556 NATO` → 5.56×45mm, `30-06` → .30-06 Springfield);
   - it contains no barrel lengths, round counts, loads or model names;
   - it does not name a different cartridge or maker (`.22 Long` is not `.22 LR`, `223 Win` is not `.223 Rem`);
   - a bare number is kept only when one caliber clearly dominates it (`9mm` → 9×19mm Parabellum, but not `.38` or `300`).
3. **Type** comes from the store categories where the guns and ammo are listed, with a few manual fixes.

## Example

```python
import pandas as pd, re

cal = pd.read_csv("hf://datasets/gunsnprices/firearm-calibers/calibers.csv")

def key(s):
    s = s.lower().replace("×", "x")
    s = re.sub(r"(?<=\d)\.(?=\d)", "#", s)
    return re.sub(r"[^a-z0-9#]+", "", s).replace("#", ".")

lookup = {}
for _, r in cal.iterrows():
    for v in [r["name"], r["site_name"]] + (r["aliases"].split(" | ") if isinstance(r["aliases"], str) else []):
        lookup[key(v)] = r["name"]

print(lookup[key("556 NATO")])         # 5.56×45mm NATO
print(lookup[key(".45 Long Colt")])    # .45 Colt
```

## Limitations

- US retail only. Military and obsolete cartridges that aren't sold online in the US are mostly missing.
- Counts are a snapshot of one day. They show what is listed, not what is popular.
- The alias list favors precision over coverage, so many calibers have no aliases yet.

## License and attribution

[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). Credit **GunsNPrices (gunsnprices.com)** with a link when you use or republish this data.

## Updates and contact

- Rebuilt from live catalog data when the dictionary changes.
- Corrections and missing spellings: app@gunsnprices.com.
- Live price comparison for every caliber: [gunsnprices.com](https://gunsnprices.com).
