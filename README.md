# Enterprise Data Reconciliation

An end-to-end Data Analytics project developed as part of the iStudio Data Analytics Internship.

This project integrates customer account records, application server logs, and daily EUR-to-USD exchange rates to identify successful transactions belonging to customers whose latest available account status is Active. The reconciled data is used to calculate USD revenue and develop a CFO dashboard.

## Project Objectives

- Identify customers using their most recent account status.
- Extract successful transactions from unstructured server logs using Regular Expressions.
- Exclude failed transaction events.
- Handle missing exchange rates using forward-filling.
- Reconcile transaction, customer, and exchange-rate records.
- Calculate total revenue in USD.
- Analyse monthly revenue and identify the five highest-spending customers.

## Technology Stack

| Technology | Purpose |
|---|---|
| Python 3.12.10 | Data processing and analysis |
| Pandas | Data cleaning, transformation and analysis |
| SQLite / SQL | Customer database extraction |
| Regular Expressions | Transaction data extraction |
| Matplotlib | Data visualization |
| Seaborn | Visualization styling |
| Jupyter Notebook | Project development and reporting |

## Dataset Description

The assessment uses three source datasets supplied by iStudio:

| Dataset | Description |
|---|---|
| `enterprise_database.db` | Customer account information and historical status records |
| `server_logs.txt` | Application server events containing successful and failed transactions |
| `daily_exchange_rates.csv` | Daily EUR-to-USD exchange-rate information |

The original datasets are not included in this repository. They must be obtained through an authorized source before executing the notebook.

## Project Workflow

### Phase 1 — SQL Customer Extraction

Used SQLite and the `ROW_NUMBER()` window function to identify each customer's latest account status.

**Output:** `df_active_users`

Results:
- Database records: 47,464
- Unique customers: 25,000
- Active customers: 14,183
- Duplicate active customer IDs: 0

### Phase 2 — Regex and Transaction Extraction

Processed 100,000 server-log events using Pandas string operations and Regular Expressions.

**Output:** `extracted_data`

Results:
- Total log records: 100,000
- Successful INFO events: 94,902
- Failed ERROR events excluded: 5,098
- Parsing failures: 0
- Unique transaction customers: 24,466
- Unique products: 900

### Phase 3 — Financial Reconciliation and Merging

Integrated successful transactions with exchange rates and active customer records.

Missing published exchange rates were handled using forward-filling, preserving the original exchange-rate values in a separate column.

**Output:** `df_final`

Results:
- Transactions matched to exchange-rate calendar: 94,902
- Transactions on dates with missing published rates: 26,984
- Unmatched transaction dates: 0
- Transactions belonging to active customers: 53,899
- Transactions excluded for inactive customers: 41,003
- Active customers with transactions: 13,888
- Total reconciled USD revenue: **$148,274,622.91**

### Phase 4 — CFO Dashboard

Developed two financial visualizations:

1. **Total USD Revenue by Month** — monthly revenue trends across 2023.
2. **Top 5 Most Valuable Customers** — customers ranked by total USD spending.

Both visualizations were developed using Matplotlib and Seaborn.

## Key Business Findings

- Total reconciled USD revenue: **$148,274,622.91**.
- January recorded the highest monthly revenue: **$12,841,761.38**.
- February recorded the lowest monthly revenue: **$11,436,130.90**.
- Customer `U-06582` ranked first with total spending of **$40,050.99**.
- The top five customers generated combined spending of **$196,415.85**.

## Data Quality and Limitations

- The supplied exchange-rate file contains 104 missing published rates: 52 Fridays and 52 Saturdays.
- Forward-filling was applied using the most recent previously published rate. This is an analytical assumption, not independent verification of the official financial-market rate for each missing date.
- The latest customer account-status update in the supplied database is February 28, 2023.
- Customer classifications therefore reflect the latest status available in the supplied database, not independently verified present-day statuses.
- The analysis uses the original assessment datasets and their documented limitations.

## Performance Testing

The complete analytical execution was measured at **5.54 seconds** in the recorded verification run, against the iStudio requirement of under 120 seconds.

## How to Run the Project

### Requirements

- Python 3.12 or a compatible version
- Visual Studio Code
- Python and Jupyter extensions

### Install Dependencies

```bash
python -m pip install pandas matplotlib seaborn ipykernel
```

### Prepare the Data Files

Place the three authorized source datasets in the same directory as the notebook:

```text
Enterprise_Data_Reconciliation/
│
├── enterprise_database.db
├── server_logs.txt
├── daily_exchange_rates.csv
└── Enterprise_Data_Reconciliation_Final.ipynb
```

### Execute the Notebook

1. Open `Enterprise_Data_Reconciliation_Final.ipynb` in VS Code.
2. Select the appropriate Python kernel.
3. Restart the kernel.
4. Select **Run All**.
5. Review the four phases, validation outputs and CFO dashboard.

## Final Deliverable

**Notebook:** `Enterprise_Data_Reconciliation_Final.ipynb`

The completed notebook contains the project overview, four analytical phases, SQL and Regex processing, financial reconciliation, validation, dashboard visualizations, business insights and conclusions.

---

**Project Title:** Enterprise Data Reconciliation  
**Internship:** iStudio Data Analytics Internship  
**Domain:** Data Analytics and Financial Reconciliation  
**Developer:** Baliji Mohan
