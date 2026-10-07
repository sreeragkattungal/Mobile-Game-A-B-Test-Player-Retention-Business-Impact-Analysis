# Mobile-Game-A-B-Test-Player-Retention-Business-Impact-Analysis
# 🎮 Mobile Game A/B Testing & Player Segmentation

## 📌 Project Overview

This project analyzes an **A/B test for a mobile game** to determine whether changing the location of the first gate affects **player retention and engagement**.

Players were randomly assigned to two experimental groups:

* **Gate 30** — First gate at level 30
* **Gate 40** — First gate at level 40

The analysis combines **SQL-based player segmentation** with **statistical hypothesis testing, bootstrap confidence intervals, and clustering** to evaluate the experiment and understand differences across player segments.

---

## 🎯 Business Objective

The main question is:

> **Does moving the first gate from level 30 to level 40 improve player retention?**

The analysis evaluates:

* 1-day retention
* 7-day retention
* Retention differences between experimental groups
* Statistical significance of observed differences
* Player engagement segments
* Whether the treatment effect differs across player segments

---

## 📊 Dataset

The project uses the **Cookie Cats** mobile-game A/B testing dataset.

| Feature          | Description                                |
| ---------------- | ------------------------------------------ |
| `userid`         | Unique player ID                           |
| `version`        | Experimental group (`gate_30` / `gate_40`) |
| `sum_gamerounds` | Number of game rounds played               |
| `retention_1`    | Whether the player returned after 1 day    |
| `retention_7`    | Whether the player returned after 7 days   |

**Dataset size:** ~90,189 players

---

# 🔬 Analysis Workflow

## 1. Data Exploration & Quality Checks

Initial analysis was performed to understand:

* Dataset dimensions
* Missing values
* Duplicate player IDs
* Distribution of game rounds
* Treatment/control group sizes
* Retention rates

Basic SQL and Python were used for data validation and exploratory analysis.

---

## 2. SQL-Based Player Segmentation

Player engagement was divided into **three tiers using SQL `NTILE(3)`**.

Players were ranked based on their engagement (`sum_gamerounds`) and divided into:

* **Low Engagement**
* **Medium Engagement**
* **High Engagement**

Example SQL approach:

```sql
SELECT
    userid,
    version,
    sum_gamerounds,
    NTILE(3) OVER (
        ORDER BY sum_gamerounds
    ) AS engagement_tier
FROM players;
```

This allowed the A/B test to be analyzed not only at the overall level but also across different engagement segments.

---

# 📈 3. A/B Test Analysis

The primary comparison was between:

**Control:** Gate 30
**Treatment:** Gate 40

Retention rates were compared using hypothesis testing.

### Chi-Square Test

A contingency table was constructed:

|         | Retained | Not Retained |
| ------- | -------: | -----------: |
| Gate 30 |      Yes |           No |
| Gate 40 |      Yes |           No |

The chi-square test evaluates whether **retention and experimental group are statistically associated**.

### Hypotheses

**H₀:** Gate placement and retention are independent.

**H₁:** Gate placement and retention are associated.

A significance level of:

```text
α = 0.05
```

was used.

---

# 📦 4. Bootstrap Confidence Intervals

Bootstrap resampling was used to estimate the uncertainty around the difference in retention between the two groups.

For each bootstrap iteration:

1. Resample players with replacement.
2. Calculate retention for each group.
3. Calculate the difference in retention.
4. Repeat many times.
5. Use the resulting distribution to construct a confidence interval.

This provides an empirical estimate of the uncertainty around the treatment effect.

### Why Bootstrap?

A bootstrap interval helps answer:

> **How much could the observed retention difference vary if we repeatedly sampled players from the same population?**

It complements the hypothesis test by providing an **effect-size estimate and uncertainty**, rather than only a p-value.

---

# 🧪 5. Segment-Level Analysis

The A/B test was also evaluated separately for:

* Low-engagement players
* Medium-engagement players
* High-engagement players

This helps determine whether the gate change affects different types of players differently.

For example:

```text
Overall effect
      ↓
 ┌────┼────┐
Low  Med   High
```

The overall test and segment-level tests were treated as separate analytical questions.

---

# 🔢 6. Multiple Testing & Bonferroni Correction

Because multiple segment-level hypotheses were tested, the probability of obtaining a false positive increases.

To control the family-wise error rate, **Bonferroni correction** was applied.

The adjusted significance level is:

$$
\alpha_{adjusted} = \frac{\alpha}{m}
$$

where:

* `α = 0.05`
* `m = number of segment-level tests`

For example, with three segment tests:

$$
\alpha_{adjusted}=\frac{0.05}{3}=0.0167
$$

### Interpretation

If a segment has:

```text
p < 0.05
```

but:

```text
p > 0.0167
```

it is significant at the uncorrected level but **does not survive Bonferroni correction**.

This reduces the chance of claiming a segment effect that is actually a false positive.

---

# 🤖 7. K-Means Player Segmentation

In addition to SQL-based segmentation, **K-Means clustering** was used to identify naturally occurring player groups.

The clustering analysis produced:

```text
Silhouette Score = 0.5438
```

A silhouette score around 0.54 indicates reasonably well-separated clusters.

The resulting clusters were compared with the SQL-based `NTILE(3)` engagement tiers.

The moderate agreement indicates that the two approaches capture related but not identical aspects of player behavior.

---

# 🛠️ Technologies Used

### Programming

* Python

### Data Analysis

* Pandas
* NumPy
* SciPy

### SQL

* SQL
* Window Functions
* `NTILE()`
* Aggregations
* CTEs
* Joins

### Statistics

* Chi-square test
* Bootstrap confidence intervals
* Hypothesis testing
* Multiple-testing correction
* Bonferroni correction

### Machine Learning

* K-Means clustering

### Visualization

* Matplotlib
---

#  Key Takeaways

### 1. Overall A/B Test

The experiment was evaluated using both **statistical significance and effect size** rather than relying only on the p-value.

### 2. Confidence Intervals

Bootstrap confidence intervals were used to quantify uncertainty around the estimated retention difference.

### 3. Player Segmentation

SQL `NTILE(3)` provided an interpretable engagement segmentation, while K-Means provided a data-driven alternative.

### 4. Multiple Testing

Bonferroni correction was applied to segment-level comparisons to control the probability of false positives.

### 5. Business Interpretation

Statistical significance alone is not enough to make a product decision. The final decision should consider:

* Size of the retention effect
* Confidence interval
* Statistical significance
* Player segment behavior
* Business impact
* Potential product-side trade-offs

---

#  Skills Demonstrated

* A/B Testing
* Product Analytics
* Statistical Hypothesis Testing
* Chi-Square Testing
* Bootstrap Resampling
* Confidence Intervals
* Multiple Testing Correction
* SQL Window Functions
* Player Segmentation
* K-Means Clustering
* Data Visualization
* Business Interpretation of Statistical Results

---

##  Author

**Sreerag K**

MSc Applied Mathematics
National Institute of Technology, Warangal
