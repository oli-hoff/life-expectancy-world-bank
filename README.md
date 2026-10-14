# What Makes People Live Longer?

Predicting life expectancy from World Bank development indicators, as part of the Udacity course "Introduction to Data Science and Supervised Machine Learning".

## Motivation

Life expectancy at birth differs by more than 30 years between countries. About 55 years in Nigeria, about 86 years in Monaco. This project looks at which development indicators go along with a long life and how well these indicators can predict the life expectancy of a country.

## Questions

1. Which indicators are most important for predicting life expectancy, and how do they influence the prediction?
2. Which countries live much longer or shorter than their indicators suggest?
3. How accurately can a model predict the life expectancy of a country?
4. What would the model predict if every country with low access to electricity had full access?

## Data

Source: [World Bank DataBank – World Development Indicators (WDI)](https://databank.worldbank.org/source/world-development-indicators), bulk download.

The raw files are too large for GitHub and are not part of this repository. To run the notebook:

1. Download the bulk file: https://databank.worldbank.org/data/download/WDI_CSV.zip
2. Extract it into the folder `data/WDI_CSV/`, so that these files exist:
   - `data/WDI_CSV/WDICSV.csv` – values of all indicators for all countries and years
   - `data/WDI_CSV/WDICountry.csv` – information about each country (for example the region)
   - `data/WDI_CSV/WDISeries.csv` – information about each indicator (for example its topic)
3. Run `life_expectancy_worldbank.ipynb` from top to bottom.

The data used in the analysis (data release from October 2026): life expectancy at birth in 2024 as the target, and for all other indicators the most recent value from 2020 to 2024.

## Libraries

The analysis was done with Python 3 (Anaconda) and Jupyter Notebook.

| Library | Version |
|---|---|
| pandas | 3.0.3 |
| numpy | 2.4.6 |
| matplotlib | 3.11.0 |
| seaborn | 0.13.2 |
| scikit-learn | 1.9.0 |
| shap | 0.53.0 |

`shap` is not part of Anaconda and can be installed with `pip install shap`.

## Files in the repository

| File | Description |
|---|---|
| `life_expectancy_worldbank.ipynb` | Complete analysis following the CRISP-DM process: data understanding, data preparation, modeling, evaluation and a what-if scenario |
| `data/prepared_life_expectancy.csv` | Prepared data set created by the notebook: 177 countries, 19 indicators, the region (one-hot encoded) and life expectancy |
| `images/` | Charts used in the blog post |
| `.gitignore` | Keeps the large raw data files out of the repository |
| `README.md` | This file |

## Process (CRISP-DM)

1. Business Understanding: motivation and questions
2. Data Understanding: first look at the three files, removal of groups of countries (for example "World"), target variable, indicator topics and a data leakage check
3. Data Preparation: removal of leakage indicators (mortality, population structure, Human capital index) and sub-group indicators, most recent value 2020-2024, selection of the 30 strongest indicators with at least 90 % coverage, removal of near-duplicates, removal of countries with gaps, one-hot encoding of the region
4. Further Data Understanding after Preparation: distributions and relationships of the selected indicators
5. Modeling: linear regression (scaled data) and random forest (tree depth chosen with 5-fold cross-validation)
6. Evaluation: R2 and mean absolute error, largest prediction errors, coefficients, SHAP values and the electricity scenario

## Summary of the results

- Accuracy: both models predict life expectancy with an average error of about 2 years and explain about 85 % of the differences between countries (random forest: R2 0.86, MAE 1.9 years; linear regression: R2 0.85, MAE 2.2 years).
- Most important indicators: internet (fixed broadband subscriptions), access to electricity, income (GNI per capita) and the fertility rate.
- Unusual countries: Nigeria lives about 10 years shorter than its indicators suggest. Guyana, Benin, Cote d'Ivoire and Guinea also live several years shorter, Malawi, Lebanon and Costa Rica 3-4 years longer.
- Scenario: for the 14 countries with less than 50 % access to electricity, full access would raise the predicted life expectancy by about 4 years on average (Chad about +7 years). The predictions still stay well below the average country (74 years).

## Limitations

- The models find relationships, not causes. Internet or electricity stand for a general level of development.
- The analysis uses one point in time (2020-2024 values, life expectancy 2024).
- 40 countries, mostly small states, were removed because of missing values.

## Blog post

A non-technical summary of the results is published on Medium: [Why Do People in Some Countries Live 30 Years Longer?](https://medium.com/@oli.hoffmann_27249/why-do-people-in-some-countries-live-30-years-longer-86c5d736e7c3)

## Acknowledgements

- Data: The World Bank, World Development Indicators, licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/).
- Course: Udacity, "Introduction to Data Science and Supervised Machine Learning".
- Title image of the blog post: world map and feature importance chart created from the World Bank data and the results of this analysis.
