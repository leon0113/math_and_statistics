# Part 2: Quantiles, Boxplots, Bivariate Analysis, Covariance & Correlation

> **Goal of this part:** Learn how to split data into positions (quantiles), detect outliers (boxplot), and measure the relationship between two columns (covariance, correlation).
>
> Prerequisite: Part 1 (mean, variance, standard deviation, data types).

---

## Table of Contents

1. Quantiles & Percentiles
2. 5-Number Summary & IQR
3. Boxplots (with full worked example)
4. Graphs for Bivariate Analysis (two columns)
5. Scatterplots
6. Covariance
7. Correlation
8. Covariance vs Correlation
9. Correlation ≠ Causation
10. Visualising Multiple Variables
11. Python Code Demo (from your notebook)
12. Quick Revision Cheat Sheet

---

## 1. Quantiles & Percentiles

### 1.1 Quantiles

**Definition:** Quantiles divide **sorted** data into groups that each hold an equal number of observations.

| Type            | Splits data into | Cut points                            |
| --------------- | ---------------- | ------------------------------------- |
| **Quartiles**   | 4 equal parts    | Q1 (25%), Q2 (50% = median), Q3 (75%) |
| **Quintiles**   | 5 equal parts    | every 20%                             |
| **Deciles**     | 10 equal parts   | D1 (10%), D2 (20%) ... D9 (90%)       |
| **Percentiles** | 100 equal parts  | P1 ... P99                            |

**Things to remember**

1. Data must be **sorted from low to high** first.
2. A quantile is really a **position (location)** in the data.
3. The result is **not necessarily an actual value** in the data (it can fall between two values).
4. All other "tiles" can be derived from percentiles (e.g. Q1 = 25th percentile).

### 1.2 Percentile

**Definition:** The percentage of observations that fall **below** a value.

> **Example:** You got the 90th percentile in an exam → 90% of students scored below you and 10% are ahead.

### 1.3 Finding the VALUE at a percentile

**Formula:** `PL = (p / 100) × (N + 1)`

- `PL` = position (location) of the percentile
- `p` = the percentile you want (e.g. 75)
- `N` = total number of values

**Worked example:** Find the 75th percentile of
`78, 82, 84, 88, 91, 93, 94, 96, 98, 99` (already sorted, N = 10)

```
Position numbers:  1   2   3   4   5   6   7   8   9   10
Values:           78  82  84  88  91  93  94  96  98   99

PL = (75/100) × (10+1) = 8.25
```

The position 8.25 is **between the 8th value (96) and 9th value (98)**.
Go 0.25 of the way from 96 to 98:

```
75th percentile = 96 + 0.25 × (98 − 96) = 96.5   ✅
```

### 1.4 The opposite: percentile RANK of a value

**Formula:** `Percentile rank = (x + 0.5·y) / n`

- `x` = number of values **below** the given value
- `y` = number of values **equal to** the given value
- `n` = total number of values

**Worked example:** percentile of **88** in the same data

```
x = 3 (78, 82, 84 are below), y = 1, n = 10
Rank = (3 + 0.5×1) / 10 = 0.35 → 35th percentile
```

**Use in ML:** Percentiles are used to detect and cap outliers (e.g. clip values above the 99th percentile), and to report model metrics such as "95th percentile latency".

---

## 2. 5-Number Summary & IQR

**Definition:** Five values that summarise a dataset by splitting it into 4 equal parts (25% each).

| #   | Value                   | Meaning                             |
| --- | ----------------------- | ----------------------------------- |
| 1   | **Minimum**             | Smallest value                      |
| 2   | **Q1 (Lower quartile)** | 25% of data lies below this         |
| 3   | **Median (Q2)**         | Middle value, 50% below / 50% above |
| 4   | **Q3 (Upper quartile)** | 75% of data lies below this         |
| 5   | **Maximum**             | Largest value                       |

```
 Min        Q1        Median      Q3         Max
  |----25%----|----25%----|----25%----|----25%----|
```

It quickly shows **centre, spread and shape** of the data, and is usually drawn as a **boxplot**.

### Interquartile Range (IQR)

**IQR = Q3 − Q1**

- It is the range of the **middle 50%** of the data.
- It is a spread measure that is **not affected by outliers**, unlike range.
- It is used to find outliers (next section).

---

## 3. Boxplots

### 3.1 What is a boxplot?

Also called a **box-and-whisker plot**. It draws the 5-number summary as a picture so you can see the distribution at a glance.

```
 outlier                                                    outliers
   •   |-------|========|=======|=======|-------|             •  •
        lower   Q1      Median    Q3     upper
        whisker  \______ IQR _____/      whisker
```

