# Results Summary

Machine learning models successfully predict college football spread coverage with consistent profitability. XGBoost and Decision Trees achieved the strongest performance across multiple alternate spread lines, with defensive passing metrics emerging as the most predictive features.

**For detailed methodology and interpretation, see `Final Documents/STA4241 Final Project Write Up.pdf`**

---

## Dataset Overview

- **Period**: 2013–2022, Weeks 6–15
- **Observations**: ~6,000 team-week combinations, ~80 differential features
- **Train/Test**: 2013–2019 training, 2020–2022 testing
- **Features**: 48 after correlation filtering (|r| > 0.8 cutoff)
- **Target**: Away team covering alternate spread (binary: 1 = cover, 0 = not)

## Evaluation Metric

**Score = (score_multiplier × wins) + (-1 × losses)**

- **score_multiplier**: Betting odds from parametric bootstrap (6% house edge)
- **wins**: Correct predictions when betting (predicted 1, actual 1)
- **losses**: Incorrect predictions when betting (predicted 1, actual 0)
- **Thresholds**: Tested 0.00 to 1.00 by 0.05 increments to optimize selectivity

Directly simulates betting profit/loss in units.

---

## Performance by Alternate Line

### Alternate Line: +13 (Most Profitable)
**Score Multiplier**: 3.8 (90% CI: [3.6, 4.0])

| Model | Mean Score | CI Range | Status |
|-------|------------|----------|--------|
| **Decision Tree** | **53.5** | [36.4, 73.5] | ✅ Best |
| **GLM** | **51.2** | [20.2, 82.5] | ✅ Strong |
| **GLMnet** | 49.8 | [39.2, 71.5] | ✅ Strong |
| **XGBoost** | 42.7 | [32.4, 78.1] | ✅ Strong |
| KNN | 39.5 | [32.8, 46.3] | ✅ Moderate |
| Others | 0.0–47.2 | — | ⚠️ Variable |

**Insight**: Most models profitable; ensemble methods excel. Decision Tree achieved highest consistency.

---

### Alternate Line: +7 (Balanced)
**Score Multiplier**: 2.0 (90% CI: [1.9, 2.1])

| Model | Mean Score | CI Range | Status |
|-------|------------|----------|--------|
| **GLMnet** | **55.3** | [47.0, 64.4] | ✅ Best |
| **XGBoost** | **45.2** | [30.7, 61.3] | ✅ Strong |
| GLM | 38.0 | [32.1, 44.6] | ✅ Strong |
| Lasso | 34.5 | [29.9, 41.6] | ✅ Strong |
| LDA | 39.0 | [33.1, 45.6] | ✅ Strong |
| Others | 0.0–26.6 | — | ⚠️ Variable |

**Insight**: GLMnet dominated; traditional linear models competitive at this spread.

---

### Alternate Line: +3 (Tight Margins)
**Score Multiplier**: 1.3 (90% CI: [1.23, 1.34])

| Model | Mean Score | CI Range | Status |
|-------|------------|----------|--------|
| **XGBoost** | **18.4** | [6.6, 29.4] | ✅ Best |
| **Decision Tree** | 14.0 | [12.4, 15.9] | ✅ Consistent |
| Ridge | 11.0 | [9.4, 12.5] | ✅ Moderate |
| GLMnet | 10.6 | [8.9, 12.2] | ✅ Moderate |
| Others | 0.0–9.7 | — | ⚠️ Weak |

**Insight**: Low multiplier compresses profit; XGBoost and Decision Tree maintain edge.

---

### Alternate Line: -7 (Generally Unprofitable)
**Score Multiplier**: 0.43 (90% CI: [0.41, 0.45])

| Model | Mean Score | Status |
|-------|------------|--------|
| PLS | 18.1 | ⚠️ Only viable option |
| XGBoost | 11.3 | ⚠️ Marginal |
| Others | 0.0–6.8 | ❌ Not profitable |

**Insight**: Very low multiplier makes profitability difficult; most models optimally chose not to bet.

---

## Overall Model Rankings

