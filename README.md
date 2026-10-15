# CSC 221: Data Science

Hofstra University · Taught Fall 2025 and Fall 2026

An overview of data science: data collection, integration, preprocessing, modeling, analysis, visualization, prediction and informed decision making, and data security and privacy. The course is for students heading into their first data job, and it follows the whole workflow of a real analysis rather than just the modeling step: getting and checking the data, validating honestly, and presenting a result a decision maker can trust. The worked examples and projects use real financial market data.

## What students learn

- Loading and merging messy vendor exports: banner and footer rows, type coercion, proving a join key is unique, and checking every merge.
- Exploratory data analysis on fat-tailed data: mean vs. median, sector and sub-industry slices, Spearman heat maps, decile spreads, and multivariate outlier detection with PyOD (ECOD, Isolation Forest).
- Not fooling yourself: spotting leakage (features built from the target), fitting preprocessing on training rows only, out-of-time tests, and spurious regression between random walks.
- Classification on an imbalanced target: Gaussian Naive Bayes, LDA and logistic regression, stratified splits, class weights, one-hot encoding, precision, recall, F1, ROC and AUC.
- Neural networks in PyTorch: logistic regression as a single neuron, the training loop, learning-rate sweeps, SGD vs. Adam, and dropout, weight decay and early stopping against a validation split.
- Regression and regularization: OLS by hand and in statsmodels, p-values and VIF, ridge and lasso as shrinkage (Stein's paradox), polynomial expansion, and gradient-boosted trees (XGBoost, CatBoost).
- Later weeks of the course: validation and cross-validation, tree-based models, SHAP for explainability and feature selection, and data security and privacy (re-identification, differential privacy, models that leak training data).

## Highlights

| Notebook | What it covers | Open |
|---|---|---|
| [01 Exploratory data analysis](notebooks/01_exploratory_data_analysis.ipynb) | Week 3. Merges two exports covering about 1,600 US stocks, then asks which features lined up with the best 1-year returns. Covers describe, sector and sub-industry slices, a Morningstar-style size/style box, a correlation heat map, a leakage alarm, decile spreads, PyOD outliers and a robustness check. | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/jordan-low-mit/hofstra-csc221-data-science/blob/main/notebooks/01_exploratory_data_analysis.ipynb) |
| [02 Classification and PyTorch](notebooks/02_classification_and_pytorch.ipynb) | Week 5. Predicts which stocks are on an AI shortlist (65 of 1,600) with Naive Bayes, LDA and logistic regression, adds one-hot sectors, then rebuilds the classifier in PyTorch. Covers learning rate, SGD vs. Adam, overfitting, dropout, weight decay and early stopping. | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/jordan-low-mit/hofstra-csc221-data-science/blob/main/notebooks/02_classification_and_pytorch.ipynb) |
| [03 Regression and shrinkage](notebooks/03_regression_and_shrinkage.ipynb) | Week 6. Compares the linear probability model with logistic regression, then fits OLS by hand and in statsmodels (does the market pay a premium for AI stocks?). Covers VIF, train vs. test R², ridge and lasso with small-sample and junk-feature experiments, regression in PyTorch, and spurious regression on random walks. | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/jordan-low-mit/hofstra-csc221-data-science/blob/main/notebooks/03_regression_and_shrinkage.ipynb) |
| [04 Imputing MAC3 factor history](notebooks/04_imputing_mac3_factors.ipynb) | Week 7. Reconstructs a risk model's factor exposures for years before the model existed. Trains on 2022–2024, tests on 2025 and scores with Spearman against a naive baseline. Covers ridge and lasso tuned leave-one-year-out, degree-2 expansion, a cumulative-market-cap feature that fixes Mid Cap, and XGBoost and CatBoost. Ends with the brief for a project option: impute the factors at the 2000 and 2007 market peaks. | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/jordan-low-mit/hofstra-csc221-data-science/blob/main/notebooks/04_imputing_mac3_factors.ipynb) |

## How the course is taught

- **Flipped classroom.** Students do the readings and videos before class. Class time goes to questions and building things, and each week ends with reflection questions.
- **Graded entirely on projects.** There are three group projects (groups of 3 to 5), each delivered as a recorded presentation, code notebooks and a Trello board. Kaggle leaderboard scores count where a project has a competition. Projects have included a Kaggle competition on market microstructure around TARP, a real data-science take-home assignment from HarperCollins, a statistical-arbitrage strategy, an Airbnb investment analysis, and the final project, "Building the Black Box: A Production ML System for Systematic Trading" on 1999–2025 Bloomberg factors.
- **AI tools within a written policy.** Students must disclose which tools they used and how. Bugs and hallucinations from generative AI carry an extra penalty, and the standard is code that runs completely and correctly.

## Data

All four notebooks use **Bloomberg PORT exports (Bloomberg L.P., licensed; not redistributed)**. The data files are not in this repository. Outputs that showed individual securities have been cleared; aggregate charts and summary statistics remain.

- **Notebooks 01–03** use two exports of the same ~1,600 US stocks, `20260917_BLG_Port.xlsx` and `20260917_BLG_Port_2.xlsx`. Each has two banner rows above the header and a footer with a disclaimer, and the two are joined on `Ticker`. Columns:
  - Identifiers and classification: `Ticker`, `Short Name`, `CUSIP` (file 1 only), `GICS Sector`, `GICS Ind Name`, `GICS SubInd`, `GICS SubInd Name`.
  - Valuation and fundamentals: `Market Cap`, `P/E`, `P/B` and `P/S` (each also as a BEst 12-month forward estimate), `ROE LF`, FCF yield (current and history), net fixed asset turnover, total-debt growth, employees (now and 5 years ago), shares outstanding (this year and last), value of buybacks, average repurchase price, and BEst capex.
  - Market: `Total Return:Y-1`, `Total Return:M-3`, `Beta:Y-1`, `Equity Traded Val:D-1`, and 1-year insider selling volume.
  - Scores: ESG disclosure, energy intensity, BI STOX scores (quality, leverage, low volatility, profitability, momentum, multi-factor), and Bloomberg issuer default likelihood and implied CDS spread.
  - Risk-model exposures: 9 legacy PORT US factors (profitability, volatility, leverage, momentum, value, growth, dividend yield, earnings variability, size) and 17 MAC3 style factors (liquidity, dividend yield, value, beta, profitability, volatility, residual volatility, variability, earnings yield, growth, quality, long-term reversal, size, momentum, leverage, mid cap, valuation).
- **Notebook 04** uses the same two-file layout at four year-ends (30 Nov 2022, 2023, 2024 and 2025), stacked into a 6,400-row panel. The MAC3 exposures are the targets.
- **Notebook 03, Part 7** simulates its random walks, so that section runs without any data.

To follow along, any cross-section of stocks with sector, market cap, valuation ratios, returns, beta and risk-model factor exposures will do. Rename your columns to match in each notebook's rename step.

## Instructor

[Jordan Low](https://github.com/jordan-low-mit), adjunct professor at Hofstra University.

---

Materials © Jordan Low. Shared for reference; please ask before reusing in a course.
