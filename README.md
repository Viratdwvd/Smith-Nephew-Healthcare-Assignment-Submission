# SIOP Reporting Dashboard — Case Study 2

> **Sales, Inventory & Operations Planning** | Power BI + Python EDA  
> Virat Dwivedi · Reg No. 22MIA1101 · B.Tech CSE (Business Analytics) · VIT Chennai

---

##  Project Overview

This project delivers a complete SIOP (Sales, Inventory & Operations Planning) reporting solution built for Case Study 2 of the internship assessment. It covers the full pipeline — from raw data transformation and KPI engineering to an interactive Power BI dashboard and a comprehensive exploratory data analysis notebook.

**Core objective:** Transform multi-snapshot forecast data, compute forecast accuracy/bias KPIs, and build an executive-grade Power BI dashboard that enables supply chain planners to monitor forecast performance across countries, periods, and planning lags.

---

##  Repository Structure

```
SIOP-Reporting/
│
├── SIOP_EDA_Virat_Dwivedi.ipynb     # Full EDA notebook (14 analyses)
├── SIOP_Report_Virat_Dwivedi.docx   # Submission report (8 sections)
├── README.md                        # This file
│
├── dashboard/
│   ├── SIOP_Dashboard.pbix          # Power BI file (6 pages)
│   └── screenshots/
│       ├── Executive_Summary.png
│       ├── Executive_Overview1.png
│       ├── Forecast_vs_Actual2.png
│       ├── Segmentation_View.png
│       ├── Accuracy_Trend.png
│       ├── Item_Level_Analysis.png
│       ├── Forecast_Insights___Risk_Analysis.png
│       └── Model_View.png
│
└── data/
    └── Case_Study_2_SIOP_Reporting.xlsb   # Source data (not included — confidential)
```

---

##  Business Requirements Addressed

| # | Requirement | Status |
|---|---|---|
| 1 | Forecast Accuracy % = 1 − \|Lag Qty − Actual Qty\| / Actual Qty | ✅ Implemented in DAX (Lag 0 & Lag 1) |
| 2 | Forecast Bias % = (Lag Qty − Actual Qty) / Actual Qty | ✅ Implemented in DAX (Lag 0 & Lag 1) |
| 3 | Executive charts for Revenue, Non-Revenue & CAPEX by Country | ✅ Executive Overview page |
| 4 | Lag Selection filter: Resultant / Lag 0 / Lag 1 | ✅ Dynamic SWITCH() measure + SlicerLag table |
| 5 | Wide-format data transformation (Resultant, Lag 0, Lag 1 columns) | ✅ Power Query 4-stage pipeline |
| 6 | Merge forecast with actuals on Item + Country + Period | ✅ Left join in Power Query |
| 7 | Report submission documenting approach and findings | ✅ 8-section Word report |

---

##  Data Model

The solution uses a **star schema** in Power BI:

```
DimPeriod ──────┐
                ├──── ForecastWide (Fact) ────── DimCountry
Actual ─────────┘
                        │
                   SlicerLag (Disconnected — drives SWITCH measures)
```

**ForecastWide** is the central fact table produced by the Power Query transformation. It contains one row per Item–Country–Period with 9 lag columns (Resultant/Lag0/Lag1 × Revenue/NonRevenue/CAPEX) plus merged Actual columns.

---

##  Data Transformation Pipeline (Power Query)

| Stage | Step | What It Does |
|---|---|---|
| 1 | Rename & Cast | Fixes `CAPEX Foreacast` typo; casts `Item` to Text in both tables |
| 2 | Snapshot Labelling | Maps `AsOfPeriod` → `SnapshotLabel` (2023 P12 = Resultant, P11 = Lag 0, P10 = Lag 1) |
| 3 | Pivot to Wide Format | Unpivot → add `PivotKey` (SnapshotLabel + "_" + Column) → Pivot to produce 9 lag columns |
| 4 | Merge with Actuals | Left join ForecastWide ← Actual on `Item + Country + Period`; expand actuals |

---

##  DAX Measure Catalogue

### Block 1 — Forecast Aggregation (9 measures)
```dax
Resultant Revenue Forecast = SUM(ForecastWide[Resultant Revenue Forecast])
Lag0 Revenue Forecast      = SUM(ForecastWide[Lag0 Revenue Forecast])
Lag1 Revenue Forecast      = SUM(ForecastWide[Lag1 Revenue Forecast])
-- (same pattern for NonRevenue and CAPEX)
```

### Block 2 — Actuals (3 measures)
```dax
Actual Revenue Qty    = SUM(ForecastWide[Actual Revenue Qty])
Actual NonRevenue Qty = SUM(ForecastWide[Actuals NonRevenue Qty])
Actual Capex Qty      = SUM(ForecastWide[Actuals Capex Qty])
```