**Parts of the plot**

- **Box** = from Q1 to Q3 (this length is the IQR)
- **Line inside the box** = median
- **Whiskers** = extend to the smallest/largest values that are _not_ outliers
- **Dots outside** = outliers

### 3.2 Outlier rules (the "fences")

```
Lower fence = Q1 − 1.5 × IQR
Upper fence = Q3 + 1.5 × IQR
```

- Any value **below the lower fence or above the upper fence = outlier**.
- The whiskers stop at the smallest and largest values **inside** the fences.

### 3.3 Full worked example (from your class)

**Data (sorted, N = 10):**
`6, 213, 241, 260, 281, 290, 314, 321, 350, 1500`

**Step 1: Median (Q2)**

```
Position = (50/100)×(10+1) = 5.5  → between 5th (281) and 6th (290)
Q2 = (281 + 290)/2 = 285.5
```

**Step 2: Q1**

```
Position = (25/100)×11 = 2.75 → between 2nd (213) and 3rd (241)
Q1 = 213 + 0.75×(241 − 213) = 234
```

**Step 3: Q3**

```
Position = (75/100)×11 = 8.25 → between 8th (321) and 9th (350)
Q3 = 321 + 0.25×(350 − 321) = 328.25 ≈ 328
```

**Step 4: IQR**

```
IQR = 328 − 234 = 94      (≈ 94.25 if you keep 328.25)
```

**Step 5: Fences**

```
Lower fence = 234 − 1.5×94 = 234 − 141 = 93
Upper fence = 328 + 1.5×94 = 328 + 141 = 469
```

**Step 6: Find outliers and whiskers**

- `6` < 93 → **outlier**
- `1500` > 469 → **outlier**
- Smallest value inside the fences = **213** (lower whisker)
- Largest value inside the fences = **350** (upper whisker)

**Final boxplot:**

```
  •                |------[ 234 | 285.5 | 328 ]------|                •
  6               213                              350             1500
(outlier)      (lower whisker)                (upper whisker)    (outlier)
```

> **Note:** In the handwritten note, "min" and "max" of the boxplot are the _fences_ (93 and 469), but the whiskers are actually drawn at the real data points **213** and **350**.

### 3.4 Benefits of a boxplot

- Easy way to **see the distribution** of data
- Tells about **skewness** (if the median is not centred in the box, the data is skewed)
- Can **identify outliers**
- Lets you **compare 2 categories** (side-by-side boxplots)

### 3.5 Side-by-side boxplot

Use one **categorical** + one **numerical** column. Example: _Cat Weight by Sex_

```
Body weight
 3.0 |   ─┬─            ┌──┐
     |    │             │  │
 2.5 |  ┌─┴─┐           │  │
     |  │ F │           │ M│
 2.0 |  └───┘           └──┘
         F                M
```

You can instantly see that males (M) weigh more than females (F).

**Use in ML:** Boxplots are used in EDA (Exploratory Data Analysis) to spot outliers before training, and to compare a feature across classes (does feature X differ between class A and B?).

---

## 4. Graphs for Bivariate Analysis (TWO columns)

Bivariate = analysing **two columns together**. The graph depends on the data types:

| Column 1    | Column 2    | Graph / Tool                                                |
| ----------- | ----------- | ----------------------------------------------------------- |
| Categorical | Categorical | **Crosstab** (cross-table), grouped bar chart               |
| Numerical   | Numerical   | **Scatterplot**                                             |
| Categorical | Numerical   | **Bar chart (average per group)**, **side-by-side boxplot** |

### 4.1 Categorical vs Categorical → Crosstab

**Example: Titanic**, with `Survived` (0 = no, 1 = yes) and `Pass_class` (0 = elite, 1 = normal, 2 = high).

A 2×3 crosstab counts how many people fall in each combination:

| Survived \ Pass_class | 0   | 1   | 2   |
| --------------------- | --- | --- | --- |
| **0**                 | 92  | 21  | 5   |
| **1**                 | 100 | 5   | 0   |

(Numbers are the example values from the notes.) Each cell tells you "how many people were in this class AND survived/not".

### 4.2 Numerical vs Numerical → Scatterplot

See the next section.

### 4.3 Categorical vs Numerical

**Example:** column `Sex` and column `Age`

| Sex    | Age |
| ------ | --- |
| male   | 22  |
| female | 38  |
| female | 21  |
| male   | 40  |

Use a **bar chart of the average age** for male and female, or group age into buckets:

