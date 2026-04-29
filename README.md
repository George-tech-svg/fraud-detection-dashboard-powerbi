# Fraud Detection Dashboard – Mobile Money Transaction Analysis

## Live Dashboard Preview
![Dashboard Screenshot](screenshots/dashboard_overview.png)

## Business Problem
Mobile money platforms lose millions annually to fraud. This dashboard helps fraud analysts identify high-risk transaction patterns, prioritize alerts, and reduce false positives.

## Dataset
- Source: PaySim Financial Simulation (Kaggle)
- Size: 6.3 million transactions
- Fraud cases: Approximately 0.13 percent (realistic imbalance)
- Time period: Simulated 30 days (one step equals one hour)

## Tools Used
- Power BI Desktop
- Power Query (M language for cleaning)
- DAX (measures and calculated columns)

## Dataset Columns

### Original Columns
| Column Name | Description |
|-------------|-------------|
| step | Time step (1 step = 1 hour of simulation) |
| type | Transaction type (PAYMENT, TRANSFER, CASH_OUT, CASH_IN, DEBIT) |
| amount | Transaction amount |
| nameOrig | Customer who initiated the transaction |
| oldbalanceOrg | Customer balance before transaction |
| newbalanceOrig | Customer balance after transaction |
| nameDest | Recipient of the transaction |
| oldbalanceDest | Recipient balance before transaction |
| newbalanceDest | Recipient balance after transaction |
| isFraud | 1 = fraudulent transaction, 0 = legitimate |
| isFlaggedFraud | 1 = system flagged as fraud, 0 = not flagged |

### Calculated Columns Created
| Column Name | Description |
|-------------|-------------|
| BalanceMismatch | Flags when balances do not add correctly (Error or OK) |
| HourOfDay | Extracted hour from step column (0-23) |
| TimeCategory | Morning, Afternoon, Evening, Night |
| AmountCategory | Micro, Small, Medium, Large based on transaction amount |
| Risk Score | High Risk, Medium Risk, Low Risk based on amount and transaction type |

## Data Cleaning Steps in Power Query
1. Filtered to high-risk transaction types (TRANSFER and CASH_OUT only)
2. Removed rows where amount is zero or greater than 10 million
3. Created BalanceMismatch flag to detect errors
4. Replaced missing recipient names with "Merchant"
5. Removed unnecessary columns (oldbalanceDest, newbalanceDest)
6. Created HourOfDay column from step
7. Created TimeCategory column (Morning, Afternoon, Evening, Night)
8. Created AmountCategory column (Micro, Small, Medium, Large)

## Key DAX Measures

### Base Measures
**Total Transactions**
Total Transactions = COUNTROWS(Transaction_Data)

text

**Total Fraud**
Total Fraud = CALCULATE([Total Transactions], Transaction_Data[isFraud] = TRUE)

text

**Fraud Rate Percentage**
Fraud Rate % = DIVIDE([Total Fraud], [Total Transactions]) * 100

text

**Total Fraud Amount**
Total Fraud Amount = CALCULATE(SUM(Transaction_Data[amount]), Transaction_Data[isFraud] = TRUE)

text

**Average Transaction Amount**
Avg Transaction Amount = AVERAGE(Transaction_Data[amount])

text

**Average Fraud Amount**
Avg Fraud Amount = AVERAGEX(FILTER(Transaction_Data, Transaction_Data[isFraud] = TRUE), Transaction_Data[amount])

text

**Total Transaction Volume**
Total Volume = SUM(Transaction_Data[amount])

text

### Calculated Columns

**Balance Mismatch Flag**
BalanceMismatch = if [oldbalanceOrg] - [newbalanceOrig] <> [amount] then "Error" else "OK"

text

**Hour of Day**
HourOfDay = Number.Mod([step], 24)

text

**Time Category**
TimeCategory =
SWITCH(TRUE(),
[HourOfDay] >= 6 && [HourOfDay] <= 11, "Morning",
[HourOfDay] >= 12 && [HourOfDay] <= 16, "Afternoon",
[HourOfDay] >= 17 && [HourOfDay] <= 21, "Evening",
"Night"
)

text

**Amount Category**
AmountCategory =
SWITCH(TRUE(),
[amount] <= 100, "Micro",
[amount] <= 500, "Small",
[amount] <= 2000, "Medium",
"Large"
)

text

**Risk Score**
Risk Score =
SWITCH(TRUE(),
[amount] > 200000 && [type] = "TRANSFER", "High Risk",
[amount] > 50000 && [type] = "TRANSFER", "Medium Risk",
[amount] > 200000 && [type] = "CASH_OUT", "Medium Risk",
[isFraud] = TRUE, "High Risk",
"Low Risk"
)

text

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
- Risk Score Filter: High Risk, Medium Risk, Low Risk (checkbox style)
- Time Category Filter: Morning, Afternoon, Evening, Night

### Visualizations Included
| Visual | Purpose |
|--------|---------|
| Fraud Cases by Transaction Type (Bar Chart) | Compares fraud volume between CASH_OUT and TRANSFER |
| Fraud Attempts Over Time (Line Chart) | Shows fraud patterns across simulated hours |
| Fraud Rate by Hour (Column Chart) | Shows which hours have highest fraud rate |
| Normal Transaction Amount Distribution (Histogram) | Distribution of legitimate transaction amounts |
| Fraud Transaction Amount Distribution (Histogram) | Distribution of fraudulent transaction amounts |
| Balance Mismatch Analysis (Table) | Shows transactions with balance errors |
| Risk Score Table (Table) | Summary of fraud risk by risk level |

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
4. Use the Risk Score slicer to filter by risk level
5. Hover over any visual for detailed tooltips

## License
This project uses the PaySim dataset for portfolio purposes only. The dataset license allows non-commercial use.

## Author
George Onyango Ochieng

Data Analytics | Python Programming | Machine Learning | ICT

Specializing in fraud detection, fintech analytics, and interactive dashboard development using Power BI, Python, and machine learning algorithms.

## Certifications

| Course | Certificate Link |
|--------|------------------|
| Data Analytics | [View Certificate](https://savanna.alxafrica.com/certificates/T95s3SPMxZ) |
| Data Science | [View Certificate](https://savanna.alxafrica.com/certificates/flJS2ZXs6r) |
| Professional Foundations | [View Certificate](https://savanna.alxafrica.com/certificates/RYz9rB28SJ) |
| Python Programming | [View Certificate](https://savanna.alxafrica.com/certificates/Ee8x6JfGCh) |
| Machine Learning | [View Certificate](https://savanna.alxafrica.com/certificates/7zsMrEN5m2) |

## Contact Me

- **Call or Text:** +254115136359
- **WhatsApp:** +254111866769
- **Email:** georgebabji1220@gmail.com
- **Upwork:** [View Profile](https://upwork.com/freelancers/~01ea729f5447c9e73b)
- **LinkedIn:** [Connect with me](https://www.linkedin.com/in/george-onyango-5a5906360/)
- **GitHub:** [View my projects](https://github.com/George-tech-svg)
