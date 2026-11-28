# Education and Salary Analysis

## 1. Research Question

**Do occupations that typically require higher levels of education tend to have higher median annual salaries?**

This question matters because education requires an investment of time and money. Understanding how education requirements relate to occupational earnings may help people make more informed career and education decisions.

## 2. Hypothesis

**Alternative hypothesis:** Occupations that typically require higher levels of education tend to have higher median annual salaries.

**Null hypothesis:** There is no relationship between an occupation's typical education requirement and its median annual salary.

## 3. Data Description

The data comes from the [**Career Explorer Occupational Level Data**](https://catalog.data.gov/dataset/career-explorer-occupational-level-data), published by **Iowa Workforce Development / Labor Market Information** and accessed through **Data.gov**.

The dataset includes information about occupations, wages, education requirements, employment, geographic areas, skills, and other career-related characteristics.

Each observation represents an occupation within a specific geographic area, so the same occupation may appear more than once across different locations.

### Project Scaffold Table

| Element | My Plan |
|---|---|
| **Topic / Question** | Do occupations that typically require higher levels of education tend to have higher median annual salaries? |
| **Hypothesis** | Occupations that typically require higher levels of education tend to have higher median annual salaries. |
| **Outcome / Metric / Test Statistic** | I will analyze `typical_education_requirement` and `median_annual_salary`. My main comparison will examine differences in median annual salary across education requirement levels. |
| **Units of Analysis** | Each observation represents an occupation within a geographic area, along with wage, education, employment, and other occupational information. |
| **Data Source(s)** | [**Career Explorer Occupational Level Data**](https://catalog.data.gov/dataset/career-explorer-occupational-level-data), published by Iowa Workforce Development / Labor Market Information and accessed through Data.gov. |
| **Why this data works** | The dataset includes both typical education requirements and median annual salary, which directly support my research question. It also includes geographic and occupational variables that can help explain possible differences in the data. |
| **Uncertainty Metric** | I plan to use bootstrapping to estimate uncertainty for a salary-based metric such as median annual salary. |
| **Null Hypothesis** | There is no relationship between an occupation's typical education requirement and its median annual salary. |

## Project Notebooks

- `01_eda.ipynb` — Exploratory data analysis, data quality checks, salary distributions, outlier analysis, and education-level comparisons.
- `02_hypothesis_test.ipynb` — Permutation test for the main hypothesis. *(To be completed.)*
- `03_bootstrap_analysis.ipynb` — Bootstrap uncertainty analysis. *(To be completed.)*

## 4. Methods

The analysis is organized into three notebooks:

01_eda.ipynb — exploratory data analysis, data quality checks, salary distributions, outlier analysis, and comparisons across education requirements.

02_hypothesis_test.ipynb — permutation test comparing occupations requiring a Bachelor's Degree with occupations requiring a High School Diploma or Equivalent.

03_bootstrap_analysis.ipynb — bootstrap uncertainty analysis for the mean and median salary differences.

For the hypothesis test, I used the difference in mean median annual salary as the test statistic:

\bar{x}_{\text{Bachelor's}}

\bar{x}_{\text{High School}}
$$

Under the null hypothesis, education requirement is not associated with salary. I randomly shuffled the education labels 10,000 times while keeping the salary values unchanged and recalculated the mean difference after each permutation.

For uncertainty estimation, I generated 10,000 bootstrap samples with replacement from each education group. I calculated confidence intervals for two metrics:

1. Difference in mean median annual salary

2. Difference in median median annual salary

The median difference was included as a non-CLT metric because the Central Limit Theorem does not provide the same simple standard error formula for the sample median as it does for the sample mean.

## 5. Results

Occupations requiring a Bachelor's Degree had a mean median annual salary of approximately $72,307.39, compared with approximately $46,600.90 for occupations requiring a High School Diploma or Equivalent.

The observed difference in means was:

$25{,}706.49
$$

None of the 10,000 permuted differences were as large as the observed difference. Therefore, the estimated one-sided p-value was less than:

$$
0.0001
$$

Because the p-value is much smaller than a significance level of:

$$
\alpha = 0.05
$$

I reject the null hypothesis.

The results provide strong statistical evidence that occupations requiring a Bachelor's Degree have a higher mean median annual salary than occupations requiring a High School Diploma or Equivalent.

## 6. Uncertainty Estimation

I used 10,000 bootstrap resamples to estimate uncertainty around both the mean and median salary differences.

The observed mean salary difference was $25,706.49, with a 95% bootstrap confidence interval of:

$$
[$25{,}167.28,\ $26{,}242.48]
$$

The observed median salary difference was $20,136.00, with a 95% bootstrap confidence interval of:

$$
[$19{,}618.00,\ $20{,}617.00]
$$

Both confidence intervals are entirely above zero, indicating that the salary difference remains positive across repeated bootstrap samples.

The median-based interval is especially important because the median is not a metric for which the Central Limit Theorem provides the same direct standard error calculation as the mean. Bootstrapping therefore provides a practical way to quantify uncertainty for this metric.

## 7. Limitations

This analysis has several limitations.

First, the dataset contains repeated occupations across different geographic areas, so the observations are not all unique occupations. Salary differences may therefore reflect geographic variation as well as education requirements.

Second, approximately 4.92% of the `median_annual_salary` values are missing. Rows with missing salary values were excluded from salary-based comparisons, which may introduce some bias if the missing values are not random.

Third, education requirement is not the only factor that affects salary. Occupation type, industry, geographic location, labor demand, experience, and other factors may also influence earnings.

Finally, the hypothesis test compares only occupations requiring a Bachelor's Degree with occupations requiring a High School Diploma or Equivalent. Therefore, the statistical conclusion should not be generalized automatically to every education category in the dataset.

## 8. BS-Audit

1. Question the Source

The data was published by Iowa Workforce Development / Labor Market Information for the Career Explorer. The dataset is intended to provide occupational information such as wages, education requirements, employment, and career characteristics.

One limitation is that the data reflects occupations and geographic areas in Iowa, so the results may not represent salary patterns across the entire United States. In addition, some salary values are missing, which may affect the analysis if those values are not missing randomly.

2. Check the Comparisons

The hypothesis test compares occupations requiring a Bachelor's Degree with occupations requiring a High School Diploma or Equivalent.

One possible unfairness is that the two groups may contain very different types of occupations. For example, differences in industry, occupation type, experience requirements, geographic area, or labor demand could influence salary in addition to education level.

To keep the comparison consistent, I used the same salary variable, median_annual_salary, and excluded observations with missing salary values from both groups.

3. Interrogate a Too-Good Result

The permutation test produced an extremely small p-value, and both bootstrap confidence intervals were entirely above zero. This result may appear unusually strong.

However, the difference was also visible during the exploratory analysis, where occupations requiring higher levels of education generally had higher salary distributions. The result was not created by removing high salaries or selecting only observations that supported the hypothesis. The same filtering rules were applied consistently to both groups.

4. Sanity-Check the Magnitude

The observed mean salary difference was approximately:

$$
$25{,}706
$$

The Bachelor's Degree group had a mean median annual salary of about:

$$
$72{,}307
$$

while the High School Diploma or Equivalent group had a mean median annual salary of about:

$$
$46{,}601
$$

A difference of about $25,700 per year is large but plausible when comparing groups of occupations with substantially different typical education requirements.

5. Name the Confirmation Bias

Before conducting the analysis, I expected occupations requiring higher levels of education to have higher salaries. This expectation could have influenced how I framed the research question or which groups I chose to compare.

I would have reconsidered my hypothesis if the permutation test had produced a large p-value, if the bootstrap confidence intervals included zero, or if the salary distributions showed little or no difference between the education groups.

## 9. References

- [Career Explorer Occupational Level Data — Iowa Workforce Development / Labor Market Information](https://catalog.data.gov/dataset/career-explorer-occupational-level-data)
- [Data.gov](https://data.gov/)
