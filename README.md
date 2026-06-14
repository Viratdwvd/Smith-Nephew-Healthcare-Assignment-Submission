# SIOP Reporting Dashboard

> Sales, Inventory & Operations Planning (SIOP) Reporting Solution developed using Power BI, Power Query, DAX, and Python.

**Author:** Virat Dwivedi  
**Program:** B.Tech CSE (Business Analytics), VIT Chennai  
**Case Study:** Internship Program – Case Study 2 (SIOP Reporting)

---

# Project Overview

This project delivers a complete SIOP (Sales, Inventory & Operations Planning) reporting solution built using Power BI.

The objective was to transform forecast and actual sales data into an executive reporting dashboard that helps business users analyze:

- Revenue Forecast
- Non-Revenue Forecast
- CAPEX Forecast
- Forecast Accuracy
- Forecast Bias
- Forecast Revisions
- Forecast Volatility
- Forecast vs Actual Performance
- Country-Level Forecast Performance
- Item-Level Forecast Performance

---

# Business Requirements Implemented

## Forecast Accuracy %

Business Formula:

```text
Forecast Accuracy %
=
1 - ABS(Forecast Qty - Actual Qty)
    / Actual Qty
```

Implemented for:

- Lag 0 Forecast
- Lag 1 Forecast

---

## Forecast Bias %

Business Formula:

```text
Forecast Bias %
=
(Forecast Qty - Actual Qty)
 / Actual Qty
```

Implemented for:

- Lag 0 Forecast
- Lag 1 Forecast

---

## Dynamic Lag Selection

Dashboard supports:

- Resultant Forecast
- Lag 0 Forecast
- Lag 1 Forecast

Using a disconnected slicer table and SWITCH() DAX measures.

---

# Technology Stack

| Layer | Technology |
|---------|---------|
| Dashboarding | Power BI Desktop |
| Data Transformation | Power Query |
| KPI Development | DAX |
| EDA | Python |
| Data Source | Excel (.xlsb) |
| Documentation | PDF |

---

# Data Model

The solution follows a Star Schema design.

```text
DimCountry
     |
     |
ForecastWide
     |
     |
DimPeriod

Actual
```

Additional disconnected table:

```text
SlicerLag
```

Used to dynamically switch between:

- Resultant
- Lag 0
- Lag 1

---

# DAX Measures Implemented

## Forecast Measures

### Revenue

```dax
Resultant Revenue Forecast
Lag0 Revenue Forecast
Lag1 Revenue Forecast
```

### Non-Revenue

```dax
Resultant NonRevenue Forecast
Lag0 NonRevenue Forecast
Lag1 NonRevenue Forecast
```

### CAPEX

```dax
Resultant CAPEX Forecast
Lag0 CAPEX Forecast
Lag1 CAPEX Forecast
```

---

## Actual Measures

```dax
Actual Revenue Qty
Actual NonRevenue Qty
Actual CAPEX Qty
```

---

## KPI Measures

```dax
Forecast Accuracy % (Lag0 Revenue)
Forecast Accuracy % (Lag1 Revenue)

Forecast Bias % (Lag0 Revenue)
Forecast Bias % (Lag1 Revenue)
```

---

## Dynamic Lag Measures

```dax
Selected Revenue Forecast
Selected NonRevenue Forecast
Selected CAPEX Forecast
```

---

## Variance Measures

```dax
Revenue Variance

Revenue Variance %

Forecast Attainment

Revenue Gap

Forecast Revision

Forecast Volatility
```

---

# Dashboard Pages

---

# 1. Executive Overview

### Purpose

Provide a high-level executive summary of forecast performance.

### Key Visuals

- Revenue Forecast KPI
- Non-Revenue Forecast KPI
- CAPEX Forecast KPI
- Forecast Trend
- Lag Comparison
- Forecast Revision Analysis
- Dynamic Lag Selection

### Business Questions Answered

- What is the latest forecast?
- How has the forecast changed over time?
- Which periods show the highest forecast revisions?
- How do Lag 0, Lag 1, and Resultant forecasts compare?

---

## Screenshot

![Executive Overview](ScreenShots/Executive%20Overview1.png)

---

# 2. Forecast vs Actual Analysis

### Purpose

Compare forecast performance against actual business results.

### Key Visuals

- Country Performance Matrix
- Forecast Accuracy KPI
- Forecast Bias KPI
- Revenue Gap KPI
- Revenue Variance KPI
- Forecast vs Actual Trend
- Revenue Gap Waterfall

### Business Questions Answered

- How accurate are forecasts?
- Are forecasts biased?
- Which countries contribute most forecast error?
- What is the revenue gap?

---