### Block 3 — KPI Measures (4 measures)
```dax
Forecast Accuracy % (Lag0 Revenue) =
    VAR LagQty = [Lag0 Revenue Forecast]
    VAR ActQty = [Actual Revenue Qty]
    RETURN IF(ActQty = 0, BLANK(), 1 - ABS(LagQty - ActQty) / ActQty)

Forecast Bias % (Lag0 Revenue) =
    VAR LagQty = [Lag0 Revenue Forecast]
    VAR ActQty = [Actual Revenue Qty]
    RETURN IF(ActQty = 0, BLANK(), DIVIDE(LagQty - ActQty, ActQty))
```

### Block 4 — Dynamic Lag Switch (3 measures)
```dax
Selected Revenue Forecast =
    SWITCH([Selected Lag Label],
        "Resultant", [Resultant Revenue Forecast],
        "Lag 0",     [Lag0 Revenue Forecast],
        "Lag 1",     [Lag1 Revenue Forecast],
        [Resultant Revenue Forecast]
    )
```

### Block 5 — Variance (2 measures)
```dax
Revenue Variance   = [Resultant Revenue Forecast] - [Actual Revenue Qty]
Revenue Variance % = DIVIDE([Revenue Variance], [Actual Revenue Qty])
```

---

##  Dashboard Pages

| Page | Purpose | Key Visuals |
|---|---|---|
| **Landing** | Navigation hub + headline KPIs | 4 KPI cards, 6 bookmark buttons |
| **Executive Overview** | Lag comparison + revision tracking | Lag-switched line chart, CAPEX/Non-Rev cards, Revision waterfall |
| **Forecast vs Actual** | Country & period accuracy deep-dive | Matrix table (conditional formatting), period line chart, Revenue Gap waterfall |
| **Segmentation Analysis** | Volume distribution by geography | Country treemap, Top 10 Items bar, Segmentation matrix, area chart |
| **Accuracy Trend** | Period-by-period accuracy monitoring | Dual-line accuracy chart, Volatility bars, Forecast Direction table |
| **Item Analysis** | SKU-level drill-down | Item table with variance, Set vs FG donut charts |
| **Forecast Insights & Risk** | Bias outlier identification | Bias league table, Non-Rev trend, Decomposition waterfall |

---

## Key Findings (EDA)

### Headline KPIs

| Metric | Value | Benchmark |
|---|---|---|
| Actual Revenue Qty | 34.44 M | — |
| Resultant Revenue Forecast | 16.05 M | — |
| Total Revenue Gap | 18.39 M | — |
| Forecast Accuracy (Lag 0) | 57.0% | ≥ 80% |
| Forecast Accuracy (Lag 1) | 40.9% | ≥ 80% |
| Forecast Bias (Lag 0) | −43% | 0% |
| Forecast Attainment | 0.47 | 1.0 |

### Country-Level Accuracy

| Country | Accuracy Lag 0 | Bias Lag 0 | Diagnosis |
|---|---|---|---|
| A | 33.0% | −67.0% | Chronically under-forecasted |
| B | −220.3% | +320.3% | Severe over-forecast |
| C | 63.2% | +36.8% | Closest to target |
| D | −1,186.3% | +1,286.3% | Critical outlier |

### EDA Coverage (14 analyses in notebook)
1. Dataset schema & column classification
2. Null value analysis & data quality
3. Forecast measure distributions (log-scale histograms)
4. Country-wise revenue forecast analysis
5. Snapshot (Lag) comparison
6. Temporal trend analysis by period
7. Forecast vs Actual overlay
8. Segmentation analysis
9. Set vs FG analysis
10. Period Offset (forecast horizon) analysis
11. Top Items Pareto curve
12. Country × Period revenue heatmap
13. Forecast revision analysis (Lag delta)
14. Forecast accuracy heatmap by Country & Period

---

##  Tech Stack

| Layer | Tools |
|---|---|
| Data Source | Excel .xlsb (pyxlsb engine) |
| EDA | Python — pandas, numpy, matplotlib, seaborn |
| Transformation | Power Query (M language) |
| Data Model | Power BI star schema |
| KPI Engineering | DAX (SWITCH, DIVIDE, VAR/RETURN pattern) |
| Dashboard | Microsoft Power BI Desktop |
| Report | Microsoft Word (.docx) |

---

## Data Quality Notes

- **Lifecycle column** — 94.7% null. Excluded from all analyses.
- **Actuals CAPEX Qty** — All values zero. CAPEX accuracy analysis not feasible.
- **Item column** — Mixed types (integer + string like `I0138.0926`). Cast to Text before join.
- **Segmentation columns** — 97–99.9% populated. Fully reliable for slicing.
- **Revenue Forecast & Actual Revenue** — Zero nulls. Primary measure is clean.

---

## Contact

**Virat Dwivedi**  
📧 vrtdwvd@gmail.com  
🔗 [LinkedIn](https://linkedin.com/in/viratdwivedi)  
💻 [GitHub](https://github.com/Viratdwvd)  

---

*VIT Chennai · B.Tech CSE — Business Analytics · Case Study 2 — SIOP Reporting*
