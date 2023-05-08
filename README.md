Download Link: https://assignmentchef.com/product/solved-pols6481-lab7
<br>
The objective of the first part of lab is to compare the <em>linear </em>model to a couple <em>nonlinear</em> models, i.e., models that fit curves other than a straight line through the mean of the data. Comparing specifications by examining model fit statistics is somewhat complicated when the dependent variable has been logarithmically transformed, and interpretation of regression coefficients is less straightforward, but some procedures may be applied to help with interpretation.




The objective of the second part of lab is to compare the <em>linear</em> regression model to the <em>Poisson </em>regression model when the dependent variable takes on integer values. After showing how to estimate the model, we will explore interpreting coefficients and generating predicted values.




<ol>

 <li><strong> Datasets</strong>: <em>OECD_health.dta</em>, <em>crab.txt</em></li>

</ol>




<strong>III. Packages</strong>:  <em>foreign</em>, <em>white-test.R</em>, <em>sandwich</em>




<ol>

 <li><strong> Preparation</strong></li>

</ol>

<ul>

 <li>Open RStudio by double-clicking the icon or selecting RStudio from the Windows Start menu.</li>

 <li>Perform a Git pull to update the latest scripts and data</li>

</ul>

3) Open both R scripts by typing <em>Ctrl</em>+<em>O</em> or by clicking on File in the upper-left corner, using the dropdown menu, and navigating to the scripts in the subdirectory Lab 7 – March 29 in your Project Directory.




<ol>

 <li><strong> Instructions for Part 1</strong></li>

</ol>




This lab starts with exploring the life expectancy data that we examined at the beginning of Lecture 13. Begin by running line 1-2  in the R script “<em>Lab 07 health.R</em>” to load the packages that you will need to open the datasets. Next, load the health data, as shown in line 4:




health &lt;- read.dta(here(“data”,”OECD_health.dta”))

Alternatively, you can modify the script to specify the full path to the data:

&gt; health &lt;- read.dta(“[full path to data]:/OECD_health.dta”)




If you type in the following two lines of code, you will see a scatterplot of average life expectancy against health expenditures, with a linear regression line. (<strong>Lines 6 and 7 in the script show an alternative method of creating the same plot</strong>):

&gt; plot(health$health_expend, health$life_expect, pch=19)

&gt; abline(lm(health$life_expect ~ health$health_expend))




Two potential outliers are far below the regression line; these are South Africa, whose expenditures are at the low end of the normal range but whose life expectancy is below 55, and the United States, whose life expectancy is in the normal range but whose expenditures are thousands of dollars above other countries. Run line 3 in the R script to subset the data, omitting South Africa:

&gt; subset &lt;- subset(health, country != “South Africa”)

Next, run lines 5–6 to plot the data again without South Africa.




<strong>The new lines 14-29 in the script illustrate the nonlinear nature of the data with two plots, using the <em>ggplot2 </em>package. If you don’t have this package installed, uncomment the code in line 11 and install the package.</strong>







Run line 31 to change the dimensions of the <strong>Plots</strong> window so that it will display a few plots side-by-side.

Next, run lines 32-33 to examine the normality of the dependent variable (<em>life_expect</em>) visually. The left side displays a histogram of <em>life_expect</em>; any deviation from a bell curve would indicate a non-normal distribution. The right side displays the normal quantile plot of <em>life_expect </em>against the normal distribution; any deviation from a straight line indicates a non-normal distribution.

&gt; hist(subset$life_expect, freq = FALSE)

&gt; qqnorm(subset$life_expect); qqline(subset$life_expect)




For a comparison, run lines 34-35 to examine the normality of the natural log of <em>life_expect</em>.

&gt; hist(log(subset$life_expect), freq = FALSE)

&gt; qqnorm(log(subset$life_expect)); qqline(log(subset$life_expect))




Do you notice any meaningful differences between the distributions of <em>life_expect</em> and <em>ln</em>(<em>life_expect</em>)?




You might like to see what the normal quantile plot and histogram look like for a variable that has better distributional properties. Run lines 36-37 for the normal quantile plot and histogram of our main independent variable (<em>health_expend</em>).

&gt; hist(subset$health_expend, freq = FALSE)

&gt; qqnorm(subset$health_expend); qqline(subset$health_expend)




Make sure to run line 38 to reset the dimensions of the <strong>Plots</strong> window.




Before we explore the linear regression results, run either lines 40-41 of the R script (without South Africa) or the following two lines of code for another scatterplot of the <em>log</em> of life expectancy against the <em>log</em> of health expenditures, with a linear regression line <em>utilizing the log-transformed variables</em>.

(<strong>This code for survey question 1)</strong>

&gt; plot(log(life_expect) ~ log(health_expend), data = health, pch=19)

&gt; abline(lm(log(health$life_expect) ~ log(health$health_expend)))

Does this relationship look more linear than the scatterplot of <em>life­_expect </em>against <em>heatlth_expend</em>?




Let us turn our attention to the bivariate relationships between health expenditures and average life expectancy. Begin by running line 43 to recode the health expenditures data, dividing by $1000. Next, run line 44 to estimate the linear model and display the results of the estimation.

&gt; linlin &lt;- lm(life_expect ~ health.1k, subset); summary(linlin)

Before moving on, think about how you should interpret the regression coefficient. That is, complete the sentence, “A $1000 increase in health expenditures leads to a ______ years increase in life expectancy.”




Run line 45 to plot the residuals against the regressor. Do the residuals look well behaved? That is, do the residuals fit our expectations of zero conditional expectation given income and constant variance?

&gt; plot(subset$health.1k, linlin$residuals, pch=19); abline(h=0)

It would not be unwise to run a Breusch-Pagan or White test for heteroscedasticity at this point. These tests should indicate that heteroscedasticity is a problem for the linear-linear regression model.




Even though <em>health_expend </em>was well-behaved, run line 47 to estimate the simple regression of average life expectancy on the natural log of health expenditures (divided by 1000) and display the results.

&gt; linlog &lt;- lm(life_expect ~ log(health.1k), subset); summary(linlog)

What was the R<sup>2</sup> from linear-linear model? _______ What is the R<sup>2</sup> from the linear-log model? _______

Are these two quantities directly comparable?




Next, look up the rules for interpretation that we covered in lecture 13. (Wooldridge’s Table 2.3 has been reprinted on page 4 to assist you.) Interpret the simple regression coefficient by completing the sentence, “A one-percent increase in health expenditures leads to a ______ years increase in life expectancy.”




Run line 48 to plot the residuals. Before moving on, you should examine whether these residuals look better behaved than the residuals from the linear-linear model.

&gt; plot(log(subset$health.1k), linlog$residuals, pch=19); abline(h=0)

In addition to visually inspecting the residuals, you can and should test for heteroscedasticity. Sadly the <em>white.test </em>function (in the <em>white-test.R </em>script) does not work when we log-transform the variables in coding the regression. As shown with the code in lines 50-52, you could run a Breusch-Pagan or White test by creating the squared residuals and regressing those on <em>health.1k </em>and/or <em>health.1k</em><sup>2</sup>.




Earlier in lab you plotted the natural log of life expectancy against the natural log of health expenditures. Run line 54 to estimate the simple regression <em>ln</em>(<em>life_expect</em>) on <em>ln</em>(<em>health_expend/$1000</em>):

&gt; loglog &lt;- lm(log(life_expect) ~ log(health.1k), subset)

&gt; summary(loglog)

What is the R<sup>2</sup> from the log-log model? _______ Is this quantity directly comparable to the earlier R<sup>2</sup>’s? Why or why not?




Using the rules for interpretation that we covered in lecture 13, interpret the regression coefficient by completing the sentence, “A one-percent increase in health expenditures leads to a ______ percent increase in life expectancy.”




Run line 55  to plot the residuals from this model:

&gt; plot(log(subset$health.1k), loglog$residuals, pch=19); abline(h=0)

You should observe that the residuals from the linear-log and log-log models look quite alike, and they also look quite unlike the residuals from the linear-linear model.




