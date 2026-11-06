#An Exploratory Analysis of Students' Mathematics Achievement, Learning Behaviour and Socioeconomic Background.

**Author:** Takramah Florence Afua
**Course / assignment:** Data Analytics/Capstone project
**Date:** 6th November, 2026

## Overview

This project analyses factors associated with secondary school students' final mathematics grades. It uses the Mathematics dataset from the UCI Student Performance data (students at two schools in Portugal) and addresses five research questions:

1. Does the amount of time students spend studying influence their mathematics performance?
2. Is there a relationship between school attendance and mathematics achievement?
3. Does parental educational background influence students' mathematics performance?
4. Do students receiving additional academic support perform better?
5. Can students' final mathematics grades be predicted using their academic and demographic characteristics?

## Dataset

- **Source:** UCI Machine Learning Repository, Student Performance dataset: https://archive.ics.uci.edu/dataset/320/student%2Bperformance
- **File used:** `student-mat.csv` (Mathematics course), semicolon-separated
- **Size:** 395 students, 33 variables, no missing values
- **Target variable:** `G3`, the final grade (0 to 20)
- **Other grade variables:** `G1` and `G2` are the first- and second-period grades
- **Citation:** Cortez, P. (2008). *Student Performance* [Dataset]. UCI Machine Learning Repository. https://doi.org/10.24432/C5TG7T
- **Licence:** CC BY 4.0

The raw data file is not modified. All cleaning is done on a copy inside the notebook.

## Data cleaning

- 38 students with a final grade (`G3`) of 0 were removed. All of them also had 0 recorded absences, which likely reflects missing records (for example, students who did not sit the final assessment), not genuine zero scores and perfect attendance.
- 357 students remain for analysis.
- Only the nominal variables (for example, `school`, `sex`, `schoolsup`) were converted to the category type. Ordinal scales (for example, `studytime`, `Medu`, `Fedu`, `failures`) were kept as numbers so that rank-based tests work.
- For Question 3, parental education level 0 (3 mothers, 2 fathers) was merged with level 1 because of the very small group sizes.

## Methods

| Question | Methods |
|---|---|
| 1. Study time | Group means, boxplot, Kruskal-Wallis test, Spearman correlation, pairwise Mann-Whitney U tests with Bonferroni correction |
| 2. Attendance | Scatterplot, Spearman correlation, mean grade by absence band |
| 3. Parental education | Spearman correlations, Kruskal-Wallis tests, mother-father correlation, cross-tabulation of mean grades |
| 4. Additional support | Mann-Whitney U tests, comparison of first-period grades (`G1`) and grade change from `G1` to `G3` |
| 5. Prediction | Linear regression and random forest, 80/20 train-test split, 5-fold cross-validation, feature importance |

Non-parametric tests were used because the main predictors are ordinal or skewed (for example, absences) and some group sizes are small and uneven.

For Question 5, `G1` and `G2` were excluded from the predictors, because they are closely tied to `G3` and would make prediction trivial.

## Key findings

- **Study time:** significantly but weakly associated with final grade (Kruskal-Wallis p = 0.011; Spearman rho = 0.11). Students studying 5 or more hours scored higher, but after Bonferroni correction only the 2-5 hour vs 5-10 hour comparison was significant.
- **Attendance:** more absences were associated with lower grades (rho = -0.244, p < 0.001).
- **Parental education:** higher parental education was associated with higher grades (mother rho = 0.192, father rho = 0.164). The two are strongly correlated (rho = 0.617), so their separate effects cannot be cleanly separated.
- **Additional support:** no type of support was associated with higher grades. Students with school support scored lower, but they already had lower first-period grades.
- **Prediction:** without earlier grades, the models explained about 20-23% of the variance on the test set and about 16% under cross-validation (weak and unstable).

All findings are associations in observational data, not causal effects.

## Requirements

- Python 3
- pandas
- numpy
- matplotlib
- seaborn
- scipy
- scikit-learn
- Jupyter Notebook or Google Colab

Install the libraries with:

```
pip install pandas numpy matplotlib seaborn scipy scikit-learn
```

## How to run

1. Download `student-mat.csv` from the UCI link above (it is inside the zip file) and place it in the same folder as the notebook.
2. Open the notebook in Jupyter or Google Colab.
3. Run all cells from top to bottom (in Jupyter: Kernel > Restart & Run All). Some cells depend on earlier ones, so the order matters.

## Project structure

```
.
├── README.md
├── [notebook-name].ipynb     # analysis notebook with code and written findings
└── student-mat.csv           # raw data (not modified)
```

## Limitations

- Observational data, so all results show associations only.
- Small sample (357 students) from two schools in Portugal, so results may not generalise.
- Study time and several other variables are self-reported in broad categories.
- Students receiving support are not chosen at random, so Question 4 cannot show whether support works.
- Prediction results varied considerably between cross-validation folds.