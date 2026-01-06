# College Football Spread Modeling

Machine learning models that predict college football spread coverage using 10 seasons of data (2013-2022). XGBoost, Decision Trees, and GLMnet achieved consistent profitability, with scores exceeding 50 units on alternate spread lines over a 3-season test period.

## Key Findings

- **Best Models**: XGBoost (42.7 mean score) and Decision Trees (53.5 mean score) at +13 alternate line
- **Critical Features**: Defensive passing metrics (explosiveness, success rate) dominated predictions
- **Most Profitable**: +13 alternate spread showed strongest returns (~3.8x payout multiplier)
- **Real-World Test**: Models validated on unseen 2020-2022 seasons after 2013-2019 training

## Quick Start

### Data Setup

The data files are **not included in this repository** due to size. Download them separately:

**Download Link:** [Google Drive - CFB Data Files](https://drive.google.com/drive/folders/1BUvSptqQwjpl0CBwl6-v7GajRjZYEP_J?usp=drive_link)

Place both CSVs in the `Data/` folder:
```
Data/
├── 07_21_2024_CFD_attributes.csv
└── 08_18_2024_CFD_attributes_last5.csv
```

**Data Source:** [College Football Data API](https://collegefootballdata.com/)

### Requirements
- R 4.x+
- Packages: `tidyverse`, `caret`, `glmnet`, `rpart`, `rpart.plot`, `xgboost`, `kernlab`, `fields`

### Data Setup
Place these CSVs in `Data/` folder:
- `07_21_2024_CFD_attributes.csv`
- `08_18_2024_CFD_attributes_last5.csv`

### Run Analysis
1. Open `Code/Final Project Github.Rmd`
2. Set `alt_line` parameter (e.g., 13, 7, 3, -7)
3. Knit or run chunks sequentially
4. Results generate automatically with visualizations and performance tables

### What is `alt_line`?
The alternate spread adjusts the standard betting line:
- Standard: Florida -17.5 vs FSU
- `alt_line = +13`: Florida -30.5 (harder to cover, higher payout ~3.8x)
- `alt_line = -7`: Florida -10.5 (easier to cover, lower payout ~0.4x)

All model training, odds calculations, and scores update based on this single parameter.

## Models Evaluated

11 approaches tested: GLM, GLMnet, Lasso, Ridge, LDA, QDA, KNN, SVM (Linear), Decision Tree, XGBoost, PLS

**Top Performers**: XGBoost, Decision Tree, GLMnet consistently profitable across spread lines.

## Outputs

- Model performance tables (score comparisons across thresholds)
- Visualizations: correlation heatmaps, feature importance, distribution plots
- Optional CSV export: `Model Scores/model_performance_score_{alt_line}.csv`

## Repository Structure

```
├── Code/
│   ├── Final Project Github.Rmd              # Main analysis
│   └── final_project_results_analysis.Rmd    # Supplemental analysis of model scores
├── Data/                                      # CSV files (not in repo)
├── Final Documents/
│   ├── STA4241 Final Project Presentation.pdf
│   └── STA4241 Final Project Write Up.pdf    # Full methodology & details
├── README.md
└── RESULTS.md                                 # Performance summary
```

## Results

See **[RESULTS.md](RESULTS.md)** for detailed performance metrics and feature importance.

**Quick Summary** (alt_line = +13):
- Decision Tree: 53.5 units (best threshold: 0.45)
- XGBoost: 42.7 units (best threshold: 0.50)
- GLMnet: 49.8 units (best threshold: 0.40)

## Deep Dive

For comprehensive methodology, interpretation, and statistical details:
- **Full Write-Up**: `Final Documents/STA4241 Final Project Write Up.pdf`
- **Presentation**: `Final Documents/STA4241 Final Project Presentation.pdf`

## Troubleshooting

**Data not found?** Ensure CSVs are in `Data/` folder with exact filenames.

**Out of memory?** Reduce cross-validation repeats or comment out slower models (SVM, QDA).

**Long runtime?** Expected 15-30 minutes. Reduce `tuneLength` or CV folds to speed up.

## Disclaimer

Educational and research purposes only. Sports betting involves risk. Past performance does not guarantee future results.