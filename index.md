---
layout: default
---

## Introduction
Many high school students take test prep courses because they want to improve their academic performance, but whether test prep is associated with meaningful improvement is not always clear. In this report, I use the Kaggle “Students’ Performance in Exams” dataset ($n = 1000$) to study math achievement and test-prep participation in a broader population of similar students. This question is relevant to students, families, and educators because test prep can take substantial time and money, and schools may use evidence like this when deciding whether to recommend or support prep programs. The main question is: “Do students who complete a test preparation course tend to score higher in math than students who do not?” To express this comparison, let $X$ be the math score of a randomly selected student who completed test prep and let $Y$ be the math score of a randomly selected student with no test prep, and define the difference random variable $D = X - Y$. I estimate the typical value of $D$ using two estimators, the mean of differences and the median of differences, and then use resampling methods to compare these estimators and answer the research question.

## Data description
The dataset used in this report contains 1,000 observations, where each row represents a unique high school student. It includes student demographic information, whether the student completed a test preparation course, and exam scores in math, reading, and writing. The population of interest is students similar to those represented in this Kaggle dataset who take standardized exams. The single random variable analyzed in this report is Math Score, a numerical value bounded between 0 and 100.

## Preliminary distributions
After separating the prep and non-prep groups, I looked at the distributions of math scores in each group, which helps show why a simple Normal model may not be appropriate.

Visual 1....

Both groups have math scores between 0 and 100, and neither histogram looks perfectly symmetric like a bell curve. A Normal distribution might seem like a reasonable model at first because test scores are numeric and often cluster around a middle value, but the Normal model is not appropriate here because the scores are bounded (0–100) and the histograms show departures from a bell shape. In addition, the full dataset is not one i.i.d. sample from a single distribution because it combines two different groups (completed vs. none), which can have different score distributions. In this analysis, I treat students within each group as independent observations, but this assumption may be violated if students are clustered within the same schools or classrooms. Because of these features, I avoid relying on a Normal distribution model and instead use bootstrap resampling within each group to approximate the sampling distributions of my estimators.

## Estimators
I compare two estimators of the central tendency of $D$. The first estimator is the mean of differences, $T_1 = \mathrm{mean}(D)$, which estimates the average score advantage. The second estimator is the median of differences, $T_2 = \mathrm{median}(D)$, which estimates the typical score advantage and is more robust to skewness and outliers. Both estimators target the same general population characteristic—the typical size of the score difference between the completed and no-prep groups—but summarize it in slightly different ways.

Because the prep and no-prep groups are separate samples with no natural one-to-one matching, I form random cross-group pairs to create a sample of differences that approximates draws of $D = X - Y$. Using these paired differences, I compute the observed mean of differences and observed median of differences as baseline estimates before applying the bootstrap procedure.

Observed mean of differences (T1_obs)   = 5.704 
Observed median of differences (T2_obs) = 6.5 

## Bootstrap method
To see how much these estimators could change from sample to sample, I use a nonparametric bootstrap. In each bootstrap run, I resample math scores with replacement from the completed group and resample the same number of scores with replacement from the no-prep group. I subtract the resampled scores to create a bootstrap set of differences $D^{*} = X^{*} - Y^{*}$, then compute the mean of differences $T_1^{*} = \mathrm{mean}(D^{*})$ and the median of differences $T_2^{*} = \mathrm{median}(D^{*})$. Repeating this many times gives empirical sampling distributions for $T_1$ and $T_2$, which lets me compare their variability and uncertainty.

$D^{*} = X^{*} - Y^{*}, \qquad T_1^{*} = \mathrm{mean}(D^{*}), \qquad T_2^{*} = \mathrm{median}(D^{*})$

visual 2....

## Results
The bootstrap histograms show the empirical sampling distributions of the mean of differences and the median of differences. Each histogram is centered near the corresponding observed estimate, shown by the red vertical line, and the spread of each distribution represents how much the estimator would vary across repeated samples. Visually, the more concentrated histogram corresponds to the estimator that is more stable.

visual 3 table .....

To compare the estimators, I use the bootstrap standard errors and 95% confidence intervals shown in the table above. The mean of differences estimator $(T_1)$ has a smaller bootstrap standard error (1.114 vs. 1.375) and a narrower 95% interval $[3.363,\ 7.752]$ than the median of differences estimator $(T_2)$, which has a 95% interval $[3.000,\ 8.000]$. This indicates that $T_1$ is more stable across repeated samples and provides a more precise estimate of the typical score advantage. For that reason, I use $T_1$ to answer the research question.

## Conclusion
Using the mean of differences as the preferred estimator, the estimated typical math-score advantage for students who completed test prep is about 5.6 points. In other words, when comparing a randomly selected student who completed test prep to a randomly selected student who did not, the completed student is expected to score roughly 5–6 points higher in math on average. The 95% bootstrap percentile confidence interval for this advantage is $[3.363,\ 7.752]$. Because the entire interval is above 0, the results provide evidence that the population-level average difference in math scores between the completed and no-prep groups is positive. Overall, within the population represented by this dataset, completing a test preparation course is associated with higher math performance by several points on a 0–100 scale.

## Limitations
This dataset is observational, so the results may not generalize broadly beyond students similar to those in this sample. Students who complete test prep may differ from those who do not in other ways (motivation, prior achievement, resources, etc.), which could explain part of the difference.

## References

Adeyemi, Timothy. “Students Performance in Exams.” Kaggle Datasets. https://www.kaggle.com/datasets/timothyadeyemi/students-performance-in-exams

<script>
  window.MathJax = {
    tex: {
      inlineMath: [['$', '$'], ['\\(', '\\)']],
      displayMath: [['$$','$$'], ['\\[','\\]']]
    },
    options: { skipHtmlTags: ['script','noscript','style','textarea','pre','code'] }
  };
</script>
<script defer src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>



