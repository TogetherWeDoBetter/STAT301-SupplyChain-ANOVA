
# STAT 301 Group Project — Supply Chain ANOVA Analysis

## Overview

This project applies **Analysis of Variance (ANOVA)** methods to a real-world supply chain dataset to investigate whether categorical operational factors significantly affect key performance metrics — specifically manufacturing costs and defect rates.

- **Dataset:** Kaggle Supply Chain Analysis dataset
- **Observations:** 100
- **Variables:** 24
- **Tools:** R (primary statistical analysis), JMP (exploratory distribution analysis)
- **Final Report:** `STAT301_FinalProject_Li_Stoven`

---

## Dataset

The dataset documents supply chain operations across five Indian cities: Bangalore, Chennai, Delhi, Kolkata, and Mumbai. It covers product categories, supplier performance, shipping logistics, manufacturing output, and quality outcomes.

**Key variable group sizes:**

| Variable | Groups |
|---|---|
| Product Type | Skincare (n=40), Haircare (n=34), Cosmetics (n=26) |
| Transportation Mode | Road (n=29), Rail (n=28), Air (n=26), Sea (n=17) |
| Shipping Carrier | Carrier B (n=43), Carrier C (n=29), Carrier A (n=28) |
| Location | Kolkata (n=25), Mumbai (n=22), Chennai (n=20), Bangalore (n=18), Delhi (n=15) |
| Supplier | Supplier 1 (n=27), Supplier 2 (n=22), Supplier 4 (n=18), Supplier 5 (n=18), Supplier 3 (n=15) |

---

## Data Cleaning

The raw dataset was cleaned in R before analysis. All steps are documented in `STAT301_Group_Project_Q1Q4.Rmd`.

| Step | Action | Result |
|---|---|---|
| Column renames | Resolved ambiguous lead time names | `Lead.times` → `Supplier_Lead_Time`, `Lead.time` → `Order_Lead_Time`, `Manufacturing.lead.time` → `manufacturing_lead_time`, `Costs` → `Total_Costs` |
| Missing values | `summarise_all(~sum(is.na(.)))` | None found |
| Duplicate rows | `duplicated()` | None found |
| Factor conversion | `mutate(as.factor())` | All 8 categorical variables converted |
| Numeric verification | `select(where(is.numeric))` | All numeric columns confirmed |
| Outlier detection | IQR-based function across all numeric columns | No outliers identified |
| **Final dataset** | `Cleaned_Supply_Chain_Data` | **100 observations, 24 variables retained** |

---

## Research Questions Evaluated

During the project, four research questions were explored before selecting the two strongest for the final report:

| # | Type | Question | Key Result | Selected? |
|---|---|---|---|---|
| Q1 | One-Way ANOVA | Does Product Type affect Manufacturing Costs? | F=0.370, p=0.692 — not significant | ✅ **Yes** |
| Q2 | Two-Way ANOVA | Do Shipping Carrier and Location interact to affect Shipping Costs? | Interaction p=0.055 — marginal, not significant at α=0.05 | ❌ Dropped |
| Q3 | One-Way ANOVA | Does Supplier affect Supplier Lead Time? | F=0.335, p=0.854 — not significant | ❌ Dropped |
| Q4 | Two-Way ANOVA | Do Transportation Mode and Product Type interact to affect Defect Rates? | Interaction p=0.014 — **significant** ✅ | ✅ **Yes** |

### Why Q1 and Q4 Were Selected

- **Q1** satisfies the one-way ANOVA requirement. The result is clean, the design is straightforward, and a non-significant finding is a valid and reportable conclusion.
- **Q4** is the strongest result in the project. The significant interaction (p=0.014) is statistically robust, all assumptions are met, and the finding has a clear and meaningful managerial interpretation.
- **Q1 and Q4 together tell a coherent story:** product type alone does not affect costs or defect rates, but its combination with transportation mode reveals a significant quality difference — demonstrating the value of testing interaction effects.
- **Q2 was dropped** because p=0.055 falls above α=0.05 and the conclusion would be ambiguous. A clear fail-to-reject decision could not be made at the required significance level.
- **Q3 was dropped** because p=0.854 is very far from significance and the result offered no analytical value.

