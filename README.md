# HaldCementDataset_R_Analysis

I used file Hald.dat which contained the information about 13 cement mixture:<br>

• column 1: Heat (cals/gm) evolved in setting, recorded to nearest tenth<br>
• column 2: calcium aluminate<br>
• column 3: tricalcium silicate<br>
• column 4: tricalcium aluminoferrite<br>
• column 5: dicalcium silicate.<br>

it is well known that some of the chemicals are partly equivalent.<br>
My interest was to evaluate of the relationship between the heat evolved in setting and the chemicals.<br>

Uploaded the data<br>

cement <- read.table('hald.dat')<br>
cement<br>
   V1   V2 V3 V4 V5<br>
1  78.5 7 26 6 60<br>
2  74.3 1 29 15 52<br>
3  104.3 11 56 8 20<br>
4  87.6 11 31 8 47<br>
5  95.9 7 52 6 33<br>
6  109.2 11 55 9 22<br>
7  102.7 3 71 17 6<br>
8  72.5 1 31 22 44<br>
9  93.1 2 54 18 22<br>
10 115.9 21 47 4 26<br>
11 83.8 1 40 23 34<br>
12 113.3 11 66 9 12<br>
13 109.4 10 68 8 12<br>

Then, I Assigned a name to the variables<br>

colnames(cement) <- c('heat','cal_alu', 'tric_sil', 'tric_fer', 'dic_sil')<br>

AFter that I did some preliminary graphical analyses<br>

boxplot(cement$heat, col='grey', main='Heat')<br>


![Heat Variable Boxplot Rplot](https://github.com/adnantheanalyst/HaldCementDataset_R_Analysis/assets/16821246/510580b4-4fb3-4650-93c0-60e74a409e64)

pairs(cement)<br>

![Residuals of the model](https://github.com/adnantheanalyst/HaldCementDataset_R_Analysis/assets/16821246/8e8f6b6e-52ea-4202-809a-e56b218468b3)

Constructed my first model with cal alu as covariate<br>

m.cement <- lm(heat ~ cal_alu, data=cement)<br>
summary(m.cement)<br>

Added on tric sil<br>

m.cement2 <- lm(heat ~ cal_alu + tric_sil, data=cement)<br>
summary(m.cement2)<br>

Both the variables were significant; including tric sil moved R2 = 0.533948 to R2 = 0.9786784.<br>
Added the remaining variables<br>

m.cement3 <- lm(heat ~ cal_alu + tric_sil + tric_fer + dic_sil, data=cement)<br>
summary(m.cement3)<br>

No significant variable were anymore...but R2 was still large...and F statistic would lead to reject the hypothesis of non-significant coefficients associated to all the covariates.... what was wrong in the model?<br>
After that I checked the correlations among the variables<br>

cor(cement)<br>

Variables cal alu and tric fer were highly correlated as well as tric sil and dic sil. Included highly correlated variables in the model which I found that hide the effects on the response...the phenomenon is called multicollinearity. The practical solution was to maintain just one of the two correlated variables in the model. So after that I refered to model m.cement2.<br>
Residuals of the model<br>

stand.res <- rstandard(m.cement2)<br>
predictions <- fitted(m.cement2)<br>
par(mfrow=c(2,2))<br>
plot(stand.res)<br>
abline(h=0, lty=2)<br>
plot(predictions, stand.res, xlab='Predictions')<br>
abline(h=0, lty=2)<br>
plot(cement$cal_alu, stand.res)<br>
abline(h=0, lty=2)<br>
plot(cement$tric_sil, stand.res)<br>
abline(h=0, lty=2)<br>



![Residuals of the model](https://github.com/adnantheanalyst/HaldCementDataset_R_Analysis/assets/16821246/c1dda09a-3b2e-4668-b653-8acc1e931289)

At last I found that there are no deterministic patterns.<br>
