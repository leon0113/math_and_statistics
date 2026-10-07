# Part 1: Statistics Foundations, Central Tendency & Dispersion

> **Goal of this part:** Understand what data is, how to summarise one column of data with a single number (mean, median...), and how to measure how "spread out" the data is.
>
> **Part 2** covers: quantiles, boxplots, covariance, correlation and multi-variable graphs.

---

## Table of Contents
1. What is Statistics?
2. Descriptive vs Inferential Statistics
3. Population vs Sample
4. Types of Data
5. Measures of Central Tendency (Mean, Median, Mode, Weighted Mean, Trimmed Mean)
6. Measures of Dispersion (Range, Variance, MAD, Standard Deviation, CV)
7. Graphs for Univariate Analysis (one column)
8. Quick Revision Cheat Sheet

---

## 1. What is Statistics?

**Definition:** Statistics is a branch of mathematics about **collecting, analysing, interpreting and presenting data**. It gives us tools to make sense of large amounts of data and to make decisions based on it.

**Why it matters in Machine Learning (ML):**
- ML models learn from data. Before training, you must *understand* your data (average, spread, outliers, relationships).
- Statistics helps you clean data, choose features, and judge whether a model's result is trustworthy.

---

## 2. Descriptive vs Inferential Statistics

| | Descriptive Statistics | Inferential Statistics |
|---|---|---|
| **Simple meaning** | *Describes* the data you already have | Uses a small group to *make guesses/predictions* about a bigger group |
| **Examples** | mean, median, standard deviation, graphs | probabilities, hypothesis tests |
| **Does it change the data?** | No, only summarises it | No, but it goes beyond the data |

> Easy memory trick: **Descriptive = "What does my data look like?"** and **Inferential = "What can I conclude about everyone?"**

---

## 3. Population vs Sample

- **Population** = the *entire* group you are interested in.
- **Sample** = a *smaller part (subset)* of the population.

**Examples (from notes):**
1. Population: all cricket fans. Sample: fans who were present in the stadium.
2. Population: all students. Sample: students who actually come to college for lectures.

**Why does this matter?** We usually can't measure everyone, so we measure a sample and use it to learn about the population. Later you will see formulas that change slightly (`N` vs `n-1`) depending on whether you have the population or a sample.

| Symbol | Population | Sample |
|---|---|---|
| Mean | μ (mu) | x̄ (x-bar) |
| Variance | σ² | s² |
| Std. deviation | σ | s |
| Size | N | n |

---

## 4. Types of Data

```
                    Data
          ┌──────────┴───────────┐
   Categorical                Numerical
 (Qualitative)              (Quantitative)
   │                              │
 ┌─┴──────────┐             ┌─────┴──────────┐
Nominal    Ordinal       Discrete        Continuous
```

### 4.1 Categorical (Qualitative) data: *labels / groups*
| Type | Meaning | Example |
|---|---|---|
| **Nominal** | Categories with **no order** | Gender (male, female), colour |
| **Ordinal** | Categories **with an order** | Ranking (1st, 2nd, 3rd), reviews (bad, ok, good) |

### 4.2 Numerical (Quantitative) data: *numbers*
| Type | Meaning | Example |
|---|---|---|
| **Discrete** | Whole numbers, **no decimals** | Age (25 years), number of children |
| **Continuous** | Can have **decimals** | Weight (7.5 kg), height |

**Use in ML:** The type of data decides which graph, which statistic and which preprocessing method you can use. For example, you can't take the "mean" of gender.

---

## 5. Measures of Central Tendency

**Definition:** A single value that represents the "typical" or "central" value of a dataset. It summarises the whole column into one number.

### 5.1 Mean (Average)
Add all values, divide by how many there are.

- Population mean: **μ = (Σ xᵢ) / N**
- Sample mean: **x̄ = (Σ xᵢ) / n**

**Example:** Data = 2, 4, 6 → mean = (2+4+6)/3 = **4**

**Weakness:** One very big or very small value (an outlier) can drag the mean a lot.

### 5.2 Median (Middle value)
Sort the data first, then take the middle.

- **Odd** number of values → the exact middle one.
  `1 2 [3] 4 5` → median = **3**
- **Even** number of values → average of the two middle ones.
  `1 2 [3 4] 5 6` → median = (3+4)/2 = **3.5**

**Strength:** Not affected much by outliers.

### 5.3 Mode (Most frequent value)
The value that appears the most times.

**Example:** `1, 1, 2, 3, 1` → mode = **1**

**Use:** Best for categorical data (e.g. most common colour).

### 5.4 Weighted Mean
Some values are **more important** than others, so each gets a *weight*.

**Formula:** (sum of value × weight) ÷ (sum of weights)

**Example (from notes):** values 10, 12, 8 with weights 0.2, 0.3, 0.5

```
(0.2×10 + 0.3×12 + 0.5×8) / (0.2 + 0.3 + 0.5)
= (2 + 3.6 + 4) / 1.0
= 9.6
```

**Real-life example:** Final marks where exam = 70%, assignment = 30%.

### 5.5 Trimmed Mean
**Idea:** Remove a certain **percentage of the smallest and largest values**, then take the mean of the rest. The % removed is called the **trimming percentage**.

**Example (from notes):** 20% trimming on 10 values

```
Data:  20, 22, 23, 25, 28, 30, 32, 35, 50, 80
20% of 10 = 2 values removed from EACH end
Removed: 20, 22  (bottom)   and   50, 80  (top)
Left:    23, 25, 28, 30, 32, 35

Trimmed mean = (23+25+28+30+32+35) / 6 = 173 / 6 ≈ 28.83
```

> **Note:** In your handwritten note the last number was written as `3.5`. It should be **35**.

