# Unemployment Analysis in India During COVID-19

A reproducible exploratory data analysis (EDA) of reported unemployment and labour-market indicators across Indian states and rural/urban areas, with a descriptive comparison of observations before and during the COVID-affected period.

> **Scope:** This project describes the dataset's reported state/area observations. Its unweighted averages are not official population-weighted national unemployment estimates, and the before/after comparison does not establish that COVID-19 caused the observed changes.

## Contents

- [Project highlights](#project-highlights)
- [Repository structure](#repository-structure)
- [Data source and fields](#data-source-and-fields)
- [Analysis workflow](#analysis-workflow)
- [Key findings](#key-findings)
- [Visualizations](#visualizations)
- [Run the analysis](#run-the-analysis)
- [Interpretation and limitations](#interpretation-and-limitations)
- [References](#references)

## Project highlights

- Cleans and validates the source data, parses dates, removes exact duplicate records, and handles missing numeric values within state/area groups.
- Explores distributions, monthly trends, state comparisons, rural/urban differences, and labour-market indicator relationships.
- Compares observations from before March 2020 with observations from March 2020 onward; also identifies April–May 2020 as a lockdown-focused window.
- Exports a cleaned dataset, a summary table, written conclusions, and chart images.

## Repository structure

```text
.
├── data/
│   ├── kaggle_unemployment.zip
│   ├── Unemployment in India.csv
│   └── Unemployment_Rate_upto_11_2020.csv
├── visualizations/                       # 14 generated PNG charts
├── India_Unemployment_COVID19_EDA.ipynb  # End-to-end analysis
├── India_Unemployment_Cleaned.csv        # Cleaned analysis dataset
├── India_Unemployment_Summary.csv         # Summary metrics
├── India_Unemployment_Conclusions.md      # Findings and caveats
└── README.md
```

The notebook selects `data/Unemployment in India.csv` when available. It can download the Kaggle dataset if no CSV is present in `data/`; alternatively, download and extract the source dataset there manually.

## Data source and fields

The source dataset is [Unemployment in India on Kaggle](https://www.kaggle.com/datasets/gokulrajkmv/unemployment-in-india). It contains reported state/region and area observations, including unemployment rates, estimated employment counts, and labour-force participation rates. See the Kaggle page for the dataset's original documentation and terms.

The analysis uses the monthly, state/area records in the source file. The notebook standardizes relevant column names and creates these derived fields:

| Field | Description |
|---|---|
| `State` | State or region name |
| `Date` | Parsed observation date |
| `Area` | Area category, such as Rural or Urban |
| `Unemployment_Rate` | Reported unemployment rate, in percent |
| `Employed` | Estimated employed count; this is a count, not a rate |
| `Labour_Participation_Rate` | Reported labour-force participation rate, in percent |
| `Year`, `Month`, `Month_Name`, `Year_Month` | Date-derived grouping fields |
| `COVID_Period` | `Pre-COVID` before March 2020; `COVID-affected` from March 2020 onward |
| `Lockdown_Focus` | Whether the observation is in April or May 2020 |
| `Employment_Rate` | Derived as `Labour_Participation_Rate × (1 − Unemployment_Rate / 100)` when participation data is available |

## Analysis workflow

The notebook performs the following steps:

1. Loads the source CSV and inspects its dimensions, columns, summary statistics, missingness, and duplicate rows.
2. Normalizes column names and data types, validates rate ranges, removes exact duplicate records, and drops rows without a valid state, date, or unemployment rate.
3. Fills missing numeric values with the median for the same state and area where a group median can be calculated. Missing unemployment values are never replaced with zero.
4. Calculates descriptive statistics and produces charts for distributions, trends, state rankings, period comparisons, area comparisons, correlations, and missingness.
5. Writes the cleaned data, summary metrics, conclusions, and PNG figures to the project folders.

Generated files are overwritten when the notebook is rerun.

## Key findings

The checked-in summary reports the following results for the cleaned dataset:

| Measure | Result |
|---|---:|
| Observations | 740 |
| States/regions | 28 |
| Date range | May 2019–June 2020 |
| Mean observed unemployment rate | 11.79% |
| Median observed unemployment rate | 8.35% |
| Pre-COVID mean (before March 2020) | 9.51% |
| COVID-affected mean (March 2020 onward) | 17.77% |
| Difference between period means | +8.26 percentage points |
| Highest mean monthly unemployment | May 2020 (24.88%) |
| Highest-average-unemployment state in this sample | Tripura (28.35%) |

These are descriptive statistics over available state/area observations, not population-weighted national rates. See [India_Unemployment_Conclusions.md](India_Unemployment_Conclusions.md) for the concise conclusions and limitations, and [India_Unemployment_Summary.csv](India_Unemployment_Summary.csv) for the exported summary.

## Visualizations

The notebook writes these 14 figures to `visualizations/`:

1. Missingness overview
2. Unemployment-rate distribution
3. Unemployment-rate boxplot
4. Monthly unemployment trend
5. Top 10 states by average unemployment
6. Monthly comparison for three selected states
7. Pre-COVID vs. COVID-affected period means
8. Top 10 state-level increases between periods
9. Selected states' period comparison
10. Labour-market indicator correlation heatmap
11. State-by-month unemployment heatmap
12. Rural vs. urban comparison
13. Month-to-month unemployment change
14. Monthly labour-force participation trend

## Run the analysis

Use Python 3 with Jupyter Notebook or JupyterLab.

1. Clone the repository and change into its directory:

   ```bash
   git clone https://github.com/naveedfaizvi/Unemploment-Analysis-in-India-in-COVID-19.git
   cd Unemploment-Analysis-in-India-in-COVID-19
   ```

2. Install the notebook dependencies:

   ```bash
   python -m pip install jupyter pandas numpy matplotlib seaborn ipython
   ```

3. Start Jupyter and open `India_Unemployment_COVID19_EDA.ipynb`:

   ```bash
   jupyter notebook
   ```

4. Run all cells from top to bottom. The checked-in `data/Unemployment in India.csv` is used when present. If running without that CSV, download the source dataset from Kaggle and place/extract its CSV files in `data/`.

The notebook saves the cleaned data and summary in the repository root and the figures in `visualizations/`. To export an HTML report after executing the notebook:

```bash
jupyter nbconvert --to html India_Unemployment_COVID19_EDA.ipynb
```

## Interpretation and limitations

- State and month averages are unweighted across available records; coverage and the number of observations may vary. They should not be read as official national unemployment estimates.
- `Pre-COVID` means dates before March 2020; `COVID-affected` begins in March 2020, which is a transition month rather than a full lockdown month. The April–May 2020 flag is a separate focused window.
- The comparison is descriptive, uses periods of different lengths, and does not control for other factors. It cannot establish a causal effect of the pandemic or lockdowns.
- Group-median imputation can affect estimates. Missing observations in the heatmap indicate absent data, not zero unemployment.
- The derived employment rate uses labour-force participation and unemployment mathematically, so its correlation with unemployment is not independent evidence. Estimated employed values are counts.
- The dataset ends in June 2020 for the selected source file; this analysis cannot describe subsequent recovery.

## References

- Kaggle dataset: [Unemployment in India](https://www.kaggle.com/datasets/gokulrajkmv/unemployment-in-india)
- Main analysis: [India_Unemployment_COVID19_EDA.ipynb](India_Unemployment_COVID19_EDA.ipynb)