To answer a question that I asked a few paragraphs ago, you cannot directly compare the R<sup>2</sup> of a log-log model against the R<sup>2</sup> of either a linear-linear or a linear-log model, because the scale of the independent variable has changed. Pay close attention to the residual standard error. The fact that we manipulated the dependent variable dramatically altered the variation around the regression line. We will correct this now.




(For more on R-squared and transformed variables, and cautions about proper use of R-squared generally: <a href="https://data.library.virginia.edu/is-r-squared-useless/">https://data.library.virginia.edu/is-r-squared-useless/</a> )




Lines 57-60 in the R script walk you through the steps required to compare the models fairly. Recall that the dependent variable in the regression is the natural log of life expectancy, so if you save the predicted values from the model you will have the predicted <em>log</em>(<em>life_expect</em>). Line 57 exponentiates the residuals from the log-log model, and line 58 finds the average exponentiated residual. Line  59 creates new fitted values of <em>life_expect </em>by multiplying the average exponentiated fitted value times the exponentiated residual. Finally, line 60 regresses test scores on the predictions based on the log-log model.

&gt; expu &lt;- exp(loglog$residuals)

&gt; alph &lt;- mean(expu)

&gt; lifehat &lt;- alph*exp(fitted(loglog))

&gt; logcheck &lt;- lm(subset$life_expect~lifehat); summary(logcheck)




If you chose to use the log-log model, then the Residual standard error and R-squared from the regression in line 60 should be used in place of line 54’s model fit statistics.




If you want to compare the R<sup>2</sup>’s from the log-log and level-log models, you can type the following code:

&gt; summary(logcheck)$r.squared; summary(linlog)$r.squared




To compare the models, run line 65 to plot <em>life_expect </em>against expenditures. Line 66 adds fitted values from the log-log model in red. Line 67 adds the curve from the linear-linear bivariate model in purple.

<u> </u>

You have estimated three models: level-level, level-log, and log-log. Which has better properties (for instance, in terms of homoscedasticity)? Which seems to fit the data best? Which is easiest to interpret?

A last step before moving on will be to clear some items from memory. For the rest of the lab you will be using data on horseshoe crabs, so clear these data by typing rm(list=ls())




<strong> </strong>

<ol>

 <li><strong> Instructions for Part 2</strong></li>

</ol>




This lab uses data on nesting horseshoe crabs. Begin by loading the horseshoe data, as shown in line 4 in the R script “<em>Lab07 crabs.R</em>”:

&gt; crab &lt;- read.table(“C:/crab.txt”)




The dataset contains data on 173 female horseshoe crabs, each of which has a male crab attached to her in her nest. The dependent variable is <em>Sa</em>, which is the number of “satellites” for each female crab; these are other male crabs living nearby.




Run lines 5-6 to name each variable and to remove the first column from the data frame, which simply identifies each observation. Explanatory variables that are thought to affect the number of male satellites include the female crab’s color (<em>C</em>), spine condition (<em>S</em>), carapace width (<em>W</em>) and weight (<em>Wt</em>). Let’s begin by examining whether the female crab’s width (<em>W</em>) affects the number of satellites (<em>Sa</em>).




Run lines 8-10 to attach the data and to view a scatterplot of satellites against carapace width, with a linear regression line.

&gt; plot(crab$W, crab$Sa, pch=19)

&gt; abline(lm(crab$Sa ~ crab$W))




You should observe that female crabs with a wider carapace tend to have more male satellites. However, there also are many female crabs with zero male satellites. Because the value of the dependent variable is truncated to non-negative values, this is almost certain to generate heteroscedasticity.




Run line 12 to estimate the linear regression of satellites on carapace width.

&gt; lmodel &lt;- lm(crab$Sa ~ crab$W); summary(lmodel)

Note that you can run line 13 to estimate the linear regression of satellites on carapace width using maximum likelihood (POLS 6482) instead of least squares (POLS 6481).




If you type the following code, you might be able to observe the predicted heteroscedasticity:

&gt; plot(crab$W, lmodel$residuals, pch=19)




Before we turn our attention fully to the Poisson regression model, we ought to spend a little time examining model fit and interpreting the impact of carapace width. The R<sup>2</sup> for the bivariate linear regression of <em>Sa</em> on <em>W</em> equals ________. Next, a one-unit increase in carapace width (<em>W</em>) increases the number of satellite mates (<em>Sa</em>) by ________; this effect (is/is not) _____ significant at the .05 level.




To carry out a comparison we’ll use later, calculate the predicted number of satellites for <em>W</em> = 25: ______ and for <em>W </em>= 26: ______ (i.e., multiply the slope coefficient times 25 or 26, and add the intercept).

No new packages are required to run the Poisson regression model, although some caution must go into specifying the model. Run line 15 of the R script to estimate the Poisson regression model

&gt; pmodel=glm(crab$Sa ~ 1 + crab$W, family=poisson(link=log))




Run line 16 to view the Poisson regression model estimated parameters. You will notice that there is <em>not</em> an R<sup>2</sup> measure presented; we will address this shortly. In the meantime, focus on the estimated coefficient for carapace width (<em>W</em>), which equals ________; is the effect statistically significant?

&gt; summary(pmodel)




Run line 18 to add the fitted values from the Poisson regression model to the data frame, which is necessary before exploring model fit. (If you wish, you may run line 19 to view the fitted values.) More importantly for our purposes, run line 20 to regress the actual values of <em>Sa</em> on the fitted values from the Poisson regression, and to display the R<sup>2</sup> from this regression, which equals ________. (This process should look familiar to you – it parallels how we assessed model fit for log-linear models!)

&gt; print = data.frame(crab, pred = pmodel$fitted)

&gt; fits = lm(Sa~pred, print); summary(fits)$r.squared




In lecture, we will address predicted values at some length. Run line 22 to display the linear predictors, which fill in the value of <em>W<sub>i</sub> </em> into the equation , for each observed value of <em>Wi</em>. Next, run line 23 to display the <u>exponentiated</u> linear predictors, i.e.,

&gt; pmodel$linear.predictors

&gt; exp(pmodel$linear.predictors)




Note that I did <u>not</u> type  in the previous paragraph and equation, because that would confuse what the Poisson model does. The exponentiated linear predictors, i.e., , equal <em>µ</em>(<em>x</em>). In order to generate actual predictions, we substitute <em>µ</em>(<em>x</em>) into the Poisson function:

, <em>k</em> = 0, 1, 2, …

where <em>µ</em>(<em>x</em>) is specific to a given value of <em>x</em>. So, if we set the value of <em>x</em>, we can generate a value of <em>µ</em>(<em>x</em>), and then we can generate a vector of predicted probabilities [ , , , …]. The idea of comparative statics is: changing the value of <em>x</em> changes the vector of predicted probabilities.




In order to clarify how this works, run line 25 to estimate the null model, which has only an intercept. This model treats every female horseshoe crab as if she is identical to all other female horseshoe crabs. If you run line 25, you should see that the null model estimates  = 1.0713. If you use your calculator, you should find that  <em>e</em><sup>1.0713</sup> = 2.919075 º <em>µ</em>. Because we are using the null model, this <em>µ</em> is <u>un</u>conditional on <em>x</em>. Finally, if you substitute <em>µ</em> = 2.9191 into the Poisson distribution, you should find the following:

= ________

= ________

= ________




You can continue to carry on for as many (or as few) values of <em>k</em> as you like.




With the process for generating predicted probabilities hopefully clearer, let us go back to the bivariate model which accounts for the female horseshoe crab’s carapace width (<em>W</em>). The mean and median values of <em>W</em> are 26.3 and 26.1, respectively. The first quartile of <em>W</em> is 24.9. So let us focus on the effect of increasing carapace width by 1 unit, from 25 (roughly the first quartile) to 26 (roughly the median).




Step 1. Calculate both  = <u>________</u> and  = <u>________</u>.

You can run lines 29-30  to check your calculations, but you should also write the equations in this space:













Step 2. Calculate <em>µ</em>(25) =  = ________ and <em>µ</em>(26) =  = ________.

You can run lines 31-32  to check your calculations, but you should also write the equations in this space:













Step 3. Calculate the probabilities of zero satellites if <em>W </em>equals 25 and if <em>W </em>equals 26:

=  = ______ vs.  =  = ______.

The calculations in Step 3. are made easier because  . Again, you can run lines 33-34  to check your calculations, but you should also write the equations in this space:













Step 4. Calculate the probabilities of one satellite if <em>W </em>equals 25 and if <em>W </em>equals 26:

=  = ______ vs.  =  = ______

You can run lines 35-36  to check your calculations, but you should also write the equations in the space below; also, this is the last time that the R script contains the code for these calculations…:













Step 5. Calculate the probabilities of two satellites if <em>W </em>equals 25 and if <em>W </em>equals 26:

=  = ______ vs.  =  = ______













Step 6 through Step ¥ would involve calculating as many predicted probabilities as you wish. In any case, if you adopt this form of interpretation, then changes in the value of your explanatory variable are changing the <em>probabilities</em> of different numbers of your dependent variable.




Another way of interpreting the coefficients resembles interpretation of the log-linear regression model. We actually use the same equation for interpreting   :  % Δ<em>y</em> = 100·[exp(  Δ<em>W</em>)–1].

In words, as <em>W</em> increases by 1 <strong>unit</strong>, <em>Sa</em> should increase by 100·[exp( )–1] <strong>percent</strong>. So, we need to exponentiate , subtract 1, and multiply by 100.




Recall that the estimated slope was  = 0.16405. Since  is positive,  will be greater than 1. Therefore, –1 will be positive, so that increases in <em>W </em>increase the expected number of satellites.

Calculate  = ______. As width increases by 1 unit, e.g., from 25 to 26, the exponentiated value should be greater by the same ratio. Run line 38 to prove that  = ______ = .

(Line 39 uses the 2nd and 6th observations, which have <em>W</em> = 26 and <em>W</em> = 25, respectively, to show this.)




If you subtract 1 from  = , you get _______. This tells you that the mean value if <em>W</em> is 26 is predicted to be ______% higher than the mean value if <em>W </em>is 25.




For a comparison, go back to the linear regression model and calculate  = ________ and  = <u>________</u> based on that model. The ratio of the latter number to the former number equals _______. If you subtract 1, this tells us that the predicted number of satellites for one additional unit of carapace width is ______ greater (which is equal to , of course).




Let us conclude our investigation of the coefficient for carapace width by looking at the confidence intervals with ordinary standard errors and with robust standard errors. Type the following code or run line 41 to examine the confidence interval for . Then, run line 42 to examine the confidence interval for  – 1.  As long as the lower and upper bounds for the confidence intervals both exceed 0, then carapace width has a significant and positive effect on satellites.




Note: You may need to uncomment and run line 43 if you do not already have the package <em>sandwich </em>installed.




Either run line 44 to load the <em>sandwich</em> package, or run line 51 to load the <em>car </em>package, both of which we have used previously. Next, either run line 45 to use the <em>sandwich </em>package to create a new variance-covariance matrix of the estimators, or run line 52 to do the same with the <em>car</em> package. Both line 45 and line 52 name this new matrix <em>robvar</em>.




Run line 46 to create the standard errors of the intercept and the slope, calculated as the square roots of the diagonal elements of the <em>robvar</em> matrix. Run lines 47-48 to bind together the coefficient, the new standard errors, the <em>p</em> value, and the confidence intervals. Run line 49 to display these in a way that should resemble usual regression output (at least in Stata…). Go back to earlier results in the lab, and compare the standard errors to what was displayed after line 15, and compare the confidence interval to what was displayed after line 40. Because the robust standard error is about 50% larger, the confidence interval is larger, but it still excludes zero by a wide margin.




To clear the <strong>Environment</strong>, type rm(list=ls()) or click on the broom icon.

To clear the <strong>Console</strong> window, type Ctrl-<em>l</em>