### ⭐ Key point
**Median and trimmed mean help remove the effect of unusual values (outliers)**, so the result makes more sense.

### 5.6 Which one should I use?
| Situation | Best choice |
|---|---|
| Normal data, no outliers | Mean |
| Data with outliers (e.g. salaries) | Median or trimmed mean |
| Categories | Mode |
| Some values more important | Weighted mean |

**Use in ML:** Missing values in a column are often filled with the mean (normal data), median (data with outliers) or mode (categories).

---

## 6. Measures of Dispersion (Spread)

**Definition:** They tell you **how spread out the data is around the centre** (mean/median). Two datasets can have the same mean but very different spread.

> **Tiny example:** `[−10, 0, 10]` and `[−20, 0, 20]` both have mean 0, but the second is more spread out. The mean alone cannot show this, so we need dispersion measures.

### 6.1 Range
**Range = Maximum − Minimum**

**Example:** data from −5 to 5 → range = 5 − (−5) = **10**

**Weakness:** Uses only 2 values, so it is badly affected by outliers.

### 6.2 Variance (σ² or s²)
**Idea:** The *average of the squared distances* of each value from the mean.

**Steps:**
1. Find the mean.
2. Subtract the mean from each value (the *deviation*).
3. Square each deviation (so negatives don't cancel positives).
4. Average them.

| | Formula |
|---|---|
| **Population variance** | σ² = Σ(x − μ)² / **N** |
| **Sample variance** | s² = Σ(x − x̄)² / **(n − 1)** |

**Worked example:** data = 2, 4, 6 (mean = 4)
```
Deviations:        −2,  0,  +2
Squared:            4,  0,   4   → sum = 8
Population var  = 8 / 3 ≈ 2.67
Sample var      = 8 / (3−1) = 4
```

> **Why n − 1 for a sample?** A sample usually under-estimates the real spread, so dividing by a slightly smaller number (n−1) corrects it.

### 6.3 Mean Absolute Deviation (MAD)
Same idea as variance, but uses **absolute value** instead of squaring.

**MAD = Σ |xᵢ − x̄| / n**

For 2, 4, 6: (2 + 0 + 2)/3 ≈ **1.33**

### 6.4 Standard Deviation (STD)
**STD = √Variance**

- Sample: **s = √[ Σ(x − x̄)² / (n−1) ]**
- **Why use it?** Variance is in *squared units* (e.g. kg²). Taking the square root puts it back in the *original units* (kg), so it is easier to understand.
- Small STD → values close to the mean. Large STD → values far from the mean.

For 2, 4, 6 (sample): s = √4 = **2**

### 6.5 Coefficient of Variation (CV)
**CV = (STD / Mean) × 100%**

- Spread as a **percentage of the mean**.
- Great for **comparing two datasets with different units or scales** (e.g. height in cm vs weight in kg).

**Example:** Mean = 50, STD = 5 → CV = (5/50)×100 = **10%**

### 6.6 Summary table
| Measure | What it tells you | Affected by outliers? |
|---|---|---|
| Range | Max − Min | Very much |
| Variance | Average squared spread | Yes |
| MAD | Average absolute spread | Less |
| Std. Deviation | Spread in original units | Yes |
| CV | Relative spread (%) | Yes |

**Use in ML:**
- **Feature scaling / standardisation** uses mean and STD: `z = (x − mean) / std`.
- Features with zero variance carry no information and can be dropped.
- Variance is central to PCA and to the bias–variance trade-off.

---

## 7. Graphs for Univariate Analysis (ONE column)

### 7.1 Categorical column → Frequency Distribution Table
A table counting **how many times each value appears**.

| Colour | Frequency | Relative Freq. | Cumulative Freq. |
|---|---|---|---|
| Red | 5 | 5/20 = 25% | 5 |
| Blue | 10 | 50% | 15 |
| Green | 5 | 25% | 20 |

| Term | Meaning | Best graph |
|---|---|---|
| **Frequency** | Count of each value | **Bar chart** |
| **Relative frequency** | Proportion or % of each value | **Pie chart** |
| **Cumulative frequency** | Running total of frequencies | **Line chart** |

```
Bar chart (frequency)
Blue  | ██████████ 10
Red   | █████ 5
Green | █████ 5
```

### 7.2 Numerical column → Frequency Table + Histogram
For numbers (like Age: 25, 10, 5, 35, 45...), we first create **buckets (bins)**:

| Age bucket | Label |
|---|---|
| 0–10 | Child |
| 11–20 | Boy/Girl |
| 21–35 | Man/Woman |
| ... | ... |

Then count how many fall in each bucket and draw a **histogram** (like a bar chart but for numeric ranges, so bars touch).

```
Histogram of Age
Count
 8 |        ███
 6 |   ███  ███
 4 |   ███  ███  ███
 2 |███ ███  ███  ███
   +--------------------
    0-10 11-20 21-35 36+
```

**Use in ML:** Histograms show whether a feature is skewed, has outliers, or needs transformation (e.g. log) before training.

---

## 8. Quick Revision Cheat Sheet

- **Descriptive** = describe data. **Inferential** = conclude about a bigger group.
- **Population** (N, μ, σ) vs **Sample** (n, x̄, s).
- Data: **Categorical** (nominal, ordinal) vs **Numerical** (discrete, continuous).
- **Central tendency:** mean, median, mode, weighted mean, trimmed mean → *median & trimmed mean resist outliers*.
- **Dispersion:** range, variance, MAD, STD, CV.
- **Variance** uses squares. **STD** = √variance. **Sample** divides by **n−1**.
- **Univariate graphs:** categorical → bar/pie/line (cumulative). Numerical → histogram.