| Group     | 20–30 | 30–40 |
| --------- | ----- | ----- |
| Male      | 32    | 10    |
| Female    | 25    | 31    |
| **Total** | 57    | 41    |

---

## 5. Scatterplots

**Definition:** A graph where **each dot is one row of data**: the x-axis shows one numerical column and the y-axis shows another.

**Example:** `Sqft` vs `Price` of houses.

| Sqft | Price |
| ---- | ----- |
| 1500 | 60L   |
| 1700 | 80L   |
| 1200 | 35L   |

```
Price
  |                     •  •
  |                 •  •
  |            •  •
  |       •  •
  |   •
  +--------------------------- Sqft
```

This looks like an upward trend, so bigger houses cost more.

**What you can see:**

- Direction (up, down, none)
- Strength (tight cloud vs scattered)
- Outliers (dots far away from the cloud)

**Use in ML:** Before building a regression model, a scatterplot shows if a straight-line relationship exists between a feature and the target.

---

## 6. Covariance

### 6.1 What problem does it solve?

Variance tells us the spread of **one** variable. But what if we want to know whether **two variables move together**? Covariance solves this.

### 6.2 Definition

> **Covariance** measures **how much two variables change together** in a **linear** way. When one goes up, does the other also go up (or down)?

### 6.3 How to read the sign

| Covariance       | Meaning                             | Scatterplot shape   |
| ---------------- | ----------------------------------- | ------------------- |
| **Positive (+)** | Both move in the **same direction** | dots go ↗           |
| **Negative (−)** | Move in **opposite directions**     | dots go ↘           |
| **≈ 0**          | **No linear relationship**          | dots spread or flat |

```
Cov +ve          Cov −ve          Cov ≈ 0
   •  •  •        •                 • • • • •
  • •              • •              (flat/random)
 - •                 • •
-                      •  •
```

### 6.4 Formula

|                | Formula                             |
| -------------- | ----------------------------------- |
| **Population** | σxy = Σ(X − μx)(Y − μy) / **N**     |
| **Sample**     | sxy = Σ(X − x̄)(Y − ȳ) / **(n − 1)** |

- X, Y = values of the two columns
- μx, μy (or x̄, ȳ) = their means
- N (or n) = number of rows

### 6.5 Step-by-step recipe

1. Find the mean of X and the mean of Y.
2. For each row: (X − mean of X) and (Y − mean of Y).
3. **Multiply** those two numbers.
4. **Add** all the products.
5. Divide by **n − 1** (sample).

> **Why multiplication works:** If both values are above their means (or both below), the product is **positive**. If one is above and the other below, the product is **negative**. The overall sum shows which type wins.

### 6.6 Example 1: Experience vs Salary (positive covariance)

x = Experience, y = Salary. Mean x = 8, mean y = 6.

| Exp (x) | Salary (y) | x − 8 | y − 6   | Product |
| ------- | ---------- | ----- | ------- | ------- |
| 2       | 1          | −6    | −5      | 30      |
| 5       | 2          | −3    | −4      | 12      |
| 8       | 5          | 0     | −1      | 0       |
| 12      | 12         | 4     | 6       | 24      |
| 13      | 10         | 5     | 4       | 20      |
|         |            |       | **Sum** | **86**  |

```
Covariance = 86 / (5 − 1) = 21.5   → POSITIVE ✅
```

More experience → more salary.

> **Note:** In your notes the third product was written as −1 (sum 85). The correct product is 0 × (−1) = **0**, so the sum is 86 and 86/4 = **21.5**, matching your final answer.

### 6.7 Example 2: Backlogs vs Package (negative covariance)

x = Backlogs, y = Package. Mean x = 8, mean y = 6.

| Backlogs (x) | Package (y) | x − 8 | y − 6   | Product |
| ------------ | ----------- | ----- | ------- | ------- |
| 2            | 10          | −6    | 4       | −24     |
| 5            | 12          | −3    | 6       | −18     |
| 8            | 5           | 0     | −1      | 0       |
| 12           | 2           | 4     | −4      | −16     |
| 13           | 1           | 5     | −5      | −25     |
|              |             |       | **Sum** | **−83** |

```
Covariance = −83 / 4 = −20.75  → NEGATIVE ✅
```

More backlogs → lower package.

> **Note:** −83/4 is **−20.75**; your notes show −21.3, which is a small rounding slip.

### 6.8 Example 3: Constant column (zero covariance)

If package is **always 10** for every row, then (y − mean y) = 0 everywhere, so every product is 0 → **Covariance = 0**.
No change in y means no relationship with x.

