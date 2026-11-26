# Data

The notebook expects two CSV files in this folder. They are not included in the repository.

| File | Content | Key columns |
|---|---|---|
| `company_info.csv` | One row per listed company | `symbol`, `type`, `exchange`, `organ_name_x`, `icb_name2` (industry) |
| `financial_statement.csv` | One row per company per year: balance sheet, income statement and cash flow items | `mã_ck` (ticker), `sàn` (exchange), `năm` (year), plus statement items in Vietnamese, e.g. `Tổng cộng tài sản`, `Lợi nhuận sau thuế TNDN` |

Both files were collected with the open-source [vnstock](https://github.com/thinh-vu/vnstock) Python library for companies on HOSE, HNX and UPCoM. The column names follow vnstock's Vietnamese output at the time of collection (early 2025); newer versions of vnstock may name some items differently.
