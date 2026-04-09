# STAT301-SupplyChain-ANOVA
An ANOVA-based analysis of supply chain data to evaluate the impact of product types, carriers, and locations on manufacturing and shipping costs for STAT 301
# STAT 301 Group Project: Supply Chain ANOVA Analysis

## Introduction
The objective of this project is to apply **Analysis of Variance (ANOVA)** methods to explore a supply chain dataset. This analysis identifies if specific categorical factors significantly impact key performance metrics like costs, lead times, and defect rates.

### Research Questions & Hypotheses
We are currently evaluating the following four research questions:

#### 1. One-Way ANOVA: Product Type vs. Manufacturing Cost
* **Research Question:** Does the Product Type (Skincare, Haircare, or Cosmetics) significantly affect the mean Manufacturing Costs?
* **Null Hypothesis ($H_0$):** The mean manufacturing cost is the same for Skincare, Haircare, and Cosmetics.
* **Alternative Hypothesis ($H_a$):** At least one product type has a different mean manufacturing cost.

#### 2. Two-Way ANOVA: Shipping Carrier & Location Interaction
* **Research Question:** How do Shipping Carrier and Location interact to affect Shipping Costs?
* **Hypotheses:** * **$H_0$ (Interaction):** The effect of the Carrier on cost does not depend on the Location.
    * **$H_a$ (Interaction):** A significant interaction exists between Carrier and Location.

#### 3. One-Way ANOVA: Supplier vs. Lead Time
* **Research Question:** Does the mean lead time differ significantly by supplier?
* **Null Hypothesis ($H_0$):** The mean lead time is the same for all suppliers.
* **Alternative Hypothesis ($H_a$):** At least one supplier has a different mean lead time.

#### 4. Two-Way ANOVA: Transportation Mode & Product Type Interaction
* **Research Question:** Do transportation mode and product type interact to affect defect rates?
* **Hypotheses:** * **$H_0$ (Interaction):** There is no interaction between transportation mode and product type regarding defect rates.
    * **$H_a$ (Interaction):** The effect of transportation mode on defect rates depends on the product type.

---

## Data Source & Processing
* **Source:** Data retrieved from [Kaggle Supply Chain Analysis](https://www.kaggle.com/code/amirmotefaker/supply-chain-analysis/input).
* **Cleaning:** We clarified that "Shipping costs" refers only to the carrier fee, while "Total Costs" includes manufacturing, shipping, and handling.
* **Renaming Log:** * `Lead times` → `Supplier_Lead_Time`
    * `Lead time` → `Order_Lead_Time`
    * `Manufacturing lead time` → `Manufacturing_Lead_Time`
    * `Costs` → `Total_Costs`

---

## Methods
* **Statistical Tools:** We are using **JMP** for initial distribution analysis and **R** for executing the formal statistical tests and ANOVA modeling.
* **Exploratory Data Analysis (EDA):** We utilized JMP Graph Builder to visualize the distribution of categorical and numerical variables to ensure data quality before testing.
* **Project Tracking:** All tasks, documentation, and milestones are managed via **Jira** (Project Key: `S3FP`).
* **Reasoning:** ANOVA is the appropriate test here as we are comparing the means of three or more groups (Product Types, Carriers, Suppliers, etc.) to determine statistical significance.

---

## Conclusion & Findings
*(To be updated after finalizing which questions to keep and running the tests in R)*

### Statistical Results
| Test Case | Test Statistic (F) | P-Value | Decision ($\alpha=0.05$) |
| :--- | :--- | :--- | :--- |
| **Q1: Product vs Cost** | [TBD] | [TBD] | [TBD] |
| **Q2: Carrier/Location** | [TBD] | [TBD] | [TBD] |
| **Q3: Supplier Lead Time** | [TBD] | [TBD] | [TBD] |
| **Q4: Defect Rates** | [TBD] | [TBD] | [TBD] |

### Interpretations & Insights
* **Key Finding:** [To be filled from JMP/R analysis]
* **Managerial Insight:** [To be determined based on results]
* **Limitations:** [Discuss any data constraints or sampling biases]

---

## Project Info
* **Course:** STAT 301
* **Working Session:** April 17th
* **Submission Date:** April 27th, 11:59 PM
