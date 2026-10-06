# Where should a French poultry brand export first?

An international market study of 164 countries: 15 PESTEL indicators, a principal component analysis and a clustering that turn public data into a three-stage export roadmap.

**Full case study:** [axeldrtdata.github.io/works/poultry-export-markets](https://axeldrtdata.github.io/works/poultry-export-markets/)

## Context

La poule qui chante, a French poultry producer (fictional OpenClassrooms case), wants to export abroad. The goal was to identify the most promising target countries from public data, covering at least 100 countries and 60% of the world population, with at least eight indicators selected through the PESTEL framework.

## Approach

1. **Data collection**: FAO (poultry production, imports, supply per person, population), World Bank (governance indices, unemployment, internet access, tourism, Logistics Performance Index, income group) and capital-to-capital distances. 15 indicators covering the 6 PESTEL dimensions.
2. **Preparation**: countries missing more than 2 of the 15 indicators excluded, remaining gaps filled with the median of the country's income group, log transformation of 4 skewed variables, standardisation.
3. **Principal component analysis**: 15 indicators reduced to 5 components keeping 79.1% of the variance (Kaiser criterion, scree plot, cumulative variance).
4. **Clustering**: hierarchical clustering (Ward), elbow method and silhouette score all point to 4 clusters, then refined with k-means.

## Results

| Profile                       | Countries | Rule of law (0–100) | Poultry imports | Distance from Paris |
| ----------------------------- | --------- | ------------------- | --------------- | ------------------- |
| **Developed economies**       | 35        | **79.7**    | **213 kt**      | **3,537 km**        |
| Large emerging economies      | 38        | 49.3        | 135 kt          | 6,294 km            |
| Small middle-income countries | 35        | 61.4        | 18 kt           | 6,567 km            |
| Developing countries          | 56        | 42.8        | 37 kt           | 6,301 km            |

**Recommendation**: target the developed economies, starting with Europe (Germany, Belgium, Italy, Spain), then North America, Japan and Oceania, while monitoring large emerging markets such as China and India.

164 countries · 95.3% of the world population · 5 principal components · 4 clusters

## Repository structure

```
poultry-export-markets/
├── notebooks/
│   ├── 01-data-preparation-and-eda.ipynb   # cleaning, imputation, outliers, scaling, correlations
│   └── 02-pca-and-clustering.ipynb         # PCA, hierarchical clustering, k-means, profiles, map
├── data/
│   ├── raw/                                # original source files, as downloaded
│   │   ├── fao_food_balance_2017.xlsx      # FAO: poultry production, imports, supply per person
│   │   ├── fao_population_2000_2018.xlsx   # FAO: population and population growth
│   │   ├── internet_access.csv             # World Bank: internet users (% of population)
│   │   ├── unemployment_rate.csv           # World Bank: unemployment rate
│   │   ├── tourism_arrivals.csv            # World Bank: international tourist arrivals
│   │   ├── logistics_performance_index.csv # World Bank: Logistics Performance Index
│   │   ├── worldwide_governance_indicators.xlsx # World Bank WGI: governance scores and income groups
│   │   └── capital_distances.xlsx          # Gleditsch: distances between capital cities
│   ├── df_final.xlsx                       # merged dataset, 181 countries × 15 indicators
│   ├── df_volaille_final.csv               # cleaned dataset, output of notebook 01
│   └── df_scaled.csv                       # standardised variables used for the PCA
├── requirements.txt
└── README.md
```

## How to run it

```bash
git clone https://github.com/axeldrtdata/poultry-export-markets.git
cd poultry-export-markets
pip install -r requirements.txt
```

Open the folder in VS Code (with the Python and Jupyter extensions) and run the notebooks in order: the first one creates the cleaned files used by the second.

## Stack

Python · pandas · NumPy · scikit-learn · SciPy · Matplotlib · Seaborn · Plotly · Excel · VS Code

## Data sources and pipeline

The raw files come from the FAO (FAOSTAT), the World Bank (World Development Indicators, Worldwide Governance Indicators, Logistics Performance Index) and Kristian Skrede Gleditsch's capital-distance dataset.

Governance indicators (control of corruption, political stability, rule of law, regulatory quality) are the 2017 **scores on the World Bank's 0–100 scale**, where higher means better governance; the income group of each country comes from the same file. Full methodology: [WGI documentation](https://www.worldbank.org/en/publication/worldwide-governance-indicators/documentation).

1. **Consolidation in Excel**: the sources were merged into `df_final.xlsx` through correspondence tables on ISO3 and FAO country codes, so that each country appears exactly once.
2. **Cleaning and analysis in Python**: `01-data-preparation-and-eda.ipynb` takes `df_final.xlsx` as input, then `02-pca-and-clustering.ipynb` runs the PCA and the clustering.

## Note

The notebooks were first written in French during my OpenClassrooms Data Analyst training. Their text, comments and chart labels have since been translated into English; variable and column names were kept as in the original data.

---

Axel Derobert · Data Analyst · [Portfolio](https://axeldrtdata.github.io) · [LinkedIn](https://www.linkedin.com/in/axel-derobert-5717463b1/)