### 6.9 Covariance of a variable with itself = Variance

Cov(X, X) = Σ(X − x̄)(X − x̄)/(n−1) = Σ(X − x̄)²/(n−1) = **Variance of X**

### 6.10 ❌ Disadvantage of covariance

**It does NOT tell the _strength_ of the relationship, because its size depends on the scale (units) of the variables.**

Your notebook proves this. Same data, but every value doubled:

| Data   | Covariance              |
| ------ | ----------------------- |
| x, y   | **1148.75**             |
| 2x, 2y | **4595.0** (4× bigger!) |

The relationship didn't change (same shape of scatterplot), but covariance jumped 4 times. So we cannot say "1148 is strong" or "4595 is stronger". There is no fixed scale. Also, covariance of 21.5 vs 4595 can't be compared across datasets.

➡️ **Solution = Correlation.**

---

## 7. Correlation

### 7.1 What problem does it solve?

It answers: _"Can we put a number on how weak or strong the relationship is, on a fixed scale that doesn't depend on units?"_

### 7.2 Definition

> **Correlation** measures the **degree (strength and direction)** to which two variables are related and tend to change together. It is measured with the **correlation coefficient (r)**.

### 7.3 Formula

```
              Cov(x, y)
Correlation = ─────────
               σx × σy
```

We divide covariance by the **standard deviations of both columns**. This cancels the units and scale. (Here this is the Pearson correlation coefficient.)

### 7.4 The scale: always between −1 and +1

| Value of r     | Meaning                                                   |
| -------------- | --------------------------------------------------------- |
| **+1**         | Perfect positive correlation (a straight line going up)   |
| **0.5 to 1**   | Strong/moderate positive                                  |
| **0**          | No (linear) correlation                                   |
| **−0.5 to −1** | Strong/moderate negative                                  |
| **−1**         | Perfect negative correlation (a straight line going down) |

```
 Strong +ve      Weak +ve        Strong −ve
   r ≈ +0.9        r ≈ +0.4         r ≈ −0.9
    •  •             • •  •          •
   • •              •    •   •        • •
  • •              •  •                 • •
 -                                         •  •

 Weak −ve        Moderate −ve        No correlation
   r ≈ −0.3        r ≈ −0.6            r ≈ 0
```

### 7.5 Proof from your notebook

Using the same data (x = [12, 25, 68, 42, 113], y = [11, 29, 58, 121, 100]):

| Comparison    | Correlation                           |
| ------------- | ------------------------------------- |
| x with itself | **1.0** (perfect)                     |
| x with y      | **0.6185** (moderate/strong positive) |
| 2x with 2y    | **0.6185** (unchanged by doubling!)   |

So correlation **does not change when you scale the data**. That is why it is the better measure of strength.

**Use in ML:**

- **Feature selection:** keep features highly correlated with the target.
- **Remove redundant features:** if two features have correlation near ±1, one is almost a copy (multicollinearity).
- A **correlation heatmap** is a standard first step in EDA.

---

## 8. Covariance vs Correlation

|                                | Covariance                    | Correlation      |
| ------------------------------ | ----------------------------- | ---------------- |
| Shows direction (+/−)?         | ✅ Yes                        | ✅ Yes           |
| Shows strength?                | ❌ No (depends on scale)      | ✅ Yes           |
| Range                          | −∞ to +∞                      | **−1 to +1**     |
| Has units?                     | Yes (units of x × units of y) | **No units**     |
| Changes if you scale the data? | ✅ Yes (4× in our example)    | ❌ No            |
| Formula                        | Σ(X−x̄)(Y−ȳ)/(n−1)             | Cov(x,y)/(σx·σy) |

**Related:** Variance = covariance of a variable with itself. Standard deviation = √variance.

---

## 9. Correlation ≠ Causation

**Meaning:** Just because two variables are related does **not** mean one _causes_ the other.

**Famous example: firefighters and fire damage**

- Data shows: **more firefighters at a fire → more damage.**
- ❌ Wrong conclusion: "firefighters cause damage."
- ✅ Real reason: a **third variable**, the _severity of the fire_. Bigger fires need more firefighters **and** cause more damage.

```
              Severity of fire
              ↙              ↘
   More firefighters      More damage
        (correlated, but neither causes the other)
```

To prove causation you need experiments, randomized controlled trials, or well-designed studies.

**Use in ML:** A model can find patterns (correlations) but it doesn't automatically know "why". Be careful when interpreting feature importance as cause.

---

## 10. Visualising Multiple Variables