---

## Final Analysis: Q1 and Q4

### Q1 — One-Way ANOVA: Product Type vs. Manufacturing Costs

**Research Question:** Does product type significantly affect manufacturing costs?

**Variables:**
- Independent variable: Product Type (categorical factor, 3 levels: cosmetics, haircare, skincare)
- Dependent variable: Manufacturing Costs (continuous numeric)

**Why one-way ANOVA:** Comparing means across 3 groups. Using 3 pairwise t-tests at α=0.05 would inflate Type I error — overall confidence drops from 95% to ~85.7%. ANOVA performs a single F-test (F = MS_B / MS_W) to control this. *(Module 3)*

**Hypotheses:**
- H₀: μ_cosmetics = μ_haircare = μ_skincare
- H₁: At least one product type has a different mean manufacturing cost

**ANOVA Results:**

| Source | df | Sum Sq | Mean Sq | F value | p-value |
|---|---|---|---|---|---|
| Product Type | 2 | 629.0 | 314.6 | 0.370 | 0.692 |
| Residuals | 97 | 82531.0 | 850.8 | — | — |

**Decision:** p = 0.692 > 0.05 → **Fail to reject H₀**

Since the F-test was not significant, no post-hoc (Tukey) testing is needed. *(Per Module 3: Tukey is only run when ANOVA shows means are not all equal.)*

**Conclusion:** No significant difference in manufacturing costs across product types. Group means: cosmetics $43.05, haircare $48.46, skincare $48.99. The $5.94 range is not statistically meaningful.

---

### Q4 — Two-Way ANOVA: Transportation Mode × Product Type vs. Defect Rates

**Research Question:** Do transportation mode and product type interact to affect defect rates?

**Variables:**
- Independent variable 1: Transportation Mode (categorical factor, 4 levels: Air, Rail, Road, Sea)
- Independent variable 2: Product Type (categorical factor, 3 levels: cosmetics, haircare, skincare)
- Dependent variable: Defect Rates (continuous numeric)

**Why two-way ANOVA with Type II SS:** Two categorical IVs and one continuous DV — factorial design. The 4×3 = 12-cell design is **unbalanced** (cell sizes range 4–13), so the default `aov()` function using Type I sums of squares gives order-dependent results and is incorrect here. Type II SS via `Anova()` from the `car` package is used instead, which is order-independent and appropriate for unbalanced designs. *(Module 4)*

```r
library(car)
q4_model <- lm(Defect.rates ~ Transportation.modes * Product.type, data = q4_data)
Anova(q4_model, type = "II")
```

**Hypotheses (three sets):**
- H₀ / H₁ (Transportation Mode): Mean defect rates equal / at least one mode differs
- H₀ / H₁ (Product Type): Mean defect rates equal / at least one product type differs
- H₀ / H₁ (Interaction): No interaction / the effect of transportation mode depends on product type

**Unbalanced Cell Counts:**

| Transport Mode | Cosmetics | Haircare | Skincare | Total |
|---|---|---|---|---|
| Air | 5 | 8 | 13 | 26 |
| Rail | 9 | 10 | 9 | 28 |
| Road | 6 | 12 | 11 | 29 |
| Sea | 6 | 4 | 7 | 17 |
| **Total** | **26** | **34** | **40** | **100** |

**Two-Way ANOVA Results (Type II SS):**

| Source | df | Sum Sq | F value | p-value | Decision |
|---|---|---|---|---|---|
| Transportation Mode | 3 | 9.07 | 1.61 | 0.193 | Fail to reject H₀ |
| Product Type | 2 | 5.13 | 1.37 | 0.261 | Fail to reject H₀ |
| Mode × Type (Interaction) | 6 | 32.00 | 2.84 | **0.014** | **Reject H₀ ✅** |
| Residuals | 88 | 165.45 | — | — | — |

