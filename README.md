# Fraud Detection Dashboard – PaySim Mobile Money Analysis

## Live Dashboard Preview
![Dashboard Screenshot](screenshots/dashboard_overview.png)

## Business Problem
Mobile money platforms lose millions annually to fraud. This dashboard helps fraud analysts identify high-risk transaction patterns, prioritize alerts, and track fraud trends over time.

## Dataset
- Source: PaySim Financial Simulation (Kaggle)
- Size: 6.3 million transactions
- Fraud cases: Approximately 0.13 percent (realistic imbalance)
- Time period: Simulated 30 days (one step equals one hour)

## Tools Used
- Power BI Desktop
- Power Query (M language for cleaning)
- DAX (measures and calculated columns)

## Data Cleaning Steps in Power Query
1. Filtered to high-risk transaction types (TRANSFER and CASH_OUT only)
2. Removed rows where amount is zero or greater than 10 million
3. Created BalanceMismatch flag to detect errors
4. Replaced missing recipient names with "Merchant"
5. Removed unnecessary columns (oldbalanceDest, newbalanceDest)

## Key DAX Measures

Total Transactions = COUNTROWS('PS_20174392719_1491204439457_log')

2. Total Fraud = CALCULATE([Total Transactions], [isFraud] = TRUE)

3. Fraud Rate % = DIVIDE([Total Fraud], [Total Transactions], 0) * 100

5. Total Fraud Amount = CALCULATE(SUM([amount]), [isFraud] = TRUE)

Risk Score =
SWITCH(TRUE(),
[amount] > 200000 && [type] = "TRANSFER", "High Risk",
[amount] > 50000, "Medium Risk",
"Low Risk"
)


## Dashboard Components

### Key Metrics Cards (Top Row)
| Card | Value |
|------|-------|
| Total Transactions | 3 Million |
| Total Fraud Cases | 8 Thousand |
| Total Fraud Amount | 12.06 Billion |
| Fraud Rate | 0.30 Percent |

### Interactive Filters (Slicers)
- Transaction Type Filter: CASH_OUT and TRANSFER (dropdown style)
- Risk Score Filter: High Risk and Low Risk (checkbox style)

### Visualizations Included
| Visual | Purpose |
|--------|---------|
| Fraud Cases by Transaction Type (Bar Chart) | Compares fraud volume between CASH_OUT and TRANSFER |
| Fraud Attempts Over Time (Line Chart) | Shows fraud patterns across simulated hours |
| Normal Transaction Amount Distribution (Histogram) | Distribution of legitimate transaction amounts |
| Fraud Transaction Amount Distribution (Histogram) | Distribution of fraudulent transaction amounts |

## Dashboard Insights
- TRANSFER transactions contain significantly more fraud cases than CASH_OUT
- High Risk transactions (amount above 200,000) show elevated fraud rates
- Fraud attempts follow a pattern across simulated time steps

## Files in this Repository
| File | Description |
|------|-------------|
| dashboard.pbix | Power BI file (open with Power BI Desktop) |
| fraud_dashboard.pdf | PDF export for users without Power BI |
| power_query/cleaning_steps.txt | M code for data transformation |
| dax_measures/measures.txt | All DAX formulas |
| data/sample_100_rows.csv | Sample data (first 100 rows only) |

## How to Use This Dashboard
1. Download the dashboard.pbix file
2. Open with Power BI Desktop (free version works)
3. Use the Transaction Type slicer to filter by CASH_OUT or TRANSFER
4. Use the Risk Score slicer to filter by High Risk or Low Risk
5. Hover over any visual for detailed tooltips

## License
This project uses the PaySim dataset for portfolio purposes only. The dataset license allows non-commercial use.

## Author
George Onyango Ochieng

Data Analyst | Python & Machine Learning | Power BI Developer

Certifications: Data Analytics, Python Programming, Machine Learning, ICT Professional Foundation

Available for freelance data analytics and dashboard development.
## Connect with Me
- Upwork Profile: https://www.upwork.com/freelancers/~01ea729f5447c9e73b
- LinkedIn: https://www.linkedin.com/in/george-onyango-5a5906360/
- GitHub: https://github.com/George-tech-svg/fraud-detection-dashboard-powerbi
