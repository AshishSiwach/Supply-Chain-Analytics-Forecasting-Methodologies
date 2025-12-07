# 📦 Supply Chain Demand Forecasting & Cost Optimization
<img width="2752" height="1536" alt="image" src="https://github.com/user-attachments/assets/be768bb1-feb2-4ee3-9ac9-5b7af417ca97" />

## 📊 Project Overview
This project focuses on optimizing supply chain operations by forecasting weekly demand to minimize inventory and capacity costs. By analyzing historical daily demand data, this analysis evaluates different time-series forecasting methodologies—specifically Exponential Smoothing (SES) and Holt’s Linear Trend—to determine the optimal parameters for distinct business constraints.
Key Objectives:

• Aggregate and clean high-frequency daily data into actionable weekly time steps.

• Identify seasonality, trends, and anomalies in market demand.

• Minimize the Variance of Forecast Errors (Inventory Focus).

• Minimize the Variance of Forecast Volatility (Capacity Focus).

--------------------------------------------------------------------------------
## 🛠️ Data Processing & Engineering
Raw Data: Daily demand records requiring aggregation to smooth volatility and align with weekly planning cycles.
Methodology:

• Temporal Aggregation: Converted daily demand to weekly demand by establishing a "weekday" reference point. Used a Wednesday (weekday = 3) cutoff based on the availability of prior 7-day historical data.

• Logic Implementation: Utilized conditional summation logic (IF(WEEKDAY(date)=3, SUM(previous_7_days), 0)) to generate a clean weekly demand feature while filtering out null values.

• Data Cleaning: Separated non-zero aggregates into a dedicated dataset for analysis, preparing the "Demand" variable for the Forecast Explorer application.

--------------------------------------------------------------------------------
## 🔍 Exploratory Data Analysis (EDA)
Statistical analysis of the time series revealed distinct patterns critical for model selection:

• Trend & Seasonality: The daily demand remained fairly constant (200–300 units) but exhibited specific seasonality, notably downward spikes in April 2024 and April 2025.

• Anomalies: A significant drop in demand was detected in January 2025, indicating a major market anomaly.

• Distribution: Aggregating to weekly data smoothed high-frequency noise, revealing a "fairly linear" overall trend despite the seasonal outliers.

--------------------------------------------------------------------------------
## 🧠 Modeling Strategy & Parameter Tuning
The forecasting strategy was divided into three business scenarios, optimizing for specific cost drivers.

Scenario A: Minimizing Inventory Costs

• Objective: Minimize the variance of the sum of forecast errors (preventing stockouts/overstock).

• Model Selected: Exponential Smoothing (SES).

• Why SES? The data presented a linear trend. SES captures the level component efficiently with a single parameter (α), keeping forecasts stable regardless of lead time.

• Optimization:

    ◦ Tuning α closer to 0 minimized variance.
    
    ◦ Optimal Parameters: α=0.031, Lead Time = 0
    
    ◦ Result: Achieved a Bias of 0.017 and minimized the variance of sum errors to 26,353.
    
Scenario B: Minimizing Capacity Costs

• Objective: Minimize the variance of the forecast itself (reducing the need for costly capacity adjustments).

• Model Selected: Holt’s Linear Trend Method.

• Why Holt's? SES was rejected for this scenario because increasing α to track changes drastically increased forecast variance. Holt’s method allowed for better control over variations in forecast and change in forecast.

• Optimization:

    ◦ Optimal Parameters: α=0.0071, β=0.05, Lead Time = 3.
    
    ◦ Result: Reduced the variance of L+1 periods ahead forecast to 138 (compared to >500 in other scenarios) while maintaining a low bias of 0.063.
    
Scenario C: Balanced Approach (Inventory + Capacity)

• Strategy: Recognized that inventory cost variations (magnitude ~26,000) significantly outweighed capacity cost variations.

• Decision: Reverted to Exponential Smoothing with Scenario A parameters (α=0.031).

• Business Justification: This method provides the best trade-off. It prioritizes the high-risk inventory metrics while still keeping capacity metrics within an optimized range compared to higher alpha values.

## 🚀 Business Impact & Recommendations

<img width="880" height="356" alt="image" src="https://github.com/user-attachments/assets/5f2cf716-15b6-457d-9d51-2fc8a149b443" />

--------------------------------------------------------------------------------
## 💻 Tools & Tech Stack

• Data Analysis: Microsoft Excel (Advanced Formulas, Statistical functions).

• Visualization: Box plots, Line charts for trend analysis and descriptive statistics.