**Post-Hoc: One-Way ANOVA within each Transportation Mode**

Because the interaction was significant, data were filtered by transportation mode and one-way ANOVA was applied within each level to identify where product type differences lie. *(Per Module 4 slide 21: when interaction is significant, filter to one level and compare the other factor.)*

```r
# Example: Air transportation
air_data <- q4_data %>% filter(Transportation.modes == "Air")
air_model <- aov(Defect.rates ~ Product.type, data = air_data)
summary(air_model)
TukeyHSD(air_model)
```

| Transport Mode | df | F value | p-value | Decision |
|---|---|---|---|---|
| Air | 2, 23 | 4.602 | **0.021** | **Reject H₀ ✅** |
| Rail | 2, 25 | 1.294 | 0.292 | Fail to reject H₀ |
| Road | 2, 26 | 0.842 | 0.442 | Fail to reject H₀ |
| Sea | 2, 14 | 2.965 | 0.084 | Fail to reject H₀ |

**Tukey HSD within Air:**
- Haircare vs. Cosmetics: difference = +2.525328, adjusted p = **0.0168950** ✅ significant
- Skincare vs. Cosmetics: difference = +1.295037, adjusted p = 0.2390981 — not significant
- Skincare vs. Haircare: difference = −1.230291, adjusted p = 0.1747089 — not significant

**Conclusion:** The significant interaction is driven entirely by Air transportation. Haircare products have significantly higher defect rates than cosmetics when shipped by air. No meaningful product-type differences exist under rail, road, or sea transportation. The risk is not inherent to haircare or to air transport in isolation — it emerges from their specific combination.

---

## Key Finding

The most important insight is the **significant interaction between Transportation Mode and Product Type** (p = 0.014). Neither factor alone predicts defect rates, but together they reveal a meaningful pattern: **air transportation is associated with significantly higher defect rates for haircare products compared to cosmetics** (Tukey adjusted p = 0.0168950, difference = +2.525328).

This is made more compelling by the Q1 result: product type does not affect manufacturing costs (p = 0.692) either — meaning product category is not a general driver of supply chain performance, but it becomes critically important when combined with the wrong logistics decision.

**Management implication:** Routing decisions should be tailored by product type. Applying uniform logistics policies across all product categories may conceal important quality risks.

---

## Limitations

1. **Sample size:** n = 100 across 12 cells means some cells have as few as 4 observations, reducing statistical power and increasing uncertainty in cell-level estimates.
2. **Observational data:** No causal conclusions can be drawn. Confounders such as shipping distance, temperature sensitivity, or packaging quality may explain the observed interaction.
3. **Generalizability:** Results reflect one supply chain context across five cities in India and may not transfer to other industries, product categories, or geographic regions.

---

## Files

| File | Description |
|---|---|
| `original_supply_chain.csv` | Raw dataset from Kaggle before cleaning |
| `Cleaned_Supply_Chain_Data.csv` | Final cleaned dataset used for all analyses |
| `FinalProjectQ1.Rmd` & `FinalProjectQ2.Rmd| Final R Markdown with all Q1 and Q4 analysis code |
| `STAT301_FinalProject_Li_Stoven.docx` | Final submitted report (Q1 + Q4, double-spaced, 2–6 pages) |

---

## R Packages Used

```r
library(tidyverse)   # data manipulation and visualization
library(dplyr)       # data wrangling
library(ggplot2)     # plotting — bar chart with CI for Q1, interaction plot for Q4
library(car)         # Type II ANOVA via Anova() — required for unbalanced Q4 design
```

> Note: `emmeans` was removed from the final analysis. Per Module 4, post-hoc for a significant interaction uses `filter() + aov() + TukeyHSD()` within each level of one factor — not `emmeans()`.