| #   | Plot                | What it shows                                                                                                                   | Variables                                                                        |
| --- | ------------------- | ------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------- |
| 1   | **3D scatter plot** | Dots in 3D space                                                                                                                | 3 numerical (e.g. weight, height, age); colour by a category (e.g. iris species) |
| 2   | **Hue parameter**   | Adds a **colour** for a category inside a plot (e.g. grouped bar plot with Program A/B/Total)                                   | 1 categorical + 1 numerical + 1 more categorical                                 |
| 3   | **Facet grids**     | Splits one chart into **multiple small charts** by a category (e.g. Lunch vs Dinner, `tip` vs `total_bill`, coloured by smoker) | Many                                                                             |
| 4   | **Joint plot**      | Scatterplot in the middle + **histograms on top and right**                                                                     | 2 numerical (e.g. bill_length vs bill_depth of penguins)                         |
| 5   | **Pair plot**       | A grid of scatterplots for **every pair** of columns; histograms on the diagonal (e.g. iris: sepal/petal length and width)      | All numerical columns                                                            |
| 6   | **Bubble plot**     | Scatterplot where **bubble size** is a 3rd variable and colour is a 4th (e.g. x = GDP, y = population, size = another measure)  | 3–4                                                                              |

```
Bubble plot idea

Population
   |        ●(big)
   |   ○            ●
   |  ●   ○
   +------------------- GDP
 size = another variable, colour = category
```

**Use in ML:** Pair plots and joint plots are the fastest way to see relationships among all features before modelling.

---

## 11. Python Code Demo (from your notebook)

```python
import seaborn as sns
import pandas as pd
import matplotlib.pyplot as plt
import numpy as np

# 1. Create two columns
x = pd.Series([12, 25, 68, 42, 113])
y = pd.Series([11, 29, 58, 121, 100])
df = pd.DataFrame({'x': x, 'y': y})

# 2. Covariance: original vs doubled data
print(np.cov(df['x'], df['y'])[0, 1])          # 1148.75
print(np.cov(df['x']*2, df['y']*2)[0, 1])      # 4595.0  (4x bigger)

# 3. Covariance of x with itself = variance of x
print(np.cov(df['x'], df['x'])[0, 1])

# 4. Correlation: unchanged by scaling
print(df['x'].corr(df['x']))                   # 1.0
print(df['x'].corr(df['y']))                   # 0.6185...
print((df['x']*2).corr(df['y']*2))             # 0.6185... (same!)

# 5. Plot scatterplots with covariance in the title
fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(10, 3))
ax1.scatter(df['x'], df['y'])
ax2.scatter(df['x']*2, df['y']*2)
ax1.set_title("Covariance - " + str(np.cov(df['x'], df['y'])[0, 1]))
ax2.set_title("Covariance - " + str(np.cov(df['x']*2, df['y']*2)[0, 1]))
plt.show()
```

**What the notebook teaches (3 lessons):**

1. Scaling the data by 2 makes covariance **4× larger**, even though the picture looks the same.
2. The correlation of a variable with itself is **1**.
3. Correlation of x and y stays **0.6185** after scaling. Correlation is scale-free.

> Tip: the line `from re import X` in your notebook is an accidental auto-import and isn't needed.

**Handy one-liners for later:**

```python
sns.boxplot(x='sex', y='weight', data=df)   # side-by-side boxplot
sns.scatterplot(x='x', y='y', data=df)      # scatterplot
sns.pairplot(df)                            # pair plot
sns.jointplot(x='x', y='y', data=df)        # joint plot
df.corr()                                   # correlation matrix
```

---

## 12. Quick Revision Cheat Sheet

**Quantiles**

- Need **sorted** data. Quartiles (4), quintiles (5), deciles (10), percentiles (100).
- Position: `PL = (p/100)(N+1)`. Percentile rank: `(x + 0.5y)/n`.

**5-number summary:** Min, Q1, Median, Q3, Max. **IQR = Q3 − Q1**.

**Boxplot:** Outliers are outside `Q1 − 1.5·IQR` and `Q3 + 1.5·IQR`.

**Which graph?**

- Cat–Cat → crosstab | Num–Num → scatterplot | Cat–Num → bar chart / side-by-side boxplot

**Covariance:** Direction only (+, −, 0). Depends on scale. Cov(X,X) = variance.

**Correlation:** `Cov(x,y)/(σx·σy)`, between **−1 and +1**, scale-free, shows direction **and** strength.

**Remember:** Correlation does **not** imply causation (firefighters example).

**Multi-variable plots:** 3D scatter, hue, facet grid, joint plot, pair plot, bubble plot.
