# IKEA Product Price & Discount Modeling

![R](https://img.shields.io/badge/-R-276DC3?style=flat-square&logo=r&logoColor=white) ![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=flat-square)

Regularized regression and classification models predicting IKEA product prices and discount likelihood from product characteristics.

## Overview

This project uses a dataset of 3,694 IKEA products (13 variables: price, dimensions, category, designer, online availability, etc.) to build two complementary models: a **regression model** predicting log-price from product characteristics, and a **classification model** predicting whether a product is currently discounted.

## Methodology

1. **Exploratory Data Analysis:** distribution, correlation, and missingness checks using `DataExplorer`
2. **Feature engineering:** log-transform of price to correct right-skew, cleaning of the `old_price` field to derive a binary `is_discounted` target, and encoding of categorical predictors (category, designer origin, online availability)
3. **Regression models:** OLS, Ridge, Lasso, and Elastic Net (via `glmnet`), tuned with 10-fold cross-validation, to predict log-price
4. **Classification models:** Logistic, Ridge, Lasso, and Elastic Net logistic regression to predict `is_discounted`, evaluated via ROC/AUC

## Key Findings

- **Price prediction was highly effective:** Lasso Regression was the best model, R² = 71.2%, RMSE = 0.785. Physical dimensions (height, width, depth) were the strongest predictors, larger furniture consistently costs more. `sellable_online` and `other_colors` also increased price; whether the designer was external to IKEA was not significant
- **Discount prediction was largely unsuccessful:** Ridge Logistic achieved the best AUC (0.68), but with only ~4% of items discounted in the test set, all regularized models converged to 0% specificity, defaulting to predicting "no discount" for every item
- **Price and discounts are driven by different logics:** price reflects production cost (physical size, OR ≈ 1.00 for discount), while discounts are driven by inventory strategy, concentrated in specific categories like Chests of Drawers (OR = 4.79) rather than by product size
- The severe class imbalance in the discount data is the main modeling bottleneck, resampling techniques (SMOTE, up/downsampling) would likely be needed to meaningfully improve discount classification

## Repository Structure

```
ikea-price-discount-modeling/
├── ikea_price_discount_modeling.qmd   # Full analysis (Quarto document)
├── data/
│   └── ikea_data.csv                  # IKEA product dataset
├── README.md
├── LICENSE
└── .gitignore
```

## Requirements

`R` (>= 4.2) with: `tidyverse`, `DataExplorer`, `caret`, `glmnet`, `car`, `mctest`, `pROC`

```r
install.packages(c("tidyverse", "DataExplorer", "caret", "glmnet", "car", "mctest", "pROC"))
```

## How to Run

```bash
git clone https://github.com/palomanavarro22/ikea-price-discount-modeling.git
cd ikea-price-discount-modeling
```

Open `ikea_price_discount_modeling.qmd` in RStudio and render. The script reads `data/ikea_data.csv` directly (adjust the path if you move the file).

## Limitations & Future Research

The dataset lacks a material-composition variable, likely the strongest missing predictor of price beyond dimensions. Discount prediction is fundamentally limited by class imbalance rather than model choice, incorporating inventory, seasonality, or product-age data, along with resampling techniques, would likely be necessary to build a useful discount classifier. The analysis is also confined to a single market, adding a country variable would allow comparison of pricing strategies across regions.

## Author

**Paloma Navarro**
MSc in Computational Social Science, UC3M
[LinkedIn](https://www.linkedin.com/in/paloma-navarro-3b7927280/)

## Citation

If you use this analysis, please cite:

```
Navarro, P. (2026). IKEA Product Price & Discount Modeling.
GitHub repository: https://github.com/palomanavarro22/ikea-price-discount-modeling
```

## License

This project is licensed under the MIT License, see the [LICENSE](LICENSE) file for details.

---

*Coursework for the Regularized Regression course, MSc in Computational Social Science, UC3M.*