| Rank | Model | Best At | Why It Works |
|------|-------|---------|--------------|
| 1 | **XGBoost** | All positive lines | Handles non-linearity; robust feature interactions |
| 2 | **Decision Tree** | +13, +3 | Interpretable; strong at wider spreads |
| 3 | **GLMnet** | +7 | Regularization; consistent lower bounds |
| 4 | **GLM** | +13, +7 | Solid baseline; competitive at wider spreads |

**Weak Performers**: SVM (Linear), QDA, Ridge struggled due to strong assumptions or inability to capture complex patterns.

---

## Feature Importance

Top 10 shared features between Decision Tree and XGBoost (alt_line = +13):

| Feature | Category | Interpretation |
|---------|----------|----------------|
| `defense_passing_plays_explosiveness_diff` | Defensive | Limiting big passing plays |
| `defense_passing_plays_success_rate_diff` | Defensive | Consistent passing defense |
| `defense_second_level_yards_diff` | Defensive | Linebacker/safety run defense |
| `defense_rushing_plays_rate_diff` | Defensive | Run defense frequency |
| `defense_power_success_diff` | Defensive | Goal-line/short-yardage stops |
| `defense_line_yards_total_diff` | Defensive | Defensive line run stopping |
| `offense_line_yards_total_diff` | Offensive | Offensive line run blocking |
| `offense_stuff_rate_diff` | Offensive | Avoiding negative runs |
| `defense_passing_downs_rate_diff` | Defensive | Third-down defense |
| `attendance` | Situational | Home crowd effect |

### Key Insights

**Defensive Dominance**: 7 of 10 features are defensive metrics. Teams with elite pass defenses consistently outperform as away underdogs.

**Passing Defense Critical**: Limiting explosive passing plays and maintaining success rate are the strongest predictors of covering large spreads.

**Practical Takeaway**: Away teams that prevent big plays and force opponents into long drives tend to cover +13 spreads, even against stronger opponents.

---

## Practical Recommendations

### By Alternate Line

| Line | Best Model | Strategy | Risk Level |
|------|-----------|----------|------------|
| **+13** | Decision Tree (53.5) | Target elite pass defenses as away underdogs | Low |
| **+7** | GLMnet (55.3) | Balanced approach; more betting opportunities | Moderate |
| **+3** | XGBoost (18.4) | Highly selective; high-confidence only | High |
| **-7** | Avoid | Not profitable with current features | N/A |

### Threshold Optimization
- Optimal thresholds: 0.35–0.55 (not default 0.50)
- **Selectivity matters**: Bet ~20-40% of games, not all games
- Higher thresholds at tighter spreads = more selective betting

---

## Limitations

1. **Missing Variables**: No player-level data, weather, injuries, or coaching changes
2. **Sample Size**: 3-season test period; longer validation needed
3. **Market Dynamics**: Static lines; real markets adjust dynamically
4. **Feature Scope**: Only team-level differentials; interaction terms unexplored
5. **Temporal**: 2013–2022 data; recent trends may differ

**For exhaustive limitations discussion, see the full write-up.**

---

## Future Work

- **Enhanced Features**: Player EPA, injuries, weather, travel distance
- **Advanced Models**: Deep learning, ensemble stacking, hyperparameter optimization
- **Expanded Scope**: NFL, other bet types (totals, moneyline), live betting
- **Real-Time Systems**: Automated retraining, line scraping, portfolio optimization

---

## Conclusion

XGBoost and Decision Tree models demonstrate consistent profitability in predicting college football alternate spread coverage. Defensive passing metrics—particularly limiting explosive plays and maintaining success rates—are the strongest predictors. The +13 and +7 alternate lines offer the best risk-adjusted returns.

**Key Finding**: Away teams with elite pass defenses consistently outperform expectations as large underdogs, covering spreads at profitable rates even with house edge factored in.

**For comprehensive methodology, statistical details, and deeper analysis, see:**
- `Final Documents/STA4241 Final Project Write Up.pdf`
- `Final Documents/STA4241 Final Project Presentation.pdf`