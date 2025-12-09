# Class 6B: Statistical Distribution Exploration
## Uncovering Hidden Patterns in Hawk Populations

---

## Learning Goals
By the end of this class, you will be able to:
- Use pandas operations to clean data and handle missing values
- Understand how binning choices affect pattern detection in histograms
- Create and interpret boxplots to visualize statistical summaries
- Customize boxplot marks for effective communication
- Apply faceting techniques to compare multiple groups
- Build violin plots to reveal distribution shapes
- Create scatter plots with regression lines to explore relationships
- Recognize Simpson's Paradox in aggregate vs. grouped data

---

## Section 0: Data Preparation & Quality

### Code Snippet 0.1: Data Cleaning Pipeline

<!-- #region -->
```python
hawks = (
    hawks
    .drop(columns=cols_to_drop, errors='ignore')
    .dropna(subset=['Weight', 'Tail'])
    .rename(columns=lambda x: x.strip().lower())
).copy()
```
<!-- #endregion -->

<!-- #region -->
### Clicker Question 1

**What does `.dropna(subset=['Weight', 'Tail'])` do? After this cleaning, the dataset went from 908 rows to 891 rows. What can we conclude?**

- [ ] A) Drops the 'Weight' and 'Tail' columns; 17 rows were removed
- [ ] B) Drops all rows with ANY missing values; 17 hawks had some missing data
- [ ] C) Drops rows where EITHER 'Weight' OR 'Tail' is missing; 17 hawks had missing weight or tail measurements
- [ ] D) Drops rows where EITHER 'Weight' OR 'Tail' is missing; 17 hawks had their values replaced
- [ ] E) Drops the 'Weight' and 'Tail' columns; 17 hawks had missing weight or tail measurements

---

## Section 1: The Power of Binning

### Visual 1.1: Histogram Comparison with Different Bin Sizes

*[Display of 5 histograms showing hawk weight distribution with 5, 10, 20, 30, and 50 bins]*

### Clicker Question 2

**Which bin size best reveals that the weight distribution might have two distinct peaks?**

- [ ] A) 5 bins
- [ ] B) 10 bins
- [ ] C) 20 bins
- [ ] D) 30 bins
- [ ] E) 50 bins



### Clicker Question 3

**What is the main risk of using too few bins (e.g., 5 bins)?**

- [ ] A) The chart becomes too wide to display
- [ ] B) Important patterns may be hidden/smoothed over
- [ ] C) Outliers become too obvious
- [ ] D) The computation takes too long
- [ ] E) The data becomes harder to read due to overlapping bars


---

### Visual 1.2: Weight Distributions Faceted by Species

*[Small multiples showing weight distributions for Cooper's Hawk, Red-tailed Hawk, and Sharp-shinned Hawk]*

### Discussion Prompt

**Would you use the same bin size for all three species? Why or why not? Which species seems to need more bins to capture its distribution pattern?**

Notes:
_____________________________________________________________________________

_____________________________________________________________________________

_____________________________________________________________________________

---

## Section 2: Boxplots - Reading Statistical Summaries (15-18 minutes)

### Visual 2.1: Anatomy of a Boxplot

*[Single boxplot of hawk weights with labels for Q1, Median, Q3, Whiskers, and Outliers]*

### Clicker Question 4

**What does the RED line in the middle of the box represent?**

- [ ] A) The mean weight
- [ ] B) The median weight
- [ ] C) The mode (most common weight)
- [ ] D) The maximum weight
- [ ] E) The standard deviation



### Clicker Question 5

**The box itself (the rectangular part) contains what percentage of the data?**

- [ ] A) 25%
- [ ] B) 50%
- [ ] C) 75%
- [ ] D) 95%
- [ ] E) 100%


---

### Visual 2.2: Boxplots Comparing Weight Across Species

*[Horizontal boxplots showing weight distributions for three hawk species with customized styling]*

---

<!-- #endregion -->

## Programming Interlude: Customizing Boxplot Marks

### Code Snippets 2.1

**Option A:**

