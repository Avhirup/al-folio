---
layout: post
title:  Relation among variables
date:   2015-03-15 16:40:16
description: association among different types of variables
published: true
---
Association between variables serve to help exploit predictive properties.These **associations/dependence** help us in finding relevant features,finding highly corelated variables etc. Random variables could be classified into continous(interval & ratio) or categorical(nominal & oridinal).This [link](https://www.spss-tutorials.com/measurement-levels/#ordinal-variable) talks more about it.  

### Correlation between two continuous variable
Pearson's Coefficient gives an indication of linear dependence of two variables.For positive correlation,as one variable increases,so does the other.For a negatively correlation,as one variable increases, other variable decreases. 

```python
#Pandas
import pandas as pd
import seaborn as sns
df=pd.read(f"{file_path_to_dataframe}")
#calculate the correlation matric
corr=df.corr(method='pearson') 
sns.heatmap(corr,
	xticklabels=corr.columns,
	yticklabels=corr.columns)

```

### Correlation between two categorical variable 
In the case of categorical variables, we cant define correlation in true sense.Though there are other methods to see relation between two variables. 
#### Ordinal Variables
For ordinal variables, we could rank the observations and measure "correlation" between ordinal variables. 
* [Spearman correlation](https://en.wikipedia.org/wiki/Spearman%27s_rank_correlation_coefficient) : Spearman's correlation coefficient, (ρ, also signified by rs) measures the strength and direction of association between two ranked variables.This metric could work for ordinal,interval & ratio variables.It varies between therange of [-1,1] with 0 implying no correlation.
```python
from scipy import stats
x,y=[1,2,3,4,5], [5,6,7,8,7]
rho,pval=stats.spearmanr(x,y)
```
* [Kendall's Tau](https://en.wikipedia.org/wiki/Kendall_rank_correlation_coefficient)"Kendall’s tau is a measure of the correspondence between two rankings. Values close to 1 indicate strong agreement, values close to -1 indicate strong disagreement"
```python
from scipy import stats
tau_stat,pvalue=stats.kendalltau(x,y)
```

#### Nominal Variables
Unlike ordinal variables,nominal variables dont have anything as "order", hence ranking them doesnt make sense. There are alternative methods to see cases like these. 
* [Cramer's Coefficient](http://mlwiki.org/index.php/Cramer%27s_Coefficient).

```python
def cramers_corrected_stat(confusion_matrix):
    """ calculate Cramers V statistic for categorial-categorial association.
        uses correction from Bergsma and Wicher, 
        Journal of the Korean Statistical Society 42 (2013): 323-328
    """
    chi2 = ss.chi2_contingency(confusion_matrix)[0]
    n = confusion_matrix.sum()
    phi2 = chi2/n
    r,k = confusion_matrix.shape
    phi2corr = max(0, phi2 - ((k-1)*(r-1))/(n-1))    
    rcorr = r - ((r-1)**2)/(n-1)
    kcorr = k - ((k-1)**2)/(n-1)
    return np.sqrt(phi2corr / min( (kcorr-1), (rcorr-1)))

import pandas 
confusion_matrix=pd.crosstab(df[column1],df[column2])
```

* [Chi squared Contingency](https://machinelearningmastery.com/chi-squared-test-for-machine-learning/)
The chi-square test of independence works by comparing the categorically coded data that you have collected (known as the observed frequencies) with the frequencies that you would expect to get in each cell of a table by chance alone (known as the expected frequencies).

```python
import pandas
from scipy.stats import chi2_contingency

def chisq_of_df_cols(df, c1, c2):
    groupsizes = df.groupby([c1, c2]).size()
    ctsum = groupsizes.unstack(c1)
    # fillna(0) is necessary to remove any NAs which will cause exceptions
    return(chi2_contingency(ctsum.fillna(0)))

test_df = pandas.DataFrame([[0, 1], [1, 0], [0, 2], [0, 1], [0, 2]], columns=['var1', 'var2'])
tab = pd.crosstab(df.A > 0, df.B > 0)
chisq_of_df_cols(test_df, 'var1', 'var2')
#or
chisq,pval,dof,expected=chi2_contingency(tab)
```

### Correlation between a categorical & continuos variable
* [one-way Anova](http://mlwiki.org/index.php/One-Way_ANOVA_F-Test).


> References :
* [Spearmans rank order correlation](https://statistics.laerd.com/statistical-guides/spearmans-rank-order-correlation-statistical-guide.php)
* [Different types of variables](https://www.spss-tutorials.com/measurement-levels/#ordinal-variable)
* [Best Question](https://stats.stackexchange.com/questions/119835/correlation-between-a-nominal-iv-and-a-continuous-dv-variable/124618#124618)
* [Correlation and dependence](https://en.wikipedia.org/wiki/Correlation_and_dependence)
* [Statistical hypothesis testing](https://en.wikipedia.org/wiki/Statistical_hypothesis_testing)
* [Chi-squared test](https://en.wikipedia.org/wiki/Chi-squared_test)
* [Cramér's V](https://en.wikipedia.org/wiki/Cram%C3%A9r%27s_V)