## Screenshot

![Forecast vs Actual](ScreenShots/Forecast%20vs%20Actual2.png)

---

# 3. Segmentation Analysis

### Purpose

Analyze revenue distribution across countries and segmentation regions.

### Key Visuals

- Country Treemap
- Top 10 Revenue Items
- Segmentation Region Matrix
- Revenue Share Trend

### Business Questions Answered

- Which countries contribute the most revenue?
- Which items drive forecast volume?
- How is revenue distributed across segmentation regions?

---

## Screenshot

![Segmentation Analysis](ScreenShots/Segmentation%20View.png)

---

# 4. Accuracy Trend

### Purpose

Monitor forecasting quality over time.

### Key Visuals

- Forecast Accuracy Trend
- Forecast Volatility Trend
- Forecast Direction Analysis
- Country Accuracy Comparison

### Business Questions Answered

- Is forecast quality improving?
- Which periods are unstable?
- Which countries have poor forecast accuracy?
- How volatile is the forecast process?

---

## Screenshot

![Accuracy Trend](ScreenShots/Accuracy%20Trend.png)

---

# 5. Item Level Analysis

### Purpose

Perform SKU-level forecast analysis.

### Key Visuals

- Item Performance Matrix
- Revenue Variance Analysis
- FG vs Set Revenue Split

### Business Questions Answered

- Which items drive forecast error?
- Which products create revenue variance?
- What is the FG vs Set revenue mix?

---

## Screenshot

![Item Analysis](ScreenShots/Item%20Level%20Analysis.png)

---

# 6. Forecast Insights & Risk Analysis

### Purpose

Identify forecasting risks and bias drivers.

### Key Visuals

- Products Causing Forecast Bias
- Non-Revenue Forecast Trend
- Forecast Decomposition Waterfall

### Business Questions Answered

- Which products cause forecast bias?
- Which periods create forecasting risk?
- How does revenue decompose over time?
- Where should planners focus attention?

---

## Screenshot

![Forecast Insights](ScreenShots/Forecast%20Insights%20%26%20Risk%20Analysis.png)

---

# Python Exploratory Data Analysis

A complete EDA notebook was developed using Python.

File:

```text
SIOP_EDA_Virat_Dwivedi.ipynb
```

### EDA Coverage

- Dataset Understanding
- Data Quality Assessment
- Missing Value Analysis
- Forecast Distribution Analysis
- Revenue Distribution Analysis
- Country-Level Analysis
- Period Trend Analysis
- Forecast Revision Analysis
- Forecast Accuracy Analysis
- Segmentation Analysis
- Item-Level Analysis
- Forecast Volatility Analysis
- Revenue Heatmaps
- Business Insight Generation

---

# Key Business Insights

### Forecast Accuracy

- Lag 0 forecasts consistently outperform Lag 1 forecasts.
- Forecast refinement closer to execution improves prediction quality.

### Forecast Bias

- Certain countries exhibit persistent over-forecasting.
- Others show chronic under-forecasting behavior.

### Revenue Gap

- Significant difference exists between forecasted and actual revenue.
- Revenue variance highlights planning inefficiencies.

### Forecast Volatility

- Several periods experience large forecast revisions.
- High volatility indicates lower planning confidence.

### Item-Level Findings

- A small number of products contribute disproportionately to forecast errors.
- Targeted planner intervention can improve overall forecast quality.

---

# Repository Structure

```text
.
│
├── ScreenShots/
│
├── Case Study 2 SIOP Reporting.xlsb
│
├── README.md
│
├── SIOP Report.pdf
│
├── SIOP-Dashboards.pbix
│
└── SIOP_EDA_Virat_Dwivedi.ipynb
```

---

# Files Included

| File | Description |
|--------|--------|
| SIOP-Dashboards.pbix | Power BI Dashboard |
| SIOP_EDA_Virat_Dwivedi.ipynb | Python EDA Notebook |
| SIOP Report.pdf | Project Report |
| README.md | Project Documentation |
| ScreenShots | Dashboard Screenshots |

---

# Skills Demonstrated

- Business Intelligence
- Power BI Dashboard Development
- Power Query ETL
- DAX Measure Development
- Data Modeling
- Forecast Analysis
- Forecast Accuracy Measurement
- Forecast Bias Analysis
- Exploratory Data Analysis
- Data Storytelling
- Executive Dashboard Design

---

# Author

**Virat Dwivedi**

B.Tech CSE (Business Analytics)  
VIT Chennai

GitHub: https://github.com/Viratdwvd

LinkedIn: https://linkedin.com/in/viratdwivedi

---

**Power BI • DAX • Power Query • Python • Business Analytics**