<!-- #region -->
```python
# OPTION A
alt.Chart(hawks).mark_boxplot(
    outliers={'color': 'orange', 'size': 50, 'fill': 'black'},
    ticks=True,
    size=40
)

# OPTION B
alt.Chart(hawks).mark_boxplot(
    outliers=('color'= 'orange', 'size'= 50, 'fill'= 'black'),
    ticks={"size": 30},
)

# OPTION C
alt.Chart(hawks).mark_boxplot(
    outliers={'color': 'orange', 'size': 50, 'fill': 'black'},
    ticks={"size": 30},
    size=40
)

# OPTION D
alt.Chart(hawks).mark_boxplot(
    outliers=('color'= 'orange', 'size'= 50, 'fill'= 'black'),
    ticks={"size": 30},
    size = 40
)

# OPTION E

alt.Chart(hawks).mark_boxplot(
    outliers= ('color'= 'orange', 'size'= 50, 'fill'= 'black'),
    ticks= True,
    size = 40,
    
)
```


<!-- #endregion -->

<!-- #region -->
### Clicker Question 6 

**Which code snippet correctly customizes BOTH the tick size AND the outliers of the boxplot?**

- [ ] A) Option A
- [ ] B) Option B
- [ ] C) Option C
- [ ] D) Option D
- [ ] E) Option E


---

### Visual 2.3: Effect of the `extent` Parameter

*[Two boxplots side-by-side: Left shows extent=0.5, Right shows extent=3.0]*

### Clicker Question 7 

**What does changing the `extent` parameter from 0.5 to 3.0 do to the boxplot?**

- [ ] A) Changes the minimum and maximum values
- [ ] B) Changes the whisker length - showing fewer points as outliers
- [ ] C) Changes the number of outliers from 5 to 30
- [ ] D) Changes the standard deviation of the box plot
- [ ] E) Changes the whisker length - showing more points as outliers

---

### Visual 2.4: Customized Boxplots Comparing Three Species

*[Boxplots with multiple customizations applied, comparing Cooper's, Red-tailed, and Sharp-shinned Hawks]*

### Discussion Questions

**Looking at the three species:**
1. Which has the highest median weight?

   _____________________________________________________________________________

2. Which species shows the most outliers (unusual individuals)?

   _____________________________________________________________________________

3. What does it tell us that Red-tailed Hawks have so many outliers compared to the other species?

   _____________________________________________________________________________

   _____________________________________________________________________________

---

<!-- #endregion -->

## Faceting: Row vs. Column

### Visual 2.5a: Age Encoded by Color (Overlapping)

*[Weight by species with age shown using color - all on one chart]*

### Visual 2.5b: Faceted by Species (ROW)

*[Weight faceted by species using row, with age on y-axis - panels stacked vertically]*

.facet(row=alt.Row('species:N', title='Species'))

### Visual 2.5c: Faceted by Species (COLUMN)

*[Weight faceted by species using column, with age on y-axis - panels side-by-side]*

.facet(column=alt.Column('species:N', title='Species'))


<!-- #region -->
### Clicker Question 8

**When would you prefer `facet(row=...)` over `facet(column=...)`?**

- [ ] A) When you want to compare values on the SAME vertical (y) scale across groups
- [ ] B) When you want to compare values on the SAME horizontal (x) scale across groups
- [ ] C) When you have more than 5 groups
- [ ] D) When your data has missing values
- [ ] E) Row faceting is always better than column faceting


### Discussion Prompt

**When would you prefer to use COLOR vs. FACETS for showing categories? What are the trade-offs?**

Notes:
_____________________________________________________________________________

_____________________________________________________________________________

_____________________________________________________________________________

---

## Section 3: Violin Plots - Revealing Distribution Shape

### Visual 3.1: Histogram vs. Boxplot Comparison

*[Top: Histogram of tail length; Bottom: Boxplot of tail length]*

### Clicker Question 9

**What information does the HISTOGRAM show that the BOXPLOT hides?**

- [ ] A) The median value
- [ ] B) The presence of outliers
- [ ] C) The shape/symmetry of the distribution
- [ ] D) The range of values
- [ ] E) The number of data points

---

### Visual 3.2: Building a Violin Plot

*[Three panels: Left - Density curve (one-sided); Middle - Symmetrical violin plot; Right - Violin plots comparing three species]*

