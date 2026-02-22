# Loan Performance Dashboard
## Project Overview

This project analyzes a realistic loan approval dataset to evaluate portfolio performance, approval behavior, and credit risk exposure.

The dashboard focuses on answering:

* What drives loan approval?
* Where is portfolio risk concentrated?
* How do income and credit factors influence loan size?

The goal was to build a structured, decision-oriented dashboard rather than a purely descriptive one.

---

## Data Source

Dataset sourced from Kaggle:
[https://www.kaggle.com/datasets/parthpatel2130/realistic-loan-approval-dataset-us-and-canada](https://www.kaggle.com/datasets/parthpatel2130/realistic-loan-approval-dataset-us-and-canada)

The data includes customer demographics, credit attributes, loan details, and approval outcomes.

For development efficiency, a reduced sample of the dataset was used. The analytical logic remains consistent with the full dataset.

---

## Methodology

### Data Preparation

* Converted numeric text fields into proper numeric types.
* Cleaned and standardized categorical values.
* Created calculated fields to enhance analysis.

### Key Calculations

**Approval Rate**

```DAX
Approval Rate = 
VAR ApprovedCount =
    CALCULATE(
        COUNTROWS('Table1'),
        'Table1'[Loan Status Text] = "Approved"
    )

VAR TotalCount =
    COUNTROWS('Table1')

RETURN
IF(
    TotalCount = 0,
    BLANK(),
    DIVIDE(ApprovedCount, TotalCount)
)
```

**Portfolio Risk Exposure**

Clients are classified as higher risk if:

* Credit Score < 600
* Debt-to-Income Ratio > 40%
* Previous Defaults exist

```DAX
Risk Flag =
IF(
    VALUE(Table1[credit_score]) < 600
    || VALUE(Table1[debt_to_income_ratio]) > 0.4
    || VALUE(Table1[defaults_on_file]) > 0,
    1,
    0
)
```

```DAX
Portfolio Risk Exposure % =
DIVIDE(
    CALCULATE(COUNTROWS(Table1), Table1[Risk Flag] = 1),
    COUNTROWS(Table1)
)

The 600 credit score threshold was identified after testing approval behavior across score bands.

## Dashboard Design

The layout follows analytical hierarchy:

1. **Primary KPIs** – Total Applications, Approval Rate, Avg Loan Amount, Portfolio Risk Exposure
2. **Credit & Risk Analysis** – Approval trends and credit relationship
3. **Customer Insights** – Income and age segmentation
4. **Portfolio Composition** – Loan demand by purpose

Filters allow dynamic exploration of segments.

---

## Key Insights

* Approval rates increase sharply beyond credit score 600.
* A defined portion of the portfolio falls under elevated risk criteria.
* Higher income segments receive larger loan amounts.
* Risk concentration is strongest in low credit score and high DTI profiles.

##Recommendations

*Focus lending on applicants with credit score ≥ 600 to improve approval quality and portfolio stability.
*Apply stricter evaluation or adjusted pricing for applicants with DTI > 40% to manage risk exposure.
*Monitor risk concentration regularly to prevent elevated-risk segments from growing beyond acceptable levels.
---

## Assumptions & Limitations

* Risk thresholds are derived from observed dataset behavior.
* No longitudinal repayment history is available.
* A reduced dataset sample was used for development performance.

---

## Deliverables

* Tableau (online): https://public.tableau.com/views/loandashboardfinal/Template?:language=en-US&publish=yes&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link

## 🎨 Dashboard Sketch

<img src="sketch loan.png" width="900">

---

## 📊  Dashboard

<img src="dashboard final.png" width="1000">

note:  The analysis was originally developed in Power BI; however, due to public publishing restrictions requiring organizational credentials, the dashboard was rebuilt from scratch in Tableau Public to ensure full online accessibility without login.

For reference, a short Power BI demo version is also included in the repositery
