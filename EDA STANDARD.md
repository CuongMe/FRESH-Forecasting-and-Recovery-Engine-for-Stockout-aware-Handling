# EDA Standard

Decision-oriented checklist for exploring FreshRetailNet before modeling.

## Decision context

| Item | Standard |
|---|---|
| Expected grain | One row represents one **store-product-day**. `hours_sale` and `hours_stock_status` describe hourly behavior within that day. |
| Supported decisions | Daily demand forecasting, latent-demand recovery, replenishment, stockout prevention, service-level management, and waste reduction. |
| Forecast origin | State the point in time when a forecast is made. Treat same-day future sales, stockout states, and aggregations as unavailable unless explicitly observed at that origin. |

## Required EDA checks

| # | Area | Questions to answer | Required deliverable |
|---:|---|---|---|
| 1 | **Decision and grain** | What does one row represent: store-product-day, transaction, or something else? What future decision will this support? | **Data-grain statement** |
| 2 | **Schema and provenance** | What are the columns, units, identifiers, sources, and data types? | **Data dictionary** |
| 3 | **Structural integrity** | Are keys unique? Are there duplicate store-product-time records? Are timestamps consistently formatted and ordered? | **Integrity report** |
| 4 | **Time coverage** | What periods and entities are covered? Are there gaps, irregular sampling, or changing coverage over time? | **Coverage calendar** |
| 5 | **Target behavior** | What is the demand distribution? How common are zeros, spikes, negative values, and extreme values? | **Target distribution and segment summary** |
| 6 | **Missingness** | Which fields are missing, when, and for which stores or products? Is missingness random or operationally meaningful? | **Missingness map** |
| 7 | **Seasonality and dynamics** | Are there day-of-week, weekly, monthly, holiday, trend, or regime effects? Does behavior differ by store or product? | **Seasonal and temporal profiles** |
| 8 | **Cross-sectional heterogeneity** | Do a few stores or products dominate volume? Which segments are intermittent, volatile, sparse, or stable? | **Store-product segmentation** |
| 9 | **Leakage and availability** | Would every feature be available at the forecast origin? Does any feature use future sales, future aggregation, or post-decision information? | **Leakage audit** |
| 10 | **Baseline signal** | Can simple rules - naive, seasonal-naive, or moving average - already capture most structure? | **Baseline benchmark** |
| 11 | **Decision relevance** | How would errors translate into stockouts, overstock, waste, or service-level changes? | **Error-to-cost assumptions** |
| 12 | **Reproducibility** | Can someone rerun the EDA and obtain the same tables and figures? | **Parameterized EDA report** |

## Minimum completion standard

| Check | Complete when the EDA... |
|---|---|
| Decision | States the grain and decision context. |
| Documentation | Documents schema, provenance, units, and identifiers. |
| Data quality | Reports integrity, coverage, target behavior, and missingness. |
| Behavior | Visualizes temporal patterns and cross-sectional differences. |
| Leakage | Identifies availability and leakage risks at the forecast origin. |
| Benchmark | Compares simple forecasting baselines. |
| Business relevance | Records error-to-cost and service-level assumptions. |
| Reproducibility | Exports tables, figures, parameters, and source references. |

## FreshRetailNet-specific cautions

| Field or topic | Interpretation and caution |
|---|---|
| `sale_amount` | Normalized daily sales. Do not interpret it as raw units without the publisher's coefficient. |
| `hours_sale` | Hourly sales sequence that may contain realized target information. |
| `stock_hour6_22_cnt` | Operational stockout count. Same-day future values may leak information. |
| `hours_stock_status` | Hourly stockout-status sequence. Use only the portion observable at the forecast origin. |
| Encoded IDs | City, store, category, and product IDs are identifiers, not continuous measurements. |
| Holiday/activity/discount | Use only when known or scheduled before the forecast origin. |
| Weather fields | Use historical lags or a weather forecast that was available at the forecast origin. |

## Recommended report order

| # | Section | Main output |
|---:|---|---|
| 1 | Decision and grain | Data-grain statement |
| 2 | Schema and provenance | Data dictionary |
| 3 | Structural integrity and coverage | Integrity report and coverage calendar |
| 4 | Target behavior and missingness | Target summary and missingness map |
| 5 | Seasonality and dynamics | Seasonal and temporal profiles |
| 6 | Store-product heterogeneity | Segmentation and concentration tables |
| 7 | Leakage and availability | Leakage audit |
| 8 | Baseline benchmark | Naive, seasonal-naive, and moving-average scores |
| 9 | Decision relevance | Error-to-cost assumptions |
| 10 | Reproducibility | Parameterized report and exported artifacts |
