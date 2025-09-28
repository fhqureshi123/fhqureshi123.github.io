Banking Analytics Dashboard
📊 Project Overview

This project demonstrates a complete end-to-end Power BI solution built on a simulated banking dataset. It showcases how raw flat-file data can be transformed into a star schema model, enriched with DAX measures, and presented through interactive dashboards to deliver real business value.

The dataset (Comprehensive_Banking_Database.csv) includes 5,000 records and 40 attributes covering customers, accounts, transactions, loans, credit cards, and feedback.

The final Power BI dashboard (Banking Analytics Dashboard.pbix) enables executives and analysts to answer critical business questions such as:

How many active customers, accounts, and loans do we have?

What’s the loan approval rate and risk profile?

How are credit cards utilized across branches?

What are the feedback trends and resolution performance?

Which transactions are anomalous or high-risk?

![Banking Analytics Dashboard](assets/images/dashboard.png)

🚀 Features
Data Preparation (Power Query)

Cleaned and profiled the dataset (missing values, duplicates, anomalies).

Split flat file into logical tables: Customers, Accounts, Transactions, Loans, Cards, Feedback.

Standardized columns and applied transformations for time intelligence.

Data Modeling (Star Schema)

Dimensions: Customer, Branch, Account, Loan, Credit Card, Feedback, Date.

Facts: Transactions, Loan Applications, Credit Card Payments, Feedback Interactions.

Relationships defined with 1-to-many joins for scalability and performance.

Business KPIs (DAX Measures)

Customer KPIs: Active vs Inactive, Churn Risk, High-value segmentation.

Accounts: Total/Avg Balance, Net Flow.

Loans: Approval Rate %, Outstanding Portfolio, Interest Revenue Potential.

Credit Cards: Utilization %, Overdue %, Rewards.

Feedback: Resolution %, Avg Resolution Time.

Risk: % of Anomalous Transactions.

Visualizations & UX

Seven dashboards designed with a banking-style theme (Navy, Teal, White):

Executive Summary – snapshot KPIs.

Customer Insights – demographics, high-value segments.

Accounts & Transactions – balances, inflow/outflow.

Loans Dashboard – portfolio, approvals, risk.

Credit Cards Dashboard – utilization, overdue analysis.

Feedback & Service – complaints and resolution performance.

Risk & Anomalies – anomaly detection and branch-level red flags.

🎨 Design Standards

Color Palette:

Navy (#0B1D3A) – Professional base

Teal/Green (#2A9D8F) – Financial highlights

Red (#E76F51) – Risks/alerts

Amber (#FFB703) – Feedback/service

Blue (#3A86FF) – Growth indicators

UI Guidelines:

KPI cards at the top for clarity.

Trend charts in the middle.

Comparison visuals (bar/donut/pie) to the right.

Detailed tables/matrices at the bottom.

Interactive slicers for Date, Branch, and Account Type.

🔒 Security (Dynamic RLS)

Role-Level Security (RLS) implemented using a parameterized access table:

RLS_AccountAccess =
DATATABLE(
    "Email", STRING,
    "Account Type", STRING,
    {
        {"savings.manager@bank.com", "Savings"},
        {"current.manager@bank.com", "Current"},
        {"ceo@bank.com", "Savings"},
        {"ceo@bank.com", "Current"}
    }
)


This ensures managers only see the accounts they are authorized to access.

📂 Repository Structure
├── Banking Analytics Dashboard.pbix      # Power BI dashboard file
├── Comprehensive_Banking_Database.csv    # Source dataset
├── Power BI Project End to End Hari.pdf  # Documentation & training material
└── README.md                             # Project documentation (this file)

💡 Business Value

Provides executives with one-stop insights into customer, financial, and operational KPIs.

Helps risk teams detect anomalous transactions and high-risk loans.

Empowers customer service to improve complaint resolution times.

Demonstrates best practices in Power BI (data modeling, DAX, UX).

📥 Getting Started

Clone this repository.

Open Banking Analytics Dashboard.pbix in Power BI Desktop.

Explore dashboards using slicers and filters.

Extend with your own datasets or connect to live banking systems.

🏆 Author

Fassahat Ullah Qureshi

Emergi Mentors Profile

LinkedIn
