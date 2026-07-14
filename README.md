# VN-DARU — Vietnam Digital Asset Regulatory Uncertainty Index

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/YOUR-GITHUB-USERNAME/VN-DARU/blob/main/notebooks/VN_DARU_colab.ipynb)
[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.XXXXXXX.svg)](https://doi.org/10.5281/zenodo.XXXXXXX)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Python 3.10+](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/)

Replication code and figures for the paper:

> **Constructing a Regulatory Uncertainty Index for Digital Assets in Vietnam (VN-DARU): A Quantitative Approach Based on News and Policy**
> Vu Quang, Huynh Tan Toi, Tran Thi My Huong (2026)

VN-DARU is a weekly, news-based index of perceived regulatory uncertainty surrounding digital assets in Vietnam (January 2021 – June 2026), built from the GDELT Global Knowledge Graph via BigQuery and validated against Bitcoin realized volatility around Vietnam's 2025–2026 legal transition (Law No. 71/2025/QH15, Resolution No. 222/2025/QH15, Resolution No. 05/2025/NQ-CP, and MoF licensing from 20 January 2026).

## Repository structure

```
VN-DARU/
├── notebooks/
│   └── VN_DARU_colab.ipynb   # Full pipeline (Google Colab): data collection + analysis
├── figures/
│   ├── fig1.png              # Raw counts & standardized index vs. global-legal control
│   └── fig2.png              # Decoupling of VN-DARU from Bitcoin volatility after Law 71/2025
├── requirements.txt          # Python dependencies
├── CITATION.cff              # Citation metadata (GitHub "Cite this repository" button)
├── .zenodo.json              # Zenodo archive metadata
├── LICENSE                   # MIT
└── README.md
```

## What the notebook does

**Part 1 — Data collection and index construction** (requires Google Cloud authentication + Google Drive)
- Queries the public GDELT GKG dataset (`gdelt-bq.gdeltv2.gkg_partitioned`) on BigQuery in monthly batches (with retry logic) to stay within quota.
- Applies a three-layer query logic: (A) crypto-asset themes, (B) a legal/regulatory/uncertainty theme allowlist including GDELT's EPU theme family, (C) Vietnamese digital-asset keywords matched in URL-slug form.
- Builds the main index and four control variants in a single pass, standardizes weekly deduplicated counts to mean = 100, and merges with Bitcoin realized volatility computed from Yahoo Finance daily prices.

**Part 2 — Analysis and validation** (requires only Google Drive + the CSV produced by Part 1)
- Descriptive statistics, correlation matrix (convergent/discriminant validity), top-uncertainty weeks.
- Stationarity tests (ADF, KPSS), OLS with HAC (Newey–West) errors, lagged specifications, Granger causality.
- Structural-break analysis around Law 71/2025 with placebo dates and control-variant checks; exclusion of the documented June 2025 GDELT infrastructure outage.
- Reproduces Figures 1 and 2 of the paper.

## How to run

1. Open the notebook in Google Colab (badge above, or upload `notebooks/VN_DARU_colab.ipynb` to [colab.research.google.com](https://colab.research.google.com)).
2. Install dependencies in the first cell:
   ```
   !pip install -q yfinance google-cloud-bigquery db-dtypes statsmodels
   ```
3. **Part 1 only:** set `PROJECT_ID` to your own Google Cloud project ID (BigQuery API enabled). GDELT is a free public dataset; queries are billed against your project's BigQuery quota.
4. Run Part 1 to generate `VN_DARU_weekly_all_variants.csv` and `VN_DARU_merged_with_RV.csv` in your Google Drive (`MyDrive/VN_DARU/`).
5. Run Part 2 to reproduce all tables and figures reported in the paper.

## Data availability

- **News data:** GDELT Global Knowledge Graph, publicly available via Google BigQuery (`gdelt-bq.gdeltv2.gkg_partitioned`).
- **Market data:** BTC-USD daily prices from Yahoo Finance via `yfinance`.
- Because GDELT is continuously updated and Yahoo Finance data may be revised, re-running the pipeline can yield marginally different counts; the analysis sample and outage-exclusion window are fixed in the code.

## Figures

| Figure 1 | Figure 2 |
|---|---|
| ![Figure 1](figures/fig1.png) | ![Figure 2](figures/fig2.png) |

## How to cite

If you use this code or the VN-DARU index, please cite the paper and the archived code:

```
Vu, Q., Huynh, T. T., & Tran, T. M. H. (2026). Constructing a Regulatory
Uncertainty Index for Digital Assets in Vietnam (VN-DARU): A Quantitative
Approach Based on News and Policy.

Code archive: https://doi.org/10.5281/zenodo.XXXXXXX
```

Citation metadata is also available in [`CITATION.cff`](CITATION.cff) (use GitHub's **"Cite this repository"** button).

## License

Code is released under the [MIT License](LICENSE). Figures © the authors, reproduced here for replication purposes.

## Contact

Corresponding author: Huynh Tan Toi — huynhtantoi.law@gmail.com — ORCID [0009-0008-2542-6645](https://orcid.org/0009-0008-2542-6645)

---

## Hướng dẫn nhanh (tiếng Việt)

Kho lưu trữ này chứa mã nguồn tái lập cho bài báo về chỉ số **VN-DARU** — chỉ số bất định pháp lý tài sản số cho Việt Nam. Notebook `notebooks/VN_DARU_colab.ipynb` gồm 2 phần: **Phần 1** thu thập dữ liệu GDELT qua BigQuery và dựng chỉ số (cần Project ID Google Cloud của bạn); **Phần 2** phân tích, kiểm định và vẽ lại Hình 1–2 từ file CSV do Phần 1 tạo ra. Mở trực tiếp trên Google Colab bằng nút "Open in Colab" ở đầu trang.
