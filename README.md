# Amazon Sales EDA

Exploratory data analysis of Amazon sales data (merchant and Amazon-fulfilled orders). The analysis is implemented in the Jupyter notebook `AmazonSalesEDA.ipynb` and uses the raw export `Amazon Sale Report.csv` as the input dataset.

This repository contains a reproducible EDA pipeline that:
- Loads the Amazon sales CSV export
- Cleans and preprocesses the data (duplicates, missing values, date parsing)
- Produces summary statistics and visualizations to understand sales performance, categories, sizes, fulfillment and B2B vs Retail split.

---

## Files in this repo

- `AmazonSalesEDA.ipynb` — Jupyter notebook containing the full EDA code, visuals and narrative.
- `Amazon Sale Report.csv` — (not checked in here) Original raw export used by the notebook.
- `README.md` — This document.

> Note: the notebook shows the sequence of work performed (data loading, column selection, cleaning, deduplication, date parsing and multiple plots & tables).

---

## Goals

- Clean the raw Amazon sales export and keep fields useful for analysis (Order ID, Date, Status, Fulfilment, Category, Size, Courier Status, Qty, Amount, ship-city/state/country, B2B flag).
- Remove duplicate order rows and rows with missing shipping location.
- Convert date strings to datetimes and create monthly summaries.
- Produce charts and summary tables to answer business questions such as:
  - Which categories and sizes sell most?
  - Monthly order distribution
  - Fulfilment / Courier status breakdown (Shipped vs Unshipped)
  - Retail vs B2B proportion

---

## Summary of main findings (from the notebook)

The notebook includes detailed plots and numbers; a few highlights from the cleaned dataset:

- Dataset size (after cleaning & deduplicating): ~108k unique orders (noting the notebook starts from ~128k raw rows and drops duplicates/missing rows during cleaning).
- Duplicate rows observed in the raw export: roughly 6.6% of rows were duplicate Order IDs and were removed.
- Shipping columns had sparse missing values; the notebook drops rows missing ship-city / ship-state / ship-country.
- Date range in the sample notebook: Orders observed across months in the dataset (example months present: Apr–Jun 2022).
- Month distribution: Visualized with counts and pie charts to show monthly order share.
- Category distribution: Top categories in the sample data include "Set" and "Kurta" (the notebook shows a histogram and concludes 'Set' slightly surpasses 'Kurta').
- Courier Status: Most orders are "Shipped" with a smaller portion "Unshipped" (e.g., shipped ~102k, unshipped ~6k in the cleaned sample).
- B2B vs Retail: Retail (B2B=False) accounts for the overwhelming majority (~99.3%), B2B ~0.7% in the sample.

These are results from the included snapshot notebook — run the notebook on your own dataset export to reproduce exact numbers for your data.

---

## How to run

1. Install Python (recommended 3.8+)
2. Create a virtual environment (optional but recommended):
   - python -m venv .venv
   - source .venv/bin/activate  (macOS / Linux)
   - .venv\Scripts\activate     (Windows)
3. Install the main dependencies:
   - pip install pandas numpy matplotlib seaborn jupyterlab
   - (You can add more packages if you want to export images or use nbconvert)
4. Place your Amazon export named `Amazon Sale Report.csv` in the repository root (or update the notebook path).
5. Open and run the notebook:
   - jupyter lab  (or jupyter notebook)
   - Open `AmazonSalesEDA.ipynb` and run cells top-to-bottom.

Notes:
- When reading the CSV, pandas may emit DtypeWarning for mixed type columns. In the notebook we select a subset of useful columns and handle types explicitly.
- The notebook converts the "Date" column using format `%m/%d/%Y %H:%M`. Adjust the parsing format if your export uses a different format.

---

## Data cleaning steps performed (high level)

- Read CSV with pandas.
- Select a subset of useful columns for analysis (Order ID, Date, Status, Fulfilment, ship-service-level, Category, Size, Courier Status, Qty, currency, Amount, ship-city, ship-state, ship-country, B2B).
- Drop rows where shipping columns are missing (ship-city/state/country) for geographic analysis.
- Remove duplicate orders by `Order ID` (keep the first occurrence).
- Convert `Date` to pandas datetime dtype and extract month for monthly aggregations.
- Drop rows with remaining missing values where needed (optional — notebook demonstrates choices).
- Basic aggregation & plotting: monthly counts, category histograms, pie charts for B2B, Courier Status counts, size distribution.

---

## Suggested next steps (analysis ideas)

- Revenue analysis: group by Category / Size / City / State to compute total revenue and average order value.
- Time series: daily/weekly revenue and orders, trend detection, moving averages.
- Cohort or repeat purchase analysis if customer IDs are available.
- RFM segmentation and CLTV estimation for prioritizing marketing.
- Map visualizations: plot orders by city/state to identify regional demand hotspots.
- Anomaly detection for refunds/cancellations or sudden drops in shipped orders.

---

## Reproducibility & notes

- If running on a different Amazon export, column names can change slightly. The notebook trims column names (str.strip()) and selects only the columns it expects; adapt the column list if your export contains different headers.
- The sample notebook contains inline comments and warnings (e.g., SettingWithCopyWarning). These can be addressed by using `.loc` assignment and avoiding chained indexing.
- If you want, export notebook outputs to HTML or PDF for sharing.

---

## Dependencies (quick list)

- Python 3.8+
- pandas
- numpy
- matplotlib
- seaborn
- jupyter / jupyterlab

Example:
pip install pandas numpy matplotlib seaborn jupyterlab

---

## License & attribution

Add a license file if you plan to share this publicly (for example MIT). The data used here is a private/exported Amazon Sales Report — do not publish raw account data or other PII.

---

If you want, I can:
- Produce a requirements.txt with exact package versions used in the notebook.
- Add a short script (Python) that performs the same cleaning steps used in the notebook (so the cleaning is runnable without opening the notebook).
- Create example plots exported as PNG for README if you want visual summaries embedded.

