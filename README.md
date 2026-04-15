# STAT 301 Group Project: Supply Chain ANOVA Analysis

## Introduction
The objective of this project is to apply **Analysis of Variance (ANOVA)** methods to explore a supply chain dataset. This analysis identifies if specific categorical factors significantly impact key performance metrics like costs, lead times, and defect rates.

### Research Questions & Hypotheses
We are currently evaluating the following four research questions:

#### 1. One-Way ANOVA: Product Type vs. Manufacturing Cost
- **Research Question:** Does the Product Type (Skincare, Haircare, or Cosmetics) significantly affect the mean Manufacturing Costs?
- **Null Hypothesis ($H_0$):** The mean manufacturing cost is the same for Skincare, Haircare, and Cosmetics.
- **Alternative Hypothesis ($H_a$):** At least one product type has a different mean manufacturing cost.

#### 2. Two-Way ANOVA: Shipping Carrier & Location Interaction (⚠️ Unbalanced Design)
- **Research Question:** How do Shipping Carrier and Location interact to affect Shipping Costs?
- **Null Hypothesis ($H_0$ - Interaction):** The effect of the Carrier on cost does not depend on the Location.
- **Alternative Hypothesis ($H_a$ - Interaction):** A significant interaction exists between Carrier and Location.
- **⚠️ Design Note:** The dataset is **unbalanced** for this test (unequal sample sizes across carrier-location combinations). Type II sums of squares were used instead of Type I to ensure valid hypothesis testing.

#### 3. One-Way ANOVA: Supplier vs. Lead Time
- **Research Question:** Does the mean lead time differ significantly by supplier?
- **Null Hypothesis ($H_0$):** The mean lead time is the same for all suppliers.
- **Alternative Hypothesis ($H_a$):** At least one supplier has a different mean lead time.

#### 4. Two-Way ANOVA: Transportation Mode & Product Type Interaction
- **Research Question:** Do transportation mode and product type interact to affect defect rates?
- **Null Hypothesis ($H_0$ - Interaction):** There is no interaction between transportation mode and product type regarding defect rates.
- **Alternative Hypothesis ($H_a$ - Interaction):** The effect of transportation mode on defect rates depends on the product type.

---

## Data Source & Processing
- **Source:** Data retrieved from [Kaggle Supply Chain Analysis](https://www.kaggle.com/code/amirmotefaker/supply-chain-analysis/input).
- **Cleaning:** We clarified that "Shipping costs" refers only to the carrier fee, while "Total Costs" includes manufacturing, shipping, and handling.
- **Renaming Log:**
  - `Lead times` → `Supplier_Lead_Time`
  - `Lead time` → `Order_Lead_Time`
  - `Manufacturing lead time` → `Manufacturing_Lead_Time`
  - `Costs` → `Total_Costs`

---

## Methods
- **Statistical Tools:** We are using **JMP** for initial distribution analysis and **R** for executing the formal statistical tests and ANOVA modeling.
- **Exploratory Data Analysis (EDA):** We utilized JMP Graph Builder to visualize the distribution of categorical and numerical variables to ensure data quality before testing.
- **Project Tracking:** All tasks, documentation, and milestones are managed via **Jira** (Project Key: `S3FP`).
- **Reasoning:** ANOVA is the appropriate test here as we are comparing the means of three or more groups (Product Types, Carriers, Suppliers, etc.) to determine statistical significance.

### ⚠️ Important: Handling Unbalanced Design (Question 2)

The two-way ANOVA for Question 2 (Shipping Carrier × Location) was found to be **unbalanced**. Evidence of imbalance:

| Carrier | Bangalore | Chennai | Delhi | Kolkata | Mumbai | Total |
|---------|-----------|---------|-------|---------|--------|-------|
| Carrier A | 6 | 7 | 5 | 6 | 4 | 28 |
| Carrier B | 7 | 8 | 6 | 10 | 12 | 43 |
| Carrier C | 5 | 5 | 4 | 9 | 6 | 29 |
| **Total** | 18 | 20 | 15 | 25 | 22 | 100 |

**Why this matters:** In a balanced design, each of the 15 cells would have ~6.67 observations. Here, cell counts range from 4 to 12. Using default Type I sums of squares (`aov()`) would produce order-dependent results.

**Solution implemented:** Type II sums of squares were used via the `Anova()` function from the `car` package in R:

```r
library(car)
model <- lm(Shipping.costs ~ Shipping.carriers * Location, data = Cleaned_df)
Anova(model, type = "II")