### Clicker Question 10

**What does the WIDTH of a violin plot at any given height represent?**

- [ ] A) The exact number of data points at that value
- [ ] B) The density/concentration of data at that value
- [ ] C) The median at that value
- [ ] D) The variance at that value
- [ ] E) The cumulative percentage of data below that value

---

### Visual 3.3: Violin Plots of Tail Length by Species

*[Violin plots of tail length faceted by species]*

### Clicker Question 11

**Which species appears to have a BIMODAL (two-peaked) distribution in tail length?**

- [ ] A) Cooper's Hawk
- [ ] B) Red-tailed Hawk
- [ ] C) Sharp-shinned Hawk
- [ ] D) All three show bimodal patterns
- [ ] E) None show bimodal patterns
---

### Matching Exercise

**Match each scenario to the best visualization type:**

**Scenarios:**
1. Want to see exact quartiles and outliers → _____
2. Want to see if distribution is symmetric vs. skewed → _____
3. Want to see multiple groups with bin counts → _____

**Options:**
- A) Histogram
- B) Boxplot
- C) Violin plot

---

## Section 4: Exploring Relationships & Simpson's Paradox

## Programming Interlude: Layering Charts

### Code Snippet 4.1: Building Layered Charts
<!-- #endregion -->

<!-- #region -->
```python
# Scatter plot
scatter = alt.Chart(hawks).mark_circle(
    size=40,
    opacity=0.6
).encode(
    x=alt.X('tail:Q', title='Tail Length (mm)'),
    y=alt.Y('weight:Q', title='Weight (g)'),
    color=alt.Color('species:N', legend=alt.Legend(title='Species'))
)

# Trend line for all data
regression = alt.Chart(hawks).mark_line(
    color='red',
    size=3
).transform_regression(
    'tail', 'weight' 
).encode(
    x='tail:Q',
    y='weight:Q'
)

# Layer them together
(scatter + regression).properties(
    width=500,
    height=400,
    title='Do Tail Length and Weight Correlate?'
)
```
<!-- #endregion -->


---

## Understanding Relationships

### Visual 4.1: Overall Correlation

*[Scatter plot of tail length vs. weight with single regression line, all data pooled]*

**Given Information:** Overall correlation = 0.875

### Clicker Question 12

**Based on this plot, what can we conclude about the relationship between tail length and weight?**

- [ ] A) Strong positive relationship - longer tails associated with heavier birds
- [ ] B) Weak relationship - tail length doesn't predict weight well
- [ ] C) Negative relationship - longer tails associated with lighter birds
- [ ] D) No relationship - tail and weight are independent
- [ ] E) Perfect relationship - tail length perfectly predicts weight

**Your Answer:** _____

---

### Visual 4.2: Within-Species Correlations

*[Same scatter plot with points colored by species and separate regression lines per species]*

**Given Information:**
- Cooper's Hawk within-species correlation: 0.26
- Red-tailed Hawk within-species correlation: 0.56
- Sharp-shinned Hawk within-species correlation: 0.76

### Clicker Question 13

**Why is the OVERALL correlation (0.875) so much higher than most of the WITHIN-SPECIES correlations?**

- [ ] A) There's an error in the calculation
- [ ] B) The strong overall correlation is driven by differences BETWEEN species (big species have both long tails and heavy weight)
- [ ] C) Tail length only matters for some species
- [ ] D) Outliers are artificially inflating the correlation
- [ ] E) The sample size is too small for accurate within-species correlations

**Your Answer:** _____

---

### Visual 4.3: Separate Scatter Plots by Species

*[Three separate scatter plots showing the weaker within-species relationships]*

### Discussion Prompt

**If you were studying hawk morphology, would you report the 0.875 correlation or the individual species correlations? Why does this matter? What could go wrong if you only reported the aggregate correlation? Can you think of a real-world scenario where this type of misleading correlation might cause problems?**

Notes:
_____________________________________________________________________________

_____________________________________________________________________________

_____________________________________________________________________________

_____________________________________________________________________________

---

## Next Steps:
Make sure you have worked through both Tutorial 6 and Class 6 files

