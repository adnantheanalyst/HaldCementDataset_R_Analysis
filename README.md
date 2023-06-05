# HaldCementDataset_R_Analysis

File Hald.dat contains the information about 13 cement mixture:
• column 1: Heat (cals/gm) evolved in setting, recorded to nearest tenth
• column 2: calcium aluminate
• column 3: tricalcium silicate
• column 4: tricalcium aluminoferrite
• column 5: dicalcium silicate.

it is well known that some of the chemicals are partly equivalent.
The interest is the evaluation of the relationship between the heat evolved in setting and
the chemicals.

Upload the data

cement <- read.table('hald.dat')
cement
 V1 V2 V3 V4 V5
1 78.5 7 26 6 60
2 74.3 1 29 15 52
3 104.3 11 56 8 20
4 87.6 11 31 8 47
5 95.9 7 52 6 33
6 109.2 11 55 9 22
7 102.7 3 71 17 6
8 72.5 1 31 22 44
9 93.1 2 54 18 22
10 115.9 21 47 4 26
11 83.8 1 40 23 34
12 113.3 11 66 9 12
13 109.4 10 68 8 12

Assign a name to the variables

colnames(cement) <- c('heat','cal_alu', 'tric_sil', 'tric_fer', 'dic_sil')

Some preliminary graphical analyses

boxplot(cement$heat, col='grey', main='Heat')


![Heat Variable Boxplot Rplot](https://github.com/adnantheanalyst/HaldCementDataset_R_Analysis/assets/16821246/510580b4-4fb3-4650-93c0-60e74a409e64)

pairs(cement)

![Residuals of the model](https://github.com/adnantheanalyst/HaldCementDataset_R_Analysis/assets/16821246/8e8f6b6e-52ea-4202-809a-e56b218468b3)

Construct a first model with cal alu as covariate

m.cement <- lm(heat ~ cal_alu, data=cement)
summary(m.cement)

Add on tric sil

m.cement2 <- lm(heat ~ cal_alu + tric_sil, data=cement)
summary(m.cement2)

Both the variables are significant; including tric sil moved R2 = 0.533948 to R2 =
0.9786784.
Add the remaining variables

m.cement3 <- lm(heat ~ cal_alu + tric_sil + tric_fer + dic_sil, data=cement)
summary(m.cement3)

No significant variable anymore...but R2 is still large...and F statistic would lead to reject the hypothesis of non-significant coefficients associated to all the covariates.... what’s wrong in the model?
Check the correlations among the variables

cor(cement)

Variables cal alu and tric fer are highly correlated as well as tric sil and dic sil. Including highly correlated variables in the model hides the effects on the response...the phenomenon is called multicollinearity. The practical solution is to maintain just one of the two correlated variables in the model. So we will refer to model m.cement2.
Residuals of the model

stand.res <- rstandard(m.cement2)
predictions <- fitted(m.cement2)
par(mfrow=c(2,2))
plot(stand.res)
abline(h=0, lty=2)
plot(predictions, stand.res, xlab='Predictions')
abline(h=0, lty=2)
plot(cement$cal_alu, stand.res)
abline(h=0, lty=2)
plot(cement$tric_sil, stand.res)
abline(h=0, lty=2)



![Residuals of the model](https://github.com/adnantheanalyst/HaldCementDataset_R_Analysis/assets/16821246/c1dda09a-3b2e-4668-b653-8acc1e931289)

There are no deterministic patterns.
