# Foundations of inference {#inference-foundations}

\BeginKnitrBlock{uptohere}<div class="uptohere">The content in this chapter is currently just placeholder. We will remove this banner once the chapter content has been updated and ready for review.</div>\EndKnitrBlock{uptohere}

\BeginKnitrBlock{chapterintro}<div class="chapterintro">Statistical inference is primarily concerned with understanding and quantifying the uncertainty of parameter estimates. 
While the equations and details change depending on the setting, the foundations for inference are the same throughout all of statistics. 

We start with two case studies designed to motivate the process of making decisions about research claims. We formalize the process through the introducuction of the **hypothesis testing framework**\index{hypothesis test}, which allows us to formally evaluate claims about the population.

Finally we expand on the familiar idea of using a sample proportion to estimate a population proportion.
That is, we create what is called a **confidence interval**\index{confidence interval}, which is a range of plausible values where we may find the true population value.</div>\EndKnitrBlock{chapterintro}





Throughout the book so far, you have worked with data in a variety of contexts.
You have learned how to summarize and visualize the data as well as how to model multiple variables at the same time.
Sometimes the dataset at hand represents the entire research question.
But more often than not, the data have been collected to answer a research question about a larger group of which the data are a (hopefully) representative subset.

You may agree that there is almost always variability in data (one dataset will not be identical to a second dataset even if they are both collected from the same population using the same methods).
However, quantifying the variability in the data is neither obvious nor easy to do (**how** different is one dataset from another?). 



\BeginKnitrBlock{example}<div class="example">Suppose your professor splits the students in class into two groups: students on the left and students on the right. If $\hat{p}_{_L}$ and $\hat{p}_{_R}$ represent the proportion of students who own an Apple product on the left and right, respectively, would you be surprised if $\hat{p}_{_L}$ did not *exactly* equal $\hat{p}_{_R}$?

---

While the proportions would probably be close to each other, it would be unusual for them to be exactly the same. We would probably observe a small difference due to *chance*.</div>\EndKnitrBlock{example}



\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">If we don't think the side of the room a person sits on in class is related to whether the person owns an Apple product, what assumption are we making about the relationship between these two variables?
(Reminder: for these Guided Practice questions, you can check your answer in the footnote.)^[We would be assuming that these two variables are **independent**\index{independent}.]</div>\EndKnitrBlock{guidedpractice}



Studying randomness of this form is a key focus of statistics. 
Throughout this chapter, and those that follow, we provide three different approaches for quantifying the variability inherent in data: randomization, bootstrapping, and mathematical models.
Using the methods provided in this chapter, we will be able to draw conclusions beyond the dataset at hand to research questions about larger populations.

## Randomization test {#inf-rand}


The first type of variability we will explore comes from experiments where the explanatory variable (or treatment) is randomly assigned to the observational units.
As you learned in Chapter \@ref(intro-to-data), a **randomized experiment** is done to assess whether or not one variable (the **explanatory** variable) causes changes in a second variable (the **response** variable). 
Every dataset has some variability in it, so to decide whether the variability in the data is due to (1) the causal mechanism (the randomized explanatory variable in the experiment) or instead (2) natural variability inherent to the data, we set up a sham randomized experiment as a comparison. 
That is, we assume that each observational unit would have gotten the exact same response value regardless of the treatment level. 
By reassigning the treatments many many times, we can compare the actual experiment to the sham experiment. If the actual experiment has more extreme results than any of the sham experiments, we are led to believe that it is the explanatory variable which is causing the result and not inherent data variability. 
Using a few different case studies, let's look more carefully at this idea of a **randomization test**\index{randomization}.





### Gender discrimination case study {#caseStudyGenderDiscrimination}


\index{data!discrimination|(}

We consider a study investigating gender discrimination in the 1970s, which is set in the context of personnel decisions within a bank.^[Rosen B and Jerdee T. 1974. "Influence of sex role stereotypes on personnel decisions." Journal of Applied Psychology 59(1):9-14.] The research question we hope to answer is, "Are females discriminated against in promotion decisions made by male managers?"

#### Observed data {-}

The participants in this study were 48 male bank supervisors attending a management institute at the University of North Carolina in 1972. 
They were asked to assume the role of the personnel director of a bank and were given a personnel file to judge whether the person should be promoted to a branch manager position. 
The files given to the participants were identical, except that half of them indicated the candidate was male and the other half indicated the candidate was female. 
These files were randomly assigned to the subjects.

\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">Is this an observational study or an experiment? How does the type of study impact what can be inferred from the results?^[The study is an experiment, as subjects were randomly assigned a "male" file or a "female" file (remember, all the files were actually identical in content). Since this is an experiment, the results can be used to evaluate a causal relationship between gender of a candidate and the promotion decision.]</div>\EndKnitrBlock{guidedpractice}

For each supervisor we recorded the gender associated with the assigned file and the promotion decision. 
Using the results of the study summarized in Table \@ref(tab:discriminationResults), we would like to evaluate if females are unfairly discriminated against in promotion decisions. 
In this study, a smaller proportion of females are promoted than males (0.583 versus 0.875), but it is unclear whether the difference provides *convincing evidence* that females are unfairly discriminated against.


<table class="table" style="margin-left: auto; margin-right: auto;">
<caption>(\#tab:discriminationResults)Summary results for the gender discrimination study.</caption>
 <thead>
<tr>
<th style="border-bottom:hidden" colspan="1"></th>
<th style="border-bottom:hidden" colspan="1"></th>
<th style="border-bottom:hidden; padding-bottom:0; padding-left:3px;padding-right:3px;text-align: center; " colspan="2"><div style="border-bottom: 1px solid #ddd; padding-bottom: 5px; ">`decision`</div></th>
<th style="border-bottom:hidden" colspan="1"></th>
</tr>
  <tr>
   <th style="text-align:left;">  </th>
   <th style="text-align:left;">  </th>
   <th style="text-align:left;"> promoted </th>
   <th style="text-align:left;"> not promoted </th>
   <th style="text-align:left;"> Total </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;">  </td>
   <td style="text-align:left;"> male </td>
   <td style="text-align:left;"> 21 </td>
   <td style="text-align:left;"> 3 </td>
   <td style="text-align:left;"> 24 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> `gender` </td>
   <td style="text-align:left;"> female </td>
   <td style="text-align:left;"> 14 </td>
   <td style="text-align:left;"> 10 </td>
   <td style="text-align:left;"> 24 </td>
  </tr>
  <tr>
   <td style="text-align:left;">  </td>
   <td style="text-align:left;"> Total </td>
   <td style="text-align:left;"> 35 </td>
   <td style="text-align:left;"> 13 </td>
   <td style="text-align:left;"> 48 </td>
  </tr>
</tbody>
</table>

The data are visualized in Figure \@ref(fig:genderrand1).  Note that the promoted decision is colored in red (promoted) and white(not promoted).  Additionally, the observations are broken up into the male and female groups.


<div class="figure" style="text-align: center">
<img src="05/figures/genderrand1b.png" alt="The gender descriminiation study can be thought of as 48 red and black cards." width="50%" />
<p class="caption">(\#fig:genderrand1)The gender descriminiation study can be thought of as 48 red and black cards.</p>
</div>

\BeginKnitrBlock{example}<div class="example">Statisticians are sometimes called upon to evaluate the strength of evidence. 
When looking at the rates of promotion for males and females in this study, why might we be tempted to immediately conclude that females are being discriminated against?
 
---
 
The large difference in promotion rates (58.3% for females versus 87.5% for males) suggest there might be discrimination against women in promotion decisions. 
However, we cannot yet be sure if the observed difference represents discrimination or is just from random chance. 
Generally there is a little bit of fluctuation in sample data, and we wouldn't expect the sample proportions to be *exactly* equal, even if the truth was that the promotion decisions were independent of gender.</div>\EndKnitrBlock{example}

The previous example is a reminder that the observed outcomes in the sample may not perfectly reflect the true relationships between variables in the underlying population.
Table \@ref(tab:discriminationResults) shows there were 7 fewer promotions in the female group than in the male group, a difference in promotion rates of 29.2% $\left( \frac{21}{24} - \frac{14}{24} = 0.292 \right)$. 
This observed difference is what we call a **point estimate**\index{point estimate} of the true effect. 
The point estimate of the difference is large, but the sample size for the study is small, making it unclear if this observed difference represents discrimination or whether it is simply due to chance. 
We label these two competing claims, $H_0$ and $H_A$:



* $H_0$: **Null hypothesis**\index{null hypothesis}. The variables `gender` and `decision` are independent. They have no relationship, and the observed difference between the proportion of males and females who were promoted, 29.2%, was due to chance.

* $H_A$: **Alternative hypothesis**\index{alternative hypothesis}. The variables `gender` and `decision` are *not* independent. The difference in promotion rates of 29.2% was not due to chance, and equally qualified females are less likely to be promoted than males.



\BeginKnitrBlock{onebox}<div class="onebox">**Hypothesis testing**

These hypotheses are part of what is called a **hypothesis test**\index{hypothesis test}. 
A hypothesis test is a statistical technique used to evaluate competing claims using data. 
Often times, the null hypothesis takes a stance of *no difference* or *no effect*. 

If the null hypothesis and the data notably disagree, then we will reject the null hypothesis in favor of the alternative hypothesis. 

Don't worry if you aren't a master of hypothesis testing at the end of this section. 
We'll discuss these ideas and details many times in this chapter and those that follow.</div>\EndKnitrBlock{onebox}



What would it mean if the null hypothesis, which says the variables `gender` and `decision` are unrelated, is true? 
It would mean each banker would decide whether to promote the candidate without regard to the gender indicated on the file. 
That is, the difference in the promotion percentages would be due to the way the files were randomly divided to the bankers, and the randomization just happened to give rise to a relatively large difference of 29.2%.

Consider the alternative hypothesis: bankers were influenced by which gender was listed on the personnel file. 
If this was true, and especially if this influence was substantial, we would expect to see some difference in the promotion rates of male and female candidates. 
If this gender bias was against females, we would expect a smaller fraction of promotion recommendations for female personnel files relative to the male files.

We will choose between these two competing claims by assessing if the data conflict so much with $H_0$ that the null hypothesis cannot be deemed reasonable. 
If this is the case, and the data support $H_A$, then we will reject the notion of independence and conclude that these data provide strong evidence of discrimination.

#### Variability of the statistic {-}

Table \@ref(tab:discriminationResults) shows that 35 bank supervisors recommended promotion and 13 did not. 
Now, suppose the bankers' decisions were independent of gender. 
Then, if we conducted the experiment again with a different random assignment of gender to the files, differences in promotion rates would be based only on random fluctuation. 
We can actually perform this **randomization**, which simulates what would have happened if the bankers' decisions had been independent of gender but we had distributed the file genders differently.^[The test procedure we employ in this section is sometimes referred to as a **permutation test**\index{permutation test}.]



In this **simulation**\index{simulation}, we thoroughly shuffle 48 personnel files, 35 labeled `promoted` and 13 labeled `not promoted`, and we deal these files into two stacks. 
Note that by keeping 35 promoted and 13 not promoted, we are assuming that 35 of the bank managers would have promoted the individual whose content is contained in the file (**independent** of gender).
We will deal 24 files into the first stack, which will represent the 24 "female" files.
The second stack will also have 24 files, and it will represent the 24 "male" files.
Figure \@ref(fig:genderrand3) highlights both the shuffle and the reallocation to the sham gender groups.


<div class="figure" style="text-align: center">
<img src="05/figures/genderrand3b.png" alt="The gender descriminiation data is shuffled and reallocated to the gender groups." width="80%" />
<p class="caption">(\#fig:genderrand3)The gender descriminiation data is shuffled and reallocated to the gender groups.</p>
</div>


Then, as we did with the original data, we tabulate the results and determine the fraction of `male` and `female` who were promoted.



Since the randomization of files in this simulation is independent of the promotion decisions, any difference in the two promotion rates is entirely due to chance. 
Table \@ref(tab:discriminationRand1) show the results of one such simulation.


<table class="table" style="margin-left: auto; margin-right: auto;">
<caption>(\#tab:discriminationRand1)Simulation results, where the difference in promotion rates between `male` and `female` is purely due to chance.</caption>
 <thead>
<tr>
<th style="border-bottom:hidden" colspan="1"></th>
<th style="border-bottom:hidden" colspan="1"></th>
<th style="border-bottom:hidden; padding-bottom:0; padding-left:3px;padding-right:3px;text-align: center; " colspan="2"><div style="border-bottom: 1px solid #ddd; padding-bottom: 5px; ">`decision`</div></th>
<th style="border-bottom:hidden" colspan="1"></th>
</tr>
  <tr>
   <th style="text-align:left;">  </th>
   <th style="text-align:left;">  </th>
   <th style="text-align:left;"> promoted </th>
   <th style="text-align:left;"> not promoted </th>
   <th style="text-align:left;"> Total </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;">  </td>
   <td style="text-align:left;"> male </td>
   <td style="text-align:left;"> 18 </td>
   <td style="text-align:left;"> 6 </td>
   <td style="text-align:left;"> 24 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> `gender` </td>
   <td style="text-align:left;"> female </td>
   <td style="text-align:left;"> 17 </td>
   <td style="text-align:left;"> 7 </td>
   <td style="text-align:left;"> 24 </td>
  </tr>
  <tr>
   <td style="text-align:left;">  </td>
   <td style="text-align:left;"> Total </td>
   <td style="text-align:left;"> 35 </td>
   <td style="text-align:left;"> 13 </td>
   <td style="text-align:left;"> 48 </td>
  </tr>
</tbody>
</table>



\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">What is the difference in promotion rates between the two simulated groups in Table \@ref(tab:discriminationRand1) ? 
How does this compare to the observed difference 29.2% from the actual study?^[$18/24 - 17/24=0.042$ or about 4.2% in favor of the men. 
This difference due to chance is much smaller than the difference observed in the actual groups.]</div>\EndKnitrBlock{guidedpractice}



Figure \@ref(fig:genderrand4) shows that the difference in promotion rates is much larger in the original data than it is in the simulated groups (0.292 >>>  0.042).
The quantity of interest throughout this case study has been the difference in promotion rates.
We call the summary value the **statistic** of interest (or often the **test statistic**).
When we encounter different data structures, the statistic is likely to change (e.g., we might calculate an average instead of a proportion), but we will always want to understand how the statistic varies from sample to sample.



<div class="figure" style="text-align: center">
<img src="05/figures/genderrand4c.png" alt="We summarize the randomized data to produce one estiamte of the difference in proportions given no gender discrimination." width="100%" />
<p class="caption">(\#fig:genderrand4)We summarize the randomized data to produce one estiamte of the difference in proportions given no gender discrimination.</p>
</div>


#### Observed statistic vs. null statistics {-}

We computed one possible difference under the null hypothesis in Guided Practice, which represents one difference due to chance. 
While in this first simulation, we physically dealt out files, it is much more efficient to perform this simulation using a computer. 
Repeating the simulation on a computer, we get another difference due to chance: -0.042. 
And another: 0.208. 
And so on until we repeat the simulation enough times that we have a good idea of what represents the *distribution of differences from chance alone*. 
Figure \@ref(fig:discRandDotPlot) shows a plot of the differences found from 100 simulations, where each dot represents a simulated difference between the proportions of male and female files recommended for promotion.


<div class="figure" style="text-align: center">
<img src="05-inference-foundations_files/figure-html/discRandDotPlot-1.png" alt="A stacked dot plot of differences from 100 simulations produced under the null hypothesis, $H_0$, where `gender_simulated` and `decision` are independent. Two of the 100 simulations had a difference of at least 29.2%, the difference observed in the study, and are shown as solid red dots." width="70%" />
<p class="caption">(\#fig:discRandDotPlot)A stacked dot plot of differences from 100 simulations produced under the null hypothesis, $H_0$, where `gender_simulated` and `decision` are independent. Two of the 100 simulations had a difference of at least 29.2%, the difference observed in the study, and are shown as solid red dots.</p>
</div>

Note that the distribution of these simulated differences in proportions is centered around 0. 
Because we simulated differences in a way that made no distinction between men and women, this makes sense: we should expect differences from chance alone to fall around zero with some random fluctuation for each simulation.

\BeginKnitrBlock{example}<div class="example">How often would you observe a difference of at least 29.2% (0.292) according to Figure \@ref(fig:discRandDotPlot)? 
Often, sometimes, rarely, or never?
 
---
 
It appears that a difference of at least 29.2% due to chance alone would only happen about 2% of the time according to Figure \@ref(fig:discRandDotPlot). 
Such a low probability indicates that observing such a large difference from chance is rare.</div>\EndKnitrBlock{example}

The difference of 29.2% is a rare event if there really is no impact from listing gender in the candidates' files, which provides us with two possible interpretations of the study results:


* $H_0$: **Null hypothesis**. Gender has no effect on promotion decision, and we observed a difference that is so large that it would only happen rarely.

* $H_A$: **Alternative hypothesis**. Gender has an effect on promotion decision, and what we observed was actually due to equally qualified women being discriminated against in promotion decisions, which explains the large difference of 29.2%.

When we conduct formal studies, we reject a null position (the idea that the data are a result of chance only) if the data strongly conflict with that null position.^[This reasoning does not generally extend to anecdotal observations. 
Each of us observes incredibly rare events every day, events we could not possibly hope to predict. 
However, in the non-rigorous setting of anecdotal evidence, almost anything may appear to be a rare event, so the idea of looking for rare events in day-to-day activities is treacherous. 
For example, we might look at the lottery: there was only a 1 in 176 million chance that the Mega Millions numbers for the largest jackpot in history (March 30, 2012) would be (2, 4, 23, 38, 46) with a Mega ball of (23), but nonetheless those numbers came up! 
However, no matter what numbers had turned up, they would have had the same incredibly rare odds. 
That is, *any set of numbers we could have observed would ultimately be incredibly rare*. 
This type of situation is typical of our daily lives: each possible event in itself seems incredibly rare, but if we consider every alternative, those outcomes are also incredibly rare. 
We should be cautious not to misinterpret such anecdotal evidence.]
In our analysis, we determined that there was only a $\approx$ 2% probability of obtaining a sample where $\geq$ 29.2% more males than females get promoted by chance alone, so we conclude that the data provide strong evidence of gender discrimination against women by the supervisors. 
In this case, we reject the null hypothesis in favor of the alternative.

\index{data!discrimination|)}

Statistical inference is the practice of making decisions and conclusions from data in the context of uncertainty. 
Errors do occur, just like rare events, and the data set at hand might lead us to the wrong conclusion. 
While a given data set may not always lead us to a correct conclusion, statistical inference gives us tools to control and evaluate how often these errors occur. 
Before getting into the nuances of hypothesis testing, let's work through another case study.


### Opportunity cost case study {#caseStudyOpportunityCost}

How rational and consistent is the behavior of the typical American college student? 
In this section, we'll explore whether college student consumers always consider the following: money not spent now can be spent later.

In particular, we are interested in whether reminding students about this well-known fact about money causes them to be a little thriftier. 
A skeptic might think that such a reminder would have no impact. 
We can summarize the two different perspectives using the null and alternative hypothesis framework.

* $H_0$: **Null hypothesis**. Reminding students that they can save money for later purchases will not have any impact on students' spending decisions.

* $H_A$: **Alternative hypothesis**. Reminding students that they can save money for later purchases will reduce the chance they will continue with a purchase.

In this section, we'll explore an experiment conducted by researchers that investigates this very question for students at a university in the southwestern United States.^[Frederick S, Novemsky N, Wang J, Dhar R, Nowlis S. 2009. Opportunity Cost Neglect. Journal of Consumer Research 36: 553-561.]

#### Observed data {-}

<!--Shane Frederick of Yale School of Management and his collaborators conducted an experiment exploring the rational behavior of consumers. 

% Suppose when a person is about to spend money, we simply reminded them that they could spend the money on something else. Would it have any impact on the likelihood that they would continue with the purchase?
%What would you do in this situation? Please circle one of the options below.

-->

One-hundred and fifty students were recruited for the study, and each was given the following statement:

> Imagine that you have been saving some extra money on the side to make some purchases, and on your most recent visit to the video store you come across a special sale on a new video. This video is one with your favorite actor or actress, and your favorite type of movie (such as a comedy, drama, thriller, etc.). This particular video that you are considering is one you have been thinking about buying for a long time. It is available for a special sale price of $14.99.

> What would you do in this situation? Please circle one of the options below.

Half of the 150 students were randomized into a control group and were given the following two options:

> (A) Buy this entertaining video.

> (B) Not buy this entertaining video.


The remaining 75 students were placed in the treatment group, and they saw a slightly modified option (B):

> (A) Buy this entertaining video.

> (B) Not buy this entertaining video. Keep the $14.99 for other purchases.

Would the extra statement reminding students of an obvious fact impact the purchasing decision? 
Table \@ref(tab:OpportunityCostTable) summarizes the study results.


<table class="table" style="margin-left: auto; margin-right: auto;">
<caption>(\#tab:OpportunityCostTable)Summary of student choices in the opportunity cost study.</caption>
 <thead>
<tr>
<th style="border-bottom:hidden" colspan="1"></th>
<th style="border-bottom:hidden; padding-bottom:0; padding-left:3px;padding-right:3px;text-align: center; " colspan="2"><div style="border-bottom: 1px solid #ddd; padding-bottom: 5px; ">`decision`</div></th>
<th style="border-bottom:hidden" colspan="1"></th>
</tr>
  <tr>
   <th style="text-align:left;">  </th>
   <th style="text-align:left;"> buy DVD </th>
   <th style="text-align:left;"> not buy DVD </th>
   <th style="text-align:left;"> Total </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> control group </td>
   <td style="text-align:left;"> 56 </td>
   <td style="text-align:left;"> 19 </td>
   <td style="text-align:left;"> 75 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> treatment group </td>
   <td style="text-align:left;"> 41 </td>
   <td style="text-align:left;"> 34 </td>
   <td style="text-align:left;"> 75 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Total </td>
   <td style="text-align:left;"> 97 </td>
   <td style="text-align:left;"> 53 </td>
   <td style="text-align:left;"> 150 </td>
  </tr>
</tbody>
</table>

<!--
%150 participants were asked whether they would buy a DVD under a particular circumstance. Participants in the control group were given two options, and participants in the treatment group were given the same options, except in the *not buy* option they were reminded that not spending the money meant the money could be used for a later purchase. This table summarizes the results from the study.}
-->

It might be a little easier to review the results using row proportions, specifically considering the proportion of participants in each group who said they would buy or not buy the DVD. 
These summaries are given in Table \@ref(tab:OpportunityCostTableRowProp).


<table class="table" style="margin-left: auto; margin-right: auto;">
<caption>(\#tab:OpportunityCostTableRowProp)The data above are now summarized using row proportions. Row proportions are particularly useful here since we can view the proportion of *buy* and *not buy* decisions in each group.</caption>
 <thead>
<tr>
<th style="border-bottom:hidden" colspan="1"></th>
<th style="border-bottom:hidden; padding-bottom:0; padding-left:3px;padding-right:3px;text-align: center; " colspan="2"><div style="border-bottom: 1px solid #ddd; padding-bottom: 5px; ">`decision`</div></th>
<th style="border-bottom:hidden" colspan="1"></th>
</tr>
  <tr>
   <th style="text-align:left;">  </th>
   <th style="text-align:left;"> buy DVD </th>
   <th style="text-align:left;"> not buy DVD </th>
   <th style="text-align:left;"> Total </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> control group </td>
   <td style="text-align:left;"> 0.747 </td>
   <td style="text-align:left;"> 0.253 </td>
   <td style="text-align:left;"> 1.00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> treatment group </td>
   <td style="text-align:left;"> 0.547 </td>
   <td style="text-align:left;"> 0.453 </td>
   <td style="text-align:left;"> 1.00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Total </td>
   <td style="text-align:left;"> 0.647 </td>
   <td style="text-align:left;"> 0.353 </td>
   <td style="text-align:left;"> 1.00 </td>
  </tr>
</tbody>
</table>

<!--
"The data from Table \@ref(tab:OpportunityCostTable) summarized using row proportions. Row proportions are particularly useful here since we can view the proportion of *buy* and *not buy* decisions in each group."
-->



We will define a **success**\index{success} in this study as a student who chooses not to buy the DVD.^[Success is often defined in a study as the outcome of interest, and a "success" may or may not actually be a positive outcome. For example, researchers working on a study on HIV prevalence might define a "success" in the statistical sense as a patient who is HIV+. A more complete discussion of the term **success** will be given in Chapter \@ref(inference-cat).] 
Then, the value of interest is the change in DVD purchase rates that results by reminding students that not spending money now means they can spend the money later.




<!--
%A first look at the data suggests that reminding students that not spending money means they can spend the money later has an impact. 
-->

We can construct a point estimate for this difference as
\begin{align*}
\hat{p}_{trmt} - \hat{p}_{ctrl}
 = \frac{34}{75} - \frac{19}{75}
 = 0.453 - 0.253
 = 0.200
\end{align*}
The proportion of students who chose not to buy the DVD was 20% higher in the treatment group than the control group.
However, is this result **statistically significant**\index{statistically significant}? In other words, is a 20% difference between the two groups so prominent that it is unlikely to have occurred from chance alone?



#### Variability of the statistic {-}

The primary goal in this data analysis is to understand what sort of differences we might see if the null hypothesis were true, i.e., the treatment had no effect on students. 
For this, we'll use the same procedure we applied in Section \@ref(caseStudyGenderDiscrimination): randomization.

Let's think about the data in the context of the hypotheses. 
If the null hypothesis ($H_0$) was true and the treatment had no impact on student decisions, then the observed difference between the two groups of 20% could be attributed entirely to chance. 
If, on the other hand, the alternative hypothesis ($H_A$) is true, then the difference indicates that reminding students about saving for later purchases actually impacts their buying decisions.

#### Observed statistic vs. null statistics {-}

Just like with the gender discrimination study, we can perform a statistical analysis. 
Using the same randomization technique from the last section, let's see what happens when we simulate the experiment under the scenario where there is no effect from the treatment.

While we would in reality do this simulation on a computer, it might be useful to think about how we would go about carrying out the simulation without a computer. 
We start with 150 index cards and label each card to indicate the distribution of our response variable: `decision`. 
That is, 53 cards will be labeled "not buy DVD" to represent the 53 students who opted not to buy, and 97 will be labeled "buy DVD" for the other 97 students. 
Then we shuffle these cards thoroughly and divide them into two stacks of size 75, representing the simulated treatment and control groups. 
Any observed difference between the proportions of "not buy DVD" cards (what we earlier defined as *success*) can be attributed entirely to chance.

\BeginKnitrBlock{example}<div class="example">If we are randomly assigning the cards into the simulated treatment and control groups, how many "not buy DVD" cards would we expect to end up with in each simulated group? 
What would be the expected difference between the proportions of "not buy DVD" cards in each group?

---

Since the simulated groups are of equal size, we would expect $53 / 2 = 26.5$, i.e., 26 or 27, "not buy DVD" cards in each simulated group, yielding a simulated point estimate of 0% . However, due to random fluctuations, we might actually observe a number a little above or below 26 and 27.</div>\EndKnitrBlock{example}

<!--
%We'll take the students and randomize them into two new groups, simulated-control and simulated-treatment groups, and then we'll look at the difference in the two groups. 
-->

The results of a single randomization from chance alone is shown in Table \@ref(tab:OpportunityCostTableSimulated). 
From this table, we can compute a difference that occurred from chance alone:
\begin{align*}
\hat{p}_{trmt, simulated} - \hat{p}_{ctrl, simulated}
 = \frac{24}{75} - \frac{29}{75}
 = 0.32 - 0.387
 = - 0.067
\end{align*}
<!--
%This difference of -6.7% is entirely due to chance.
-->


<table class="table" style="margin-left: auto; margin-right: auto;">
<caption>(\#tab:OpportunityCostTableSimulated)Summary of student choices against their simulated groups. The group assignment had no connection to the student decisions, so any difference between the two groups is due to chance.</caption>
 <thead>
<tr>
<th style="border-bottom:hidden" colspan="1"></th>
<th style="border-bottom:hidden; padding-bottom:0; padding-left:3px;padding-right:3px;text-align: center; " colspan="2"><div style="border-bottom: 1px solid #ddd; padding-bottom: 5px; ">`decision`</div></th>
<th style="border-bottom:hidden" colspan="1"></th>
</tr>
  <tr>
   <th style="text-align:left;">  </th>
   <th style="text-align:left;"> buy DVD </th>
   <th style="text-align:left;"> not buy DVD </th>
   <th style="text-align:left;"> Total </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> control group </td>
   <td style="text-align:left;"> 46 </td>
   <td style="text-align:left;"> 29 </td>
   <td style="text-align:left;"> 75 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> treatment group </td>
   <td style="text-align:left;"> 51 </td>
   <td style="text-align:left;"> 24 </td>
   <td style="text-align:left;"> 75 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Total </td>
   <td style="text-align:left;"> 97 </td>
   <td style="text-align:left;"> 53 </td>
   <td style="text-align:left;"> 150 </td>
  </tr>
</tbody>
</table>


Just one simulation will not be enough to get a sense of what sorts of differences would happen from chance alone. 
We'll simulate another set of simulated groups and compute the new difference: 0.013. 
And again: 0.067. 
And again: -0.173. 
We'll do this 1,000 times. The results are summarized in a dot plot in Figure \@ref(fig:OpportunityCostDiffsDotPlot), where each point represents a simulation. 
Since there are so many points, it is more convenient to summarize the results in a histogram such as the one in Figure \@ref(fig:OpportunityCostDiffs), where the height of each histogram bar represents the fraction of observations in that group.


<div class="figure" style="text-align: center">
<img src="05-inference-foundations_files/figure-html/OpportunityCostDiffsDotPlot-1.png" alt="A stacked dot plot of 1,000 chance differences produced under the null hypothesis, $H_0$. Six of the 1,000 simulations had a difference of at least 20% , which was the difference observed in the study." width="70%" />
<p class="caption">(\#fig:OpportunityCostDiffsDotPlot)A stacked dot plot of 1,000 chance differences produced under the null hypothesis, $H_0$. Six of the 1,000 simulations had a difference of at least 20% , which was the difference observed in the study.</p>
</div>


<div class="figure" style="text-align: center">
<img src="05-inference-foundations_files/figure-html/OpportunityCostDiffs-1.png" alt="A histogram of 1,000 chance differences produced under the null hypothesis, $H_0$. Histograms like this one are a more convenient representation of data or results when there are a large number of observations." width="70%" />
<p class="caption">(\#fig:OpportunityCostDiffs)A histogram of 1,000 chance differences produced under the null hypothesis, $H_0$. Histograms like this one are a more convenient representation of data or results when there are a large number of observations.</p>
</div>


If there was no treatment effect, then we'd only observe a difference of at least +20% about 0.6% of the time, or about 1-in-150 times. 
That is really rare! 
Instead, we will conclude the data provide strong evidence there is a treatment effect: reminding students before a purchase that they could instead spend the money later on something else lowers the chance that they will continue with the purchase. 
Notice that we are able to make a causal statement for this study since the study is an experiment.



### Hypothesis testing {#HypothesisTesting}


In the last two sections, we utilized a **hypothesis test**\index{hypothesis test}, which is a formal technique for evaluating two competing possibilities. 
In each scenario, we described a **null hypothesis**\index{null hypothesis}, which represented either a skeptical perspective or a perspective of no difference. 
We also laid out an **alternative hypothesis**\index{alternative hypothesis}, which represented a new perspective such as the possibility that there has been a change or that there is a treatment effect in an experiment.  The alternative hypothesis is usually the reason the scientists set out to do the research in the first place.





\BeginKnitrBlock{onebox}<div class="onebox">**Null and alternative hypotheses.**

The **null hypothesis ($H_0$)** often represents either a skeptical perspective or a claim to be tested. The **alternative hypothesis ($H_A$)** represents an alternative claim under consideration and is often represented by a range of possible values for the value of interest. </div>\EndKnitrBlock{onebox}

The hypothesis testing framework is a very general tool, and we often use it without a second thought. 
If a person makes a somewhat unbelievable claim, we are initially skeptical. 
However, if there is sufficient evidence that supports the claim, we set aside our skepticism. 
The hallmarks of hypothesis testing are also found in the US court system. 

#### The US court system {-}


\BeginKnitrBlock{example}<div class="example">A US court considers two possible claims about a defendant: they are either innocent or guilty. If we set these claims up in a hypothesis framework, which would be the null hypothesis and which the alternative?

---
 
The jury considers whether the evidence is so convincing (strong) that there is no reasonable doubt regarding the person's guilt. 
That is, the skeptical perspective (null hypothesis) is that the person is innocent until evidence is presented that convinces the jury that the person is guilty (alternative hypothesis).</div>\EndKnitrBlock{example}


Jurors examine the evidence to see whether it convincingly shows a defendant is guilty. 
Notice that if a jury finds a defendant *not guilty*, this does not necessarily mean the jury is confident in the person's innocence. 
They are simply not convinced of the alternative that the person is guilty.

This is also the case with hypothesis testing: *even if we fail to reject the null hypothesis, we typically do not accept the null hypothesis as truth*. 
Failing to find strong evidence for the alternative hypothesis is not equivalent to providing evidence that the null hypothesis is true.


#### p-value and statistical significance {-}

In Section \@ref(caseStudyGenderDiscrimination) we encountered a study from the 1970's that explored whether there was strong evidence that women were less likely to be promoted than men. 
The research question -- are females discriminated against in promotion decisions? -- was framed in the context of hypotheses:


* $H_0$: Gender has no effect on promotion decisions.

* $H_A$: Women are discriminated against in promotion decisions.


The null hypothesis ($H_0$) was a perspective of no difference. 
The data on gender discrimination provided a point estimate of a 29.2% difference in recommended promotion rates between men and women. 
We determined that such a difference from chance alone would be rare: it would only happen about 2 in 100 times. 
When results like these are inconsistent with $H_0$, we reject $H_0$ in favor of $H_A$. 
Here, we concluded there was discrimination against women.

The 2-in-100 chance is what we call a **p-value**, which is a probability quantifying the strength of the evidence against the null hypothesis and in favor of the alternative. 

<!--%Formally the p-value is a conditional probability, which is basically\footnote{Want to learn more probability? Check out Appendix \ref{probability}.}
-->

\BeginKnitrBlock{onebox}<div class="onebox">**p-value.**

The **p-value**\index{hypothesis testing!p-value|textbf} is the probability of observing data at least as favorable to the alternative hypothesis as our current dataset, if the null hypothesis were true. 
We typically use a summary statistic of the data, such as a difference in proportions, to help compute the p-value and evaluate the hypotheses. 
This summary value that is used to compute the p-value is often called the **test statistic**\index{test statistic}.</div>\EndKnitrBlock{onebox}




\BeginKnitrBlock{example}<div class="example">In the gender discrimination study, the difference in discrimination rates was our test statistic. What was the test statistic in the opportunity cost study covered in Section \@ref(caseStudyOpportunityCost)?
 
---
 
The test statistic in the opportunity cost study was the difference in the proportion of students who decided against the DVD purchase in the treatment and control groups. 
In each of these examples, the **point estimate** of the difference in proportions was used as the test statistic.</div>\EndKnitrBlock{example}

When the p-value is small, i.e., less than a previously set threshold, we say the results are **statistically significant**. 
This means the data provide such strong evidence against $H_0$ that we reject the null hypothesis in favor of the alternative hypothesis. 
The threshold, called the **significance level**\index{hypothesis testing!significance level}\index{significance level} and often represented by $\alpha$ (the Greek letter *alpha*), is typically set to $\alpha = 0.05$, but can vary depending on the field or the application. 
Using a significance level of $\alpha = 0.05$ in the discrimination study, we can say that the data provided statistically significant evidence against the null hypothesis.

\BeginKnitrBlock{onebox}<div class="onebox">**Statistical significance.**

We say that the data provide **statistically significant**\index{hypothesis testing!statistically significant|textbf} evidence against the null hypothesis if the p-value is less than some reference value, often $\alpha=0.05$.</div>\EndKnitrBlock{onebox}

<!--
%\begin{termBox}{\tBoxTitle{Significance Level}
%If the null hypothesis is true, the significance level $\alpha$ defines the probability that we will make a Type 1 Error.}
%\end{termBox}
-->


\BeginKnitrBlock{example}<div class="example">In the opportunity cost study in Section \@ref(caseStudyOpportunityCost), we analyzed an experiment where study participants had a 20% drop in likelihood of continuing with a DVD purchase if they were reminded that the money, if not spent on the DVD, could be used for other purchases in the future. 
We determined that such a large difference would only occur about 1-in-150 times if the reminder actually had no influence on student decision-making. 
What is the p-value in this study? Was the result statistically significant?
 
---
 
The p-value was 0.006 (about 1/150). Since the p-value is less than 0.05, the data provide statistically significant evidence that US college students were actually influenced by the reminder.</div>\EndKnitrBlock{example}




\BeginKnitrBlock{onebox}<div class="onebox">**What's so special about 0.05?**

We often use a threshold of 0.05 to determine whether a result is statistically significant. 
But why 0.05? 
Maybe we should use a bigger number, or maybe a smaller number. 
If you're a little puzzled, that probably means you're reading with a critical eye -- good job! 
We've made a video to help clarify *why 0.05*:
<center>
[https://www.openintro.org/book/stat/why05/](https://www.openintro.org/book/stat/why05/)
</center>
<br>
Sometimes it's also a good idea to deviate from the standard. 
We'll discuss when to choose a threshold different than 0.05 in Section \@ref(two-prop-errors).</div>\EndKnitrBlock{onebox}





### Randomization test summary

<div class="figure" style="text-align: center">
<img src="05/figures/fullrand.png" alt="An example of one simulation of the full randomization procedure.  We repeat the steps hundreds or thousands of times." width="100%" />
<p class="caption">(\#fig:fullrand)An example of one simulation of the full randomization procedure.  We repeat the steps hundreds or thousands of times.</p>
</div>

\index{randomization test}




> **Frame the research question in terms of hypotheses.** Hypothesis tests are appropriate for research questions that can be summarized in two competing hypotheses. The null hypothesis ($H_0$) usually represents a skeptical perspective or a perspective of no difference. The alternative hypothesis ($H_A$) usually represents a new view or a difference. 

> **Collect data with an observational study or experiment.** If a research question can be formed into two hypotheses, we can collect data to run a hypothesis test. If the research question focuses on associations between variables but does not concern causation, we would run an observational study. If the research question seeks a causal connection between two or more variables, then an experiment should be used. 

> **Model the randomness as if the null hypothesis was true.** In the examples above, the variability has been modeled as if the treatment (e.g., gender, opportunity, blood thinner) allocation was independent of the outcome of the study. The computer generated the null distribution from many different randomizations in order to quantify the null variability.

> **Analyze the data.** Choose an analysis technique appropriate for the data and identify the p-value. So far, we've only seen one analysis technique: randomization. Throughout the rest of this textbook, we'll encounter several new methods suitable for many other contexts. 

> **Form a conclusion.** Using the p-value from the analysis, determine whether the data provide statistically significant evidence against the null hypothesis. Also, be sure to write the conclusion in plain language so casual readers can understand the results.


<table class="table" style="margin-left: auto; margin-right: auto;">
<caption>(\#tab:chp5-1-summary)Summary of Randomization Tests as an inferential statistical method.</caption>
 <thead>
  <tr>
   <th style="text-align:left;">  </th>
   <th style="text-align:left;"> Randomization Test </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> What does it do? </td>
   <td style="text-align:left;"> Shuffles the explanatory variable to mimic the natural variability  found in a randomized experiment. </td>
  </tr>
  <tr>
   <td style="text-align:left;"> What is the random process described? </td>
   <td style="text-align:left;"> randomized experiment </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Is there flexibility? </td>
   <td style="text-align:left;"> Yes, can be used to describe random sampling in an observational model </td>
  </tr>
  <tr>
   <td style="text-align:left;"> What is it best for? </td>
   <td style="text-align:left;"> Hypothesis Testing (can be used for Confidence Intervals, but not covered in this text). </td>
  </tr>
  <tr>
   <td style="text-align:left;"> What physical object represents the simulation process? </td>
   <td style="text-align:left;"> shuffling cards </td>
  </tr>
</tbody>
</table>

## Bootstrap confidence interval {#boot-ci}


Randomization is a statistical technique suitable for evaluating whether a difference in sample proportions is due to chance. 
Randomization tests are best suited for modeling experiments where the treatment (explanatory variable) has been randomly assigned to the observational units. 
In this section, we explore the situation where we focus on a single proportion, and we introduce a new simulation method, bootstrapping. 
Bootstrapping is best suited for modeling studies where the data have been generated through random sampling from a population.

Sometime the mathematical theory for how an estimate varies is well-known; this is the case for the sample proportion as seen in Section \@ref(inf-math). 
However, some statistics don't have simple theory for how they vary, and bootstrapping provides a computational approach for providing interval estimates for almost any population parameter (we will revisit bootstrapping in Chapters \@ref(inference-cat), \@ref(inference-num), and \@ref(inference-reg) so you'll get plenty of practice as well as exposure to bootstrapping in many different data settings). 

If the goal is to produce a range of possible values for a population value, then in an ideal world, we would sample data from the population again and recompute the sample proportion. 
Then we could do it again. 
And again. 
And so on until we have a good sense of the variability of our original estimate. 
The ideal world where sampling data is free or extremely cheap is almost never the case, and taking repeated samples from a population is usually impossible. 
So, instead of using a "resample from the population" approach, bootstrapping uses a "resample from the sample" approach.


### Medical consultant case study {#sec-med-consult}

\index{data!medical consultant|(}

People providing an organ for donation sometimes seek the help of a special medical consultant. 
These consultants assist the patient in all aspects of the surgery, with the goal of reducing the possibility of complications during the medical procedure and recovery. 
Patients might choose a consultant based in part on the historical complication rate of the consultant's clients.


#### Observed data {-}

One consultant tried to attract patients by noting the average complication rate for liver donor surgeries in the US is about 10%, but her clients have had only 3 complications in the 62 liver donor surgeries she has facilitated. 
She claims this is strong evidence that her work meaningfully contributes to reducing complications (and therefore she should be hired!).

\BeginKnitrBlock{example}<div class="example">We will let $p$ represent the true complication rate for liver donors working with this consultant. 
(The "true" complication rate will be referred to as the **parameter**.)  
Estimate $p$ using the data, and label this value $\hat{p}$.

---
 
The sample proportion for the complication rate is 3 complications divided by the 62 surgeries the consultant has worked on: $\hat{p} = 3/62 = 0.048$.</div>\EndKnitrBlock{example}

\BeginKnitrBlock{example}<div class="example">Is it possible to assess the consultant's claim using the data?

---

No. The claim is that there is a causal connection, but the data are observational, so we must be on the lookout for confounding variables. 
For example, maybe patients who can afford a medical consultant can afford better medical care, which can also lead to a lower complication rate.

While it is not possible to assess the causal claim, it is still possible to understand the consultant's true rate of complications.</div>\EndKnitrBlock{example}


\BeginKnitrBlock{onebox}<div class="onebox">**Parameter.**\index{parameter}

A **parameter** is the "true" value of interest. 
We typically estimate the parameter using a point estimate\index{point estimate} from a sample of data. 

For example, we estimate the probability $p$ of a complication for a client of the medical consultant by examining the past complications rates of her clients:

 
$$\hat{p} = 3 / 62 = 0.048\qquad\text{is used to estimate}\qquad p$$</div>\EndKnitrBlock{onebox}


<!--
%\begin{termBox}{\tBoxTitle{Point estimates vs parameter}\index{point estimate}\index{parameter}
%Point estimates are calculated based on a sample. For example, the *observed* complication rate for the medical consultant's patients is $\hat{p} = 0.048$. This point estimate is our best guess at the probability $p$ a randomly selected client of hers has a complication. This probability is the parameter and its precise value is never known. However, we can estimate it using the point estimate $\hat{p}$.}
%\end{termBox}


\BeginKnitrBlock{onebox}<div class="onebox">**Null value of a hypothesis test.**

The **null value** is the reference value for the parameter in $H_0$, and it is sometimes represented with the parameter's label with a subscript 0, e.g., $p_0$ (just like $H_0$).</div>\EndKnitrBlock{onebox}

-->



#### Variability of the statistic {-}

In the medical consultant case study, the parameter is $p$, the true probability of a complication for a client of the medical consultant.
There is no reason to believe that $p$ is exactly $\hat{p} = 3/62$, but there is also no reason to believe that $p$ is particularly far from $\hat{p} = 3/62$.
By sampling with replacement from the dataset (a process called **bootstrapping**\index{bootstrapping}), the variability of the possible $\hat{p}$ values can be approximated. 



\BeginKnitrBlock{todo}<div class="todo">need a new physical object!</div>\EndKnitrBlock{todo}

Most of the inferential procedures covered in this text are grounded in quantifying how one data set would differ from another when they are both taken from the same population.
It doesn't make sense to take repeated samples from the same population because if you have the means to take more samples, a larger sample size will benefit you more than the exact same sample twice.
Instead, we measure how the samples behave under an estimate of the population.  Figure \@ref(fig:boot1) shows how the unknown original population can be estimated by using the sample to approximate the proportion of successes and failures (in our case, the proportion of complications and no complications for the medical consultant).

<div class="figure" style="text-align: center">
<img src="05/figures/boot1.png" alt="first figure with the ? pop, then sample, then estimate of the pop.  below i said 3 white (success) and 4 red (failure).  easy to change the text below." width="75%" />
<p class="caption">(\#fig:boot1)first figure with the ? pop, then sample, then estimate of the pop.  below i said 3 white (success) and 4 red (failure).  easy to change the text below.</p>
</div>

By taking repeated samples from the estimated population, the variability from sample to sample can be observed.  In Figure \@ref(fig:boot2) the repeated bootstrap samples are obviously different both from each other and from the original population.
Recall that the bootstrap samples were taken from the same (estimated) population, and so the differences are due entirely to natural variability in the sampling procedure.

<div class="figure" style="text-align: center">
<img src="05/figures/boot2.png" alt="next fig, has the bootstrap samples" width="75%" />
<p class="caption">(\#fig:boot2)next fig, has the bootstrap samples</p>
</div>

By summarizing each of the bootstrap samples (here, using the sample proportion), we see, directly, the variability of the sample proportion, $\hat{p}$, from sample to sample.
The distribution of $\hat{p}_{bs}$ for the example scenario is shown in Figure \@ref(fig:boot3), and the bootstrap distribution for the medical consultant data is shown in Figure \@ref(fig:MedConsBSSim).

<div class="figure" style="text-align: center">
<img src="05/figures/boot3.png" alt="WITH ADDED HISTOGRAM... boot samples, arrow, histogram of all of them" width="75%" />
<p class="caption">(\#fig:boot3)WITH ADDED HISTOGRAM... boot samples, arrow, histogram of all of them</p>
</div>


It turns out that in practice, it is very difficult for computers to work with an infinite population (with the same proportional breakdown as in the sample).
However, there is a physical and computational model which produces an equivalent bootstrap distribution of the sample proportion in a computationally efficient manner.
Consider the observed data to be a bag of marbles 3 of which are success (white) and 4 of which are failures (red).  By drawing the marbles out of the bag with replacement, we depict the same sampling **process** as was done with the infinitely large estimated population.
Note in Figure \@ref(fig:boot4) that when sampling the original observations, a particular data point may end up in the new sample one time (evidenced by a circle around the observation), two times (evidenced by two circles around the observation), or not at all (no circles around the observation).  

If we apply the bootstrap sampling process to the medical consultant example, we consider each client to be one of the marbles in the bag.
There will be 59 white marbles (no complication) and 3 red marbles (complication).
If we 62 choose marbles out of the bag (one at a time) and compute the proportion of simulated patients with complications, $\hat{p}_{bs}$, then this "bootstrap" proportion represents a single simulated proportion from the "resample from the sample" approach.

<div class="figure" style="text-align: center">
<img src="05/figures/boot4.png" alt="sampling with replacement figure" width="75%" />
<p class="caption">(\#fig:boot4)sampling with replacement figure</p>
</div>

\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">In a simulation of 62 patients, about how many would we expect to have had a complication?^[About 5% of the patients (6.2 on average) in the simulation will have a complication, though we will see a little variation from one simulation to the next.]</div>\EndKnitrBlock{guidedpractice}

Figure \@ref(fig:boot5) visualizes one simulation for the medical consultant.
Out of the simulated cases, there were 5 with a complication and 57 without a complication: $\hat{p}_{bs} = 5/62 = 0.081$.

<div class="figure" style="text-align: center">
<img src="05-inference-foundations_files/figure-html/boot5-1.png" alt="how impossible would it be to create one simulation with 62 marbles? 3 red, 59 white?" width="75%" />
<p class="caption">(\#fig:boot5)how impossible would it be to create one simulation with 62 marbles? 3 red, 59 white?</p>
</div>

One simulation isn't enough to get a sense of the variability from one bootstrap proportion to another bootstrap proportion, so we repeated the simulation 10,000 times using a computer. 
Figure \@ref(fig:MedConsBSSim) shows the distribution from the 10,000 bootstrap simulations. 
The bootstrapped proportions vary from about zero to 11.3%. 
The variability in the bootstrapped proportions leads us to believe that the true probability of complication (the parameter, $p$) is somewhere between 0 and 11.3%.
<!--The simulated proportions that are less than or equal to $\hat{p}=0.048$ are shaded.
There were 1222 simulated sample proportions with $\hat{p}_{bs} \leq 0.048$, which represents a fraction 0.1222 of our simulations:
-->

<div class="figure" style="text-align: center">
<img src="05-inference-foundations_files/figure-html/MedConsBSSim-1.png" alt="The original medical consultant data is bootstrapped 10,000 times. Each simulation creates a sample from the original data where the probability of a complication is $\hat{p} = 3/62$. The bootstrap 2.5 percentile proportion is 0 and the 97.5 percentile is 0.113. The result is: we are confident that, in the population, the true probability of a complication is between 0% and 11.3%." width="70%" />
<p class="caption">(\#fig:MedConsBSSim)The original medical consultant data is bootstrapped 10,000 times. Each simulation creates a sample from the original data where the probability of a complication is $\hat{p} = 3/62$. The bootstrap 2.5 percentile proportion is 0 and the 97.5 percentile is 0.113. The result is: we are confident that, in the population, the true probability of a complication is between 0% and 11.3%.</p>
</div>

<!--
\begin{align*}
\text{left tail }
	= \frac{\text{Number of observed simulations with }\hat{p}_{bs}\leq\text{ 0.048}}{10000}
	= \frac{1222}{10000} = 0.1222
\end{align*}
However, this is not our p-value! Remember that we are conducting a two-sided test, so we should double the one-tail area to get the p-value:\footnote{This doubling approach is preferred even when the distribution isn't symmetric, as in this case.}
\begin{align*}
\text{p-value} = 2 \times \text{left tail} = 2 \times 0.1222 = 0.2444
\end{align*}

\begin{figure}[ht]
\centering
\includegraphics[width=0.83\textwidth]{02/figures/MedicalConsultant/MedConsBSSim}
\caption{The null distribution for $\hat{p}$, created from 10,000 simulated studies. The left tail contains 12.22% of the simulations. We double this value to get the p-value.}
\label{MedConsBSSim}
\end{figure}

\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">Because the p-value is 0.2444, which is larger than the significance level 0.05, we do not reject the null hypothesis. Explain what this means in the context of the problem using plain language.^[The data do not provide strong evidence that the consultant's work is associated with a lower or higher rate of surgery complications than the general rate of 10%.]</div>\EndKnitrBlock{guidedpractice}
-->

\BeginKnitrBlock{example}<div class="example">The original claim was that the consultant's true rate of complication was under the national rate of 10%. Does the interval estimate of 0 to 11.3% for the true probability of complication indicate that the surgical consultant has a lower rate of complications than the national average?
Explain.

---

No. Because the interval overlaps 10%, it might be that the consultant's work is associated with a lower risk of complciations, or it might be that the consulant's work is associated with a higher risk (i.e., greater than 10%) of complications! Additionally, as previously mentioned, because this is an observational study, even if an association can be measured, there is no evidence that the consultant's work is the cause of the complication rate (being higher or lower). </div>\EndKnitrBlock{example}

<!--
%However, we currently don't have enough data to say whether the corresponding complication rate is any different than 0.10.
-->
\index{data!medical consultant|)}


### Tappers and listeners case study

Here's a game you can try with your friends or family: pick a simple, well-known song, tap that tune on your desk, and see if the other person can guess the song. 
In this simple game, you are the tapper, and the other person is the listener.

#### Observed data {-}

A Stanford University graduate student named Elizabeth Newton conducted an experiment using the tapper-listener game.^[This case study is described in [Made to Stick](http://www.openintro.org/redirect.php?go=made-to-stick&redirect=simulation_textbook_pdf_preliminary) by Chip and Dan Heath. Little known fact: the teaching principles behind many OpenIntro resources are based on *Made to Stick*.] In her study, she recruited 120 tappers and 120 listeners into the study. 
About 50% of the tappers expected that the listener would be able to guess the song. 
Newton wondered, is 50% a reasonable expectation?

<!--
Newton's research question can be framed into two hypotheses:

* $H_0$: The tappers are correct, and generally 50% of the time listeners are able to guess the tune. $p = 0.50$

* $H_A$: The tappers are incorrect, and either more than or less than 50% of listeners will be able to guess the tune. $p \neq 0.50$
-->


In Newton's study, only 3 out of 120 listeners ($\hat{p} = 0.025$) were able to guess the tune! That seems like quite a low number which leads the researcher to ask: what is the true proportion of people who can guess the tune?

<!--
From the perspective of the null hypothesis, we might wonder, how likely is it that we would get this result from chance alone? 
That is, what's the chance we would happen to see such a small fraction if $H_0$ were true and the true correct-guess rate is 0.50?
-->

\BeginKnitrBlock{todo}<div class="todo">need a new physical object!</div>\EndKnitrBlock{todo}

#### Variability of the statistic {-}

To answer the question, we will again use a simulation. 
To simulate 120 games, this time we use a bag of 120 marbles 3 are red and 117 are white.
Sampling from the bag 120 times (while not actually removing the marbles from the bag) produces one bootstrap sample.   
<!--
To simulate 120 games under the null hypothesis where $p = 0.50$, we could flip a coin 120 times. 
Each time the coin came up heads, this could represent the listener guessing correctly, and tails would represent the listener guessing incorrectly. 
-->


For example, we can start by simulating 5 tapper-listener pairs by sampling 5 marbles from the bag of 3 red and 117 white marbles.

| W | W | W | R | W |
|:-----:|:-----:|:-----:|:-------:|:-----:|
| Wrong | Wrong | Wrong | Correct | Wrong |


After selecting 120 marbles, we counted 2 red for $\hat{p}_{bs} = 0.0167$. 
As we did with the randomization technique, seeing what would happen with one simulation isn't enough. 
In order to evaluate whether our originally observed proportion of 0.025 is unusual or not, we should generate more simulations. 
Here we've repeated the entire simulation ten times:
\begin{align*}
0.0417 \quad 0.0250 \quad 0.0250 \quad 0.0083 \quad
0.0500 \quad 0.0333 \quad 0.0250 \quad 0.000 \quad
0.0083 \quad 0.000
\end{align*} <!-- % round(rbinom(10, 120, 0.5) / 120, 3) -->
As before, we'll run a total of 10,000 simulations using a computer. 
As seen in Figure \@ref(fig:tappers-bs-sim), the range of 95% of the resampled $\hat{p}_{bs}$ is 0.000 to 0.0583. 
That is, we expect that between 0% and 5.83% of people are truly able to guess the tapper's tune.

<div class="figure" style="text-align: center">
<img src="05-inference-foundations_files/figure-html/tappers-bs-sim-1.png" alt="The original listener-tapper data is bootstrapped 10,000 times. Each simulation creates a sample where the probability of being correct is $\hat{p} = 3/120$. The 2.5 percentile proportion is 0 and the 97.5 percentile is 0.0583. The result is that we are confident that, in the population, the true percent of people who can guess correctly is between 0% and 5.83%." width="70%" />
<p class="caption">(\#fig:tappers-bs-sim)The original listener-tapper data is bootstrapped 10,000 times. Each simulation creates a sample where the probability of being correct is $\hat{p} = 3/120$. The 2.5 percentile proportion is 0 and the 97.5 percentile is 0.0583. The result is that we are confident that, in the population, the true percent of people who can guess correctly is between 0% and 5.83%.</p>
</div>

<!--
Figure \@ref(fig:TappersAndListenersNullDistribution) shows the results of these simulations. Even in these 10,000 simulations, we don't see any results close to 0.025.

\begin{figure}[ht]
\centering
\includegraphics[width=0.8\textwidth]{02/figures/TappersAndListeners/TappersAndListenersNullDistribution}
\caption{Results from 10,000 simulations of the tapper-listener study where guesses are correct half of the time.}
\label{TappersAndListenersNullDistribution}
\end{figure}
-->

<!--
\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">What is the p-value for the hypothesis test?^[The p-value is the chance of seeing the data summary or something more in favor of the alternative hypothesis. Since we didn't observe anything even close to just 3 correct, the p-value will be small, around 1-in-10,000 or smaller.]</div>\EndKnitrBlock{guidedpractice}
-->

\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">Do the data provide statistically significant evidence against the claim that 50% of listeners can guess the tapper's tune?^[Because 50% is not in the interval estimate for the true parameter, there is statistically significant evidence. 
Indeed, the data provide strong evidence that the chance a listener will guess the correct tune is less than 50%.]</div>\EndKnitrBlock{guidedpractice}


### Confidence intervals {#ConfidenceIntervals}

\index{confidence interval|(}

A point estimate provides a single plausible value for a parameter. 
However, a point estimate is rarely perfect; usually there is some error in the estimate. 
In addition to supplying a point estimate of a parameter, a next logical step would be to provide a plausible *range of values* for the parameter.


#### Population parameter {-}

A plausible range of values for the population parameter is called a **confidence interval**. 
Using only a single point estimate is like fishing in a murky lake with a spear, and using a confidence interval is like fishing with a net. 
We can throw a spear where we saw a fish, but we will probably miss. 
On the other hand, if we toss a net in that area, we have a good chance of catching the fish.

If we report a point estimate, we probably will not hit the exact population parameter. 
On the other hand, if we report a range of plausible values -- a confidence interval -- we have a good shot at capturing the parameter.

\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">If we want to be very certain we capture the population parameter, should we use a wider interval or a smaller interval?^[If we want to be more certain we will capture the fish, we might use a wider net. Likewise, we use a wider confidence interval if we want to be more certain that we capture the parameter.]</div>\EndKnitrBlock{guidedpractice}

#### Bootstrap confidence interval {-}


As we saw above, a **bootstrap sample**\index{bootstrap sample} is a sample of the original sample. 
In the case of the medical complications data, we proceed as follows:

\BeginKnitrBlock{todo}<div class="todo">need a new physical object!</div>\EndKnitrBlock{todo}

1. Randomly sample one observation from the 62 patients (sample, without removing the marbles, 62 marbles from the bag).
2. Randomly sample a second observation from the 62 patients. Because we sample with replacement (i.e., we don't actually remove the marbles from the bag), there is a 1-in-62 chance that the second observation will be the same one sampled in the first step!
...
62. Randomly sample a 62nd observation from the 62 patients.



Bootstrap sampling is often called **sampling with replacement**. 
A bootstrap sample behaves similarly to how an actual sample would behave, and we compute the point estimate of interest (here, compute $\hat{p}_{bs}$). 

Due to theory that is beyond this text, we know that the bootstrap proportions $\hat{p}_{bs}$ vary around $\hat{p}$ in a similar way to how different sample (i.e., different datasets which would produce different $\hat{p}$ values) proportions vary around the true parameter $p$. 
Therefore, an interval estimate for $p$ can be produced using the $\hat{p}_{bs}$ values themselves.

\BeginKnitrBlock{onebox}<div class="onebox">**95% Bootstrap confidence interval for a parameter $p$.**

The 95% bootstrap confidence interval for the parameter $p$ can be obtained directly using the ordered values $\hat{p}_{bs}$ values. Consider the sorted $\hat{p}_{bs}$ values, and let $\hat{p}_{bs, 0.025}$ be the 2.5% value and $\hat{p}_{bs, 0.975}$ be the 97.5% value. The 95% confidence interval is given by:
<center>
($\hat{p}_{bs, 0.025}$, $\hat{p}_{bs, 0.975}$)
</center></div>\EndKnitrBlock{onebox}

In Section \@ref(one-prop-null-boot) we will discuss different percentages for the confidence interval (e.g., 90% confidence interval or 99% confidence interval).  Section \@ref(one-prop-null-boot) also provides a longer discussion on what "95% confidence" actually means.

### Bootstrap summary

\BeginKnitrBlock{todo}<div class="todo">Bootstrap flow chart</div>\EndKnitrBlock{todo}


<div class="figure" style="text-align: center">
<img src="05/figures/bootboth.png" alt="(not sure if this should include a histogram or not) the infinite pop compared to with replacement" width="75%" />
<p class="caption">(\#fig:bootboth)(not sure if this should include a histogram or not) the infinite pop compared to with replacement</p>
</div>

<table class="table" style="margin-left: auto; margin-right: auto;">
<caption>(\#tab:chp5-2-summary)Summary of bootstrapping as an inferential statistical method.</caption>
 <thead>
  <tr>
   <th style="text-align:left;">  </th>
   <th style="text-align:left;"> Bootstrapping </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> What does it do? </td>
   <td style="text-align:left;"> Resamples (with replacement) from the observed data to mimic the sampling variability found by collecting data. </td>
  </tr>
  <tr>
   <td style="text-align:left;"> What is the random process described? </td>
   <td style="text-align:left;"> random sampling </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Is there flexibility? </td>
   <td style="text-align:left;"> Yes, can be used to describe random allocation in an experiment </td>
  </tr>
  <tr>
   <td style="text-align:left;"> What is it best for? </td>
   <td style="text-align:left;"> Confidence Intervals (HT for one proportion covered in Chapter 6). </td>
  </tr>
  <tr>
   <td style="text-align:left;"> What physical object represents the simulation process? </td>
   <td style="text-align:left;"> pulling balls from a bag </td>
  </tr>
</tbody>
</table>

## Mathematical model {#inf-math}

### Central Limit Theorem {#CLTsection}

We've encountered four case studies so far this chapter. 
While they differ in the settings, in their outcomes, and also in the technique we've used to analyze the data, they all have something in common: the general shape of the null distribution.

#### Null distribution {-}

Figure \@ref(fig:FourCaseStudies) shows the null distributions in each of the four case studies where we ran 10,000 simulations. 
In the case of the opportunity cost study, which originally had just 1,000 simulations, we've included an additional 9,000 simulations.

\BeginKnitrBlock{todo}<div class="todo">
"The null distribution for each of the four case studies presented in Sections \ref{caseStudyOpportunityCost}-\ref{SimulationCaseStudies}."</div>\EndKnitrBlock{todo}


<div class="figure" style="text-align: center">
<img src="05-inference-foundations_files/figure-html/FourCaseStudies-1.png" alt="The null distribution for each of the four case studies presented previously in this Chapter." width="70%" /><img src="05-inference-foundations_files/figure-html/FourCaseStudies-2.png" alt="The null distribution for each of the four case studies presented previously in this Chapter." width="70%" /><img src="05-inference-foundations_files/figure-html/FourCaseStudies-3.png" alt="The null distribution for each of the four case studies presented previously in this Chapter." width="70%" /><img src="05-inference-foundations_files/figure-html/FourCaseStudies-4.png" alt="The null distribution for each of the four case studies presented previously in this Chapter." width="70%" />
<p class="caption">(\#fig:FourCaseStudies)The null distribution for each of the four case studies presented previously in this Chapter.</p>
</div>

\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">Describe the shape of the distributions and note anything that you find interesting.^[In general, the distributions are reasonably symmetric. The case study for the medical consultant is the only distribution with any evident skew.]</div>\EndKnitrBlock{guidedpractice}

As we observed in Chapter \@ref(intro-to-data), it's common for distributions to be skewed or contain outliers. 
However, the null distributions we've so far encountered have all looked somewhat similar and, for the most part, symmetric. 
They all resemble a bell-shaped curve. 
This is not a coincidence, but rather, is guaranteed by mathematical theory.

\BeginKnitrBlock{onebox}<div class="onebox">**Central Limit Theorem for proportions.**\index{Central Limit Theorem}

If we look at a proportion (or difference in proportions) and the scenario satisfies certain conditions, then the sample proportion (or difference in proportions) will appear to follow a bell-shaped curve called the *normal distribution*.\index{Central Limit Theorem}\index{Central Limit Theorem!proportion}\index{Central Limit Theorem!difference in proportions}</div>\EndKnitrBlock{onebox}



An example of a perfect normal distribution is shown in Figure \@ref(fig:simpleNormal). 
Imagine laying a normal curve over each of the four null distributions in Figure \@ref(fig:FourCaseStudies). 
While the mean (center) and standard deviation (width or spread) may change for each plot, the general shape remains roughly intact.


<div class="figure" style="text-align: center">
<img src="05-inference-foundations_files/figure-html/simpleNormal-1.png" alt="A normal curve." width="70%" />
<p class="caption">(\#fig:simpleNormal)A normal curve.</p>
</div>

Mathematical theory guarantees that if repeated samples are taken a sample proportion or a difference in sample proportions will follow something that resembles a normal distribution when certain conditions are met. (Note: we typically only take **one** sample, but the mathematical model lets us know what to expect if we *had* taken repeated samples.)
These conditions fall into two categories:

> Observations in the sample are independent. Independence is guaranteed when we take a random sample from a population. It can also be guaranteed if we randomly divide individuals into treatment and control groups.

> The sample is large enough. The sample size cannot be too small. What qualifies as "small" differs from one context to the next, and we'll provide suitable guidelines for proportions in Chapter \@ref(inference-cat).


So far we've had no need for the normal distribution. 
We've been able to answer our questions somewhat easily using simulation techniques. However, soon this will change. 
Simulating data can be non-trivial. 
For example, some of the scenarios encountered in Chapters \@ref(cor-reg) and \@ref(mult-reg) would require complex simulations in order to make inferential conclusions. 
Instead, the normal distribution and other distributions like it offer a general framework for statistical inference that applies to a very large number of settings.


#### Anticipating frequent use of the normal distribution {-}

Below we introduce three new settings where the normal distribution will be useful, and constructing suitable simulations can be difficult.

\BeginKnitrBlock{example}<div class="example">The opportunity cost study determined that students are thriftier if they are reminded that saving money now means they can spend the money later. The study's point estimate for the estimated impact was 20%, meaning 20% fewer students would move forward with a DVD purchase in the study scenario. However, as we've learned, point estimates aren't perfect -- they only provide an approximation of the truth.

---

It would be useful if we could provide a *range of plausible values* for the impact, more formally known as a **confidence interval**. It is often difficult to construct a reliable confidence interval in many situations using simulations.^[The percentile bootstrap method has been introduced in Section \@ref(ConfidenceIntervals).  We will revisit bootstrap intervals in Section \@ref(two-prop-boot-ci).] However, doing so is reasonably straightforward using the normal distribution. </div>\EndKnitrBlock{example}

\BeginKnitrBlock{example}<div class="example">Book prices were collected for 73 courses at UCLA in Spring 2010. Data were collected from both the UCLA Bookstore and Amazon. The differences in these prices are shown in Figure \@ref(fig:diffInTextbookPricesS10-CLTsection). The mean difference in the price of the books was $12.76, and we might wonder, does this provide strong evidence that the prices differ between the two book sellers?
 
---
 
Here again we can apply the normal distribution, this time in the context of numerical data. We'll explore this example and construct such a hypothesis test in Section \@ref(paired-data).</div>\EndKnitrBlock{example}



<div class="figure" style="text-align: center">
<img src="05-inference-foundations_files/figure-html/diffInTextbookPricesS10-CLTsection-1.png" alt="Histogram of the difference in price for each book sampled. These data are strongly skewed." width="70%" />
<p class="caption">(\#fig:diffInTextbookPricesS10-CLTsection)Histogram of the difference in price for each book sampled. These data are strongly skewed.</p>
</div>

\index{skew!example: strong}\textA{\vspace{-3mm}}

\BeginKnitrBlock{example}<div class="example">Elmhurst College in Illinois released anonymized data for family income and financial support provided by the school for Elmhurst's first-year students in 2011. Figure \@ref(fig:elmhurstScatterWLSROnly-CLTsection) shows a *regression line* fit to a scatterplot of a sample of the data. One question we will ask is, do the data show a real trend, or is the trend we observe reasonably explained by chance?

---

In Chapter \@ref(cor-reg) we learned how to apply least squares regression to quantify the trend.  In Chapter \@ref(inference-reg) we will focus on whether or not that trend can be explained by chance alone. For this case study, we could again use the normal distribution to help us answer this question.</div>\EndKnitrBlock{example}



<div class="figure" style="text-align: center">
<img src="05-inference-foundations_files/figure-html/elmhurstScatterWLSROnly-CLTsection-1.png" alt="Gift aid and family income for a random sample of 50 first-year students from Elmhurst College, shown with a regression line." width="70%" />
<p class="caption">(\#fig:elmhurstScatterWLSROnly-CLTsection)Gift aid and family income for a random sample of 50 first-year students from Elmhurst College, shown with a regression line.</p>
</div>


These examples highlight the value of the normal distribution approach. 
However, before we can apply the normal distribution to statistical inference, it is necessary to become familiar with the mechanics of the normal distribution. 
In Section \@ref(normalDist) we discuss characteristics of the normal distribution, explore examples of data that follow a normal distribution, and learn a new plotting technique that is useful for evaluating whether a data set roughly follows the normal distribution. 
In Section \@ref(ApplyingTheNormalModel), we apply the new knowledge in the context of hypothesis tests and confidence intervals.


### Normal Distribution {#normalDist}

\index{normal distribution|(}


Among all the distributions we see in statistics, one is overwhelmingly the most common. 
The symmetric, unimodal, bell curve is ubiquitous throughout statistics. 
It is so common that people know it as a variety of names including the **normal curve**\index{normal curve}, **normal model**\index{normal model}, or **normal distribution**\index{normal distribution}.^[It is also introduced as the Gaussian distribution after Frederic Gauss, the first person to formalize its mathematical expression.] 
Under certain conditions, sample proportions, sample means, and sample differences can be modeled using the normal distribution. 
Additionally, some variables such as SAT scores and heights of US adult males closely follow the normal distribution.





\BeginKnitrBlock{onebox}<div class="onebox">**Normal distribution facts.**

Many summary statistics and variables are nearly normal, but none are exactly normal. 
Thus the normal distribution, while not perfect for any single problem, is very useful for a variety of problems. 
We will use it in data exploration and to solve important problems in statistics.</div>\EndKnitrBlock{onebox}

In this section, we will discuss the normal distribution in the context of data to become familiar with normal distribution techniques. 
In the following sections and beyond, we'll move our discussion to focus on applying the normal distribution and other related distributions to model point estimates for hypothesis tests and for constructing confidence intervals.

#### Normal distribution model {-}


The normal distribution always describes a symmetric, unimodal, bell-shaped curve. 
However, normal curves can look different depending on the details of the model. 
Specifically, the normal model can be adjusted using two parameters: mean and standard deviation. 
As you can probably guess, changing the mean shifts the bell curve to the left or right, while changing the standard deviation stretches or constricts the curve. 
Figure \@ref(fig:twoSampleNormals) shows the normal distribution with mean $0$ and standard deviation $1$  (which is commonly referred to as the **standard normal distribution**\index{standard normal distribution}) on top.
A normal distributions with mean $19$ and standard deviation $4$ is shown on the bottom. Figure \@ref(fig:twoSampleNormalsStacked) shows the same two normal distributions on the same axis.



<div class="figure" style="text-align: center">
<img src="05-inference-foundations_files/figure-html/twoSampleNormals-1.png" alt="Both curves represent the normal distribution, however, they differ in their center and spread. The normal distribution with mean 0 and standard deviation 1 is called the **standard normal distribution**." width="70%" /><img src="05-inference-foundations_files/figure-html/twoSampleNormals-2.png" alt="Both curves represent the normal distribution, however, they differ in their center and spread. The normal distribution with mean 0 and standard deviation 1 is called the **standard normal distribution**." width="70%" />
<p class="caption">(\#fig:twoSampleNormals)Both curves represent the normal distribution, however, they differ in their center and spread. The normal distribution with mean 0 and standard deviation 1 is called the **standard normal distribution**.</p>
</div>






\BeginKnitrBlock{todo}<div class="todo">still can't figure out references

The normal models shown in Figure \ref{twoSampleNormals} but plotted together and on the same scale.</div>\EndKnitrBlock{todo}

<div class="figure" style="text-align: center">
<img src="05-inference-foundations_files/figure-html/twoSampleNormalsStacked-1.png" alt="The two normal models shown above and now plotted together on the same scale." width="70%" />
<p class="caption">(\#fig:twoSampleNormalsStacked)The two normal models shown above and now plotted together on the same scale.</p>
</div>

If a normal distribution has mean $\mu$ and standard deviation $\sigma$, we may write the distribution as $N(\mu, \sigma)$\marginpar[\raggedright\vspace{-5mm}

$N(\mu, \sigma)$\vspace{1mm}\\\footnotesize Normal dist.\\with mean $\mu$\\\& st. dev. $\sigma$]{\raggedright\vspace{-5mm}

$N(\mu, \sigma)$\vspace{1mm}\\\footnotesize Normal dist.\\with mean $\mu$\\\& st. dev. $\sigma$}. The two distributions in Figure \@ref(fig:twoSampleNormalsStacked) can be written as
\begin{align*}
N(\mu=0,\sigma=1)\quad\text{and}\quad N(\mu=19,\sigma=4)
\end{align*}
Because the mean and standard deviation describe a normal distribution exactly, they are called the distribution's **parameters**\index{parameter}.



\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">Write down the short-hand for a normal distribution with (a) mean 5 and standard deviation 3, (b) mean -100 and standard deviation 10, and (c) mean 2 and standard deviation 9.^[(a) $N(\mu=5,\sigma=3)$. (b) $N(\mu=-100, \sigma=10)$. (c) $N(\mu=2, \sigma=9)$.]</div>\EndKnitrBlock{guidedpractice}


#### Standardizing with Z scores {-}

\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">Table \@ref(tab:satACTstats) shows the mean and standard deviation for total scores on the SAT and ACT. The distribution of SAT and ACT scores are both nearly normal. Suppose Ann scored 1800 on her SAT and Tom scored 24 on his ACT. Who performed better?^[We use the standard deviation as a guide. Ann is 1 standard deviation above average on the SAT: $1500 + 300=1800$. Tom is 0.6 standard deviations above the mean on the ACT: $21+0.6\times 5=24$. In Figure \@ref(fig:satActNormals), we can see that Ann tends to do better with respect to everyone else than Tom did, so her score was better.]</div>\EndKnitrBlock{guidedpractice}


<table class="table" style="margin-left: auto; margin-right: auto;">
<caption>(\#tab:satACTstats)Mean and standard deviation for the SAT and ACT.</caption>
 <thead>
  <tr>
   <th style="text-align:left;">  </th>
   <th style="text-align:left;"> SAT </th>
   <th style="text-align:left;"> ACT </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Mean </td>
   <td style="text-align:left;"> 1500 </td>
   <td style="text-align:left;"> 21 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> SD </td>
   <td style="text-align:left;"> 300 </td>
   <td style="text-align:left;"> 5 </td>
  </tr>
</tbody>
</table>

<div class="figure" style="text-align: center">
<img src="05-inference-foundations_files/figure-html/satActNormals-1.png" alt="Ann's and Tom's scores shown with the distributions of SAT and ACT scores." width="70%" />
<p class="caption">(\#fig:satActNormals)Ann's and Tom's scores shown with the distributions of SAT and ACT scores.</p>
</div>


The solution to the previous example relies on a standardization technique called a Z score, a method most commonly employed for nearly normal observations (but that may be used with any distribution). The **Z score**\index{Z score}\marginpar[\raggedright\vspace{-3mm}


$Z$\vspace{1mm}\\\footnotesize Z score, the\\standardized\\observation]{\raggedright\vspace{-3mm}

$Z$\vspace{1mm}\\\footnotesize Z score, the\\standardized\\observation}\index{Z@$Z$} of an observation is defined as the number of standard deviations it falls above or below the mean. 
If the observation is one standard deviation above the mean, its Z score is 1. If it is 1.5 standard deviations *below* the mean, then its Z score is -1.5. 
If $x$ is an observation from a distribution $N(\mu, \sigma)$, we define the Z score mathematically as



\begin{eqnarray*}
Z = \frac{x-\mu}{\sigma}
\end{eqnarray*}
Using $\mu_{SAT}=1500$, $\sigma_{SAT}=300$, and $x_{Ann}=1800$, we find Ann's Z score:
\begin{eqnarray*}
Z_{Ann} = \frac{x_{Ann} - \mu_{SAT}}{\sigma_{SAT}} = \frac{1800-1500}{300} = 1
\end{eqnarray*}


\BeginKnitrBlock{onebox}<div class="onebox">**The Z score.**

The Z score of an observation is the number of standard deviations it falls above or below the mean. 
We compute the Z score for an observation $x$ that follows a distribution with mean $\mu$ and standard deviation $\sigma$ using
\begin{eqnarray*}
Z = \frac{x-\mu}{\sigma}
\end{eqnarray*}</div>\EndKnitrBlock{onebox}


\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">Use Tom's ACT score, 24, along with the ACT mean and standard deviation to compute his Z score.^[$Z_{Tom} = \frac{x_{Tom} - \mu_{ACT}}{\sigma_{ACT}} = \frac{24 - 21}{5} = 0.6$]</div>\EndKnitrBlock{guidedpractice}

Observations above the mean always have positive Z scores while those below the mean have negative Z scores. 
If an observation is equal to the mean (e.g., SAT score of 1500), then the Z score is $0$.


\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">Let $X$ represent a random variable from $N(\mu=3, \sigma=2)$, and suppose we observe $x=5.19$. (a) Find the Z score of $x$. (b) Use the Z score to determine how many standard deviations above or below the mean $x$ falls.^[(a) Its Z score is given by $Z = \frac{x-\mu}{\sigma} = \frac{5.19 - 3}{2} = 2.19/2 = 1.095$. (b) The observation $x$ is 1.095 standard deviations *above* the mean. We know it must be above the mean since $Z$ is positive.]</div>\EndKnitrBlock{guidedpractice}


\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">Head lengths of brushtail possums follow a nearly normal distribution with mean 92.6 mm and standard deviation 3.6 mm. Compute the Z scores for possums with head lengths of 95.4 mm and 85.8 mm.^[For $x_1=95.4$ mm: $Z_1 = \frac{x_1 - \mu}{\sigma} = \frac{95.4 - 92.6}{3.6} = 0.78$. For $x_2=85.8$ mm: $Z_2 = \frac{85.8 - 92.6}{3.6} = -1.89$.]</div>\EndKnitrBlock{guidedpractice}

We can use Z scores to roughly identify which observations are more unusual than others. 
One observation $x_1$ is said to be more unusual than another observation $x_2$ if the absolute value of its Z score is larger than the absolute value of the other observation's Z score: $|Z_1| > |Z_2|$. 
This technique is especially insightful when a distribution is symmetric.

\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">Which of the two brushtail possum observations in the previous guided practice is more *unusual*?^[Because the *absolute value* of Z score for the second observation is larger than that of the first, the second observation has a more unusual head length.]</div>\EndKnitrBlock{guidedpractice}


#### Normal probability calculations {-}

\BeginKnitrBlock{example}<div class="example">Ann from the SAT Guided Practice earned a score of 1800 on her SAT with a corresponding $Z=1$. She would like to know what percentile she falls in among all SAT test-takers.

---
 
Ann's **percentile**\index{percentile} is the percentage of people who earned a lower SAT score than Ann. We shade the area representing those individuals in Figure \@ref(fig:satBelow1800). The total area under the normal curve is always equal to 1, and the proportion of people who scored below Ann on the SAT is equal to the *area* shaded in Figure \@ref(fig:satBelow1800): 0.8413. In other words, Ann is in the $84^{th}$ percentile of SAT takers.</div>\EndKnitrBlock{example}




<div class="figure" style="text-align: center">
<img src="05-inference-foundations_files/figure-html/satBelow1800-1.png" alt="The normal model for SAT scores, shading the area of those individuals who scored below Ann." width="70%" />
<p class="caption">(\#fig:satBelow1800)The normal model for SAT scores, shading the area of those individuals who scored below Ann.</p>
</div>


We can use the normal model to find percentiles or probabilities. 
A **normal probability table**\index{normal probability table}, which lists Z scores and corresponding percentiles, can be used to identify a percentile based on the Z score (and vice versa). 
Statistical software can also be used.





Normal probabilities are most commonly found using statistical software which we will show here using R. 
We use the software to identify the percentile corresponding to any particular Z score. 
For instance, the percentile of $Z=0.43$ is 0.6664, or the $66.64^{th}$ percentile. 
The `pnorm()` function is available in default R and will provide the percentile associated with any cutoff on a normal curve. 
The `normTail()` function is available in the openintro R package and will draw the associated curve if it is helpful.



```r
pnorm(0.43, m = 0, s = 1)
#> [1] 0.666
openintro::normTail(0.43, m = 0, s = 1)
```

<img src="05-inference-foundations_files/figure-html/unnamed-chunk-72-1.png" width="70%" style="display: block; margin: auto;" />

<!--
<div class="figure" style="text-align: center">
<img src="05-inference-foundations_files/figure-html/normalTails-1.png" alt="The area to the left of $Z$ represents the percentile of the observation." width="70%" /><img src="05-inference-foundations_files/figure-html/normalTails-2.png" alt="The area to the left of $Z$ represents the percentile of the observation." width="70%" />
<p class="caption">(\#fig:normalTails)The area to the left of $Z$ represents the percentile of the observation.</p>
</div>
-->


<!--
\begin{table}
\centering
\begin{tabular}{c | rrrrr | rrrrr |}
 \cline{2-11}
&&&& \multicolumn{4}{c}{Second decimal place of $Z$} &&& \\
 \cline{2-11}
$Z$ & 0.00 & 0.01 & 0.02 & \highlightT{0.03} & \highlightO{0.04} & 0.05 & 0.06 & 0.07 & 0.08 & 0.09 \\
 \hline
 \hline
0.0 & \scriptsize{0.5000} & \scriptsize{0.5040} & \scriptsize{0.5080} & \scriptsize{0.5120} & \scriptsize{0.5160} & \scriptsize{0.5199} & \scriptsize{0.5239} & \scriptsize{0.5279} & \scriptsize{0.5319} & \scriptsize{0.5359} \\
 0.1 & \scriptsize{0.5398} & \scriptsize{0.5438} & \scriptsize{0.5478} & \scriptsize{0.5517} & \scriptsize{0.5557} & \scriptsize{0.5596} & \scriptsize{0.5636} & \scriptsize{0.5675} & \scriptsize{0.5714} & \scriptsize{0.5753} \\
 0.2 & \scriptsize{0.5793} & \scriptsize{0.5832} & \scriptsize{0.5871} & \scriptsize{0.5910} & \scriptsize{0.5948} & \scriptsize{0.5987} & \scriptsize{0.6026} & \scriptsize{0.6064} & \scriptsize{0.6103} & \scriptsize{0.6141} \\
% May comment out 0.0-0.2 to make extra space. Then insert the following line:
% $\vdots$ & $\vdots$ &  $\vdots$ &  $\vdots$ &  $\vdots$ &  $\vdots$ &  $\vdots$ &  $\vdots$ &  $\vdots$ &  $\vdots$ &  $\vdots$ \\
 0.3 & \scriptsize{0.6179} & \scriptsize{0.6217} & \scriptsize{0.6255} & \scriptsize{0.6293} & \scriptsize{0.6331} & \scriptsize{0.6368} & \scriptsize{0.6406} & \scriptsize{0.6443} & \scriptsize{0.6480} & \scriptsize{0.6517} \\
\highlightT{0.4} & \scriptsize{0.6554} & \scriptsize{0.6591} & \scriptsize{0.6628} & \highlightT{\scriptsize{0.6664}} & \scriptsize{0.6700} & \scriptsize{0.6736} & \scriptsize{0.6772} & \scriptsize{0.6808} & \scriptsize{0.6844} & \scriptsize{0.6879} \\
 \hline
 0.5 & \scriptsize{0.6915} & \scriptsize{0.6950} & \scriptsize{0.6985} & \scriptsize{0.7019} & \scriptsize{0.7054} & \scriptsize{0.7088} & \scriptsize{0.7123} & \scriptsize{0.7157} & \scriptsize{0.7190} & \scriptsize{0.7224} \\
 0.6 & \scriptsize{0.7257} & \scriptsize{0.7291} & \scriptsize{0.7324} & \scriptsize{0.7357} & \scriptsize{0.7389} & \scriptsize{0.7422} & \scriptsize{0.7454} & \scriptsize{0.7486} & \scriptsize{0.7517} & \scriptsize{0.7549} \\
 0.7 & \scriptsize{0.7580} & \scriptsize{0.7611} & \scriptsize{0.7642} & \scriptsize{0.7673} & \scriptsize{0.7704} & \scriptsize{0.7734} & \scriptsize{0.7764} & \scriptsize{0.7794} & \scriptsize{0.7823} & \scriptsize{0.7852} \\
\highlightO{0.8} & \scriptsize{0.7881} & \scriptsize{0.7910} & \scriptsize{0.7939} & \scriptsize{0.7967} & \highlightO{\scriptsize{0.7995}} & \scriptsize{0.8023} & \scriptsize{0.8051} & \scriptsize{0.8078} & \scriptsize{0.8106} & \scriptsize{0.8133} \\
 0.9 & \scriptsize{0.8159} & \scriptsize{0.8186} & \scriptsize{0.8212} & \scriptsize{0.8238} & \scriptsize{0.8264} & \scriptsize{0.8289} & \scriptsize{0.8315} & \scriptsize{0.8340} & \scriptsize{0.8365} & \scriptsize{0.8389} \\
 \hline
 \hline
 1.0 & \scriptsize{0.8413} & \scriptsize{0.8438} & \scriptsize{0.8461} & \scriptsize{0.8485} & \scriptsize{0.8508} & \scriptsize{0.8531} & \scriptsize{0.8554} & \scriptsize{0.8577} & \scriptsize{0.8599} & \scriptsize{0.8621} \\
 1.1 & \scriptsize{0.8643} & \scriptsize{0.8665} & \scriptsize{0.8686} & \scriptsize{0.8708} & \scriptsize{0.8729} & \scriptsize{0.8749} & \scriptsize{0.8770} & \scriptsize{0.8790} & \scriptsize{0.8810} & \scriptsize{0.8830} \\
 $\vdots$ &  $\vdots$ &  $\vdots$ &  $\vdots$ &  $\vdots$ &  $\vdots$ &  $\vdots$ &  $\vdots$ &  $\vdots$ &  $\vdots$ &  $\vdots$ \\
  \hline
\end{tabular}
\caption{A section of the normal probability table. The percentile for a normal random variable with $Z=0.43$ has been \highlightT{highlighted}, and the percentile closest to 0.8000 has also been \highlightO{highlighted}.\textA{\vspace{-3mm}}}
\label{zTableShort}
\end{table}


|         |           |              |                     |    Second decimal place of $Z$   |                                  |                     |                     |                     |                     |                     |
|:----------------:|--------------------:|--------------------:|--------------------:|:--------------------------------:|---------------------------------:|--------------------:|--------------------:|--------------------:|--------------------:|--------------------:|
|        $Z$       |                0.00 |                0.01 |                0.02 |                \highlightT{0.03} |                \highlightO{0.04} |                0.05 |                0.06 |                0.07 |                0.08 |                0.09 |
|        0.0       | \scriptsize{0.5000} | \scriptsize{0.5040} | \scriptsize{0.5080} |              \scriptsize{0.5120} |              \scriptsize{0.5160} | \scriptsize{0.5199} | \scriptsize{0.5239} | \scriptsize{0.5279} | \scriptsize{0.5319} | \scriptsize{0.5359} |
|        0.1       | \scriptsize{0.5398} | \scriptsize{0.5438} | \scriptsize{0.5478} |              \scriptsize{0.5517} |              \scriptsize{0.5557} | \scriptsize{0.5596} | \scriptsize{0.5636} | \scriptsize{0.5675} | \scriptsize{0.5714} | \scriptsize{0.5753} |
|        0.2       | \scriptsize{0.5793} | \scriptsize{0.5832} | \scriptsize{0.5871} |              \scriptsize{0.5910} |              \scriptsize{0.5948} | \scriptsize{0.5987} | \scriptsize{0.6026} | \scriptsize{0.6064} | \scriptsize{0.6103} | \scriptsize{0.6141} |
|        0.3       | \scriptsize{0.6179} | \scriptsize{0.6217} | \scriptsize{0.6255} |              \scriptsize{0.6293} |              \scriptsize{0.6331} | \scriptsize{0.6368} | \scriptsize{0.6406} | \scriptsize{0.6443} | \scriptsize{0.6480} | \scriptsize{0.6517} |
| \highlightT{0.4} | \scriptsize{0.6554} | \scriptsize{0.6591} | \scriptsize{0.6628} | \highlightT{\scriptsize{0.6664}} |              \scriptsize{0.6700} | \scriptsize{0.6736} | \scriptsize{0.6772} | \scriptsize{0.6808} | \scriptsize{0.6844} | \scriptsize{0.6879} |
|        0.5       | \scriptsize{0.6915} | \scriptsize{0.6950} | \scriptsize{0.6985} |              \scriptsize{0.7019} |              \scriptsize{0.7054} | \scriptsize{0.7088} | \scriptsize{0.7123} | \scriptsize{0.7157} | \scriptsize{0.7190} | \scriptsize{0.7224} |
|        0.6       | \scriptsize{0.7257} | \scriptsize{0.7291} | \scriptsize{0.7324} |              \scriptsize{0.7357} |              \scriptsize{0.7389} | \scriptsize{0.7422} | \scriptsize{0.7454} | \scriptsize{0.7486} | \scriptsize{0.7517} | \scriptsize{0.7549} |
|        0.7       | \scriptsize{0.7580} | \scriptsize{0.7611} | \scriptsize{0.7642} |              \scriptsize{0.7673} |              \scriptsize{0.7704} | \scriptsize{0.7734} | \scriptsize{0.7764} | \scriptsize{0.7794} | \scriptsize{0.7823} | \scriptsize{0.7852} |
| \highlightO{0.8} | \scriptsize{0.7881} | \scriptsize{0.7910} | \scriptsize{0.7939} |              \scriptsize{0.7967} | \highlightO{\scriptsize{0.7995}} | \scriptsize{0.8023} | \scriptsize{0.8051} | \scriptsize{0.8078} | \scriptsize{0.8106} | \scriptsize{0.8133} |
|        0.9       | \scriptsize{0.8159} | \scriptsize{0.8186} | \scriptsize{0.8212} |              \scriptsize{0.8238} |              \scriptsize{0.8264} | \scriptsize{0.8289} | \scriptsize{0.8315} | \scriptsize{0.8340} | \scriptsize{0.8365} | \scriptsize{0.8389} |
|        1.0       | \scriptsize{0.8413} | \scriptsize{0.8438} | \scriptsize{0.8461} |              \scriptsize{0.8485} |              \scriptsize{0.8508} | \scriptsize{0.8531} | \scriptsize{0.8554} | \scriptsize{0.8577} | \scriptsize{0.8599} | \scriptsize{0.8621} |
|        1.1       | \scriptsize{0.8643} | \scriptsize{0.8665} | \scriptsize{0.8686} |              \scriptsize{0.8708} |              \scriptsize{0.8729} | \scriptsize{0.8749} | \scriptsize{0.8770} | \scriptsize{0.8790} | \scriptsize{0.8810} | \scriptsize{0.8830} |
|     $\vdots$     |            $\vdots$ |            $\vdots$ |            $\vdots$ |                         $\vdots$ |                         $\vdots$ |            $\vdots$ |            $\vdots$ |            $\vdots$ |            $\vdots$ |            $\vdots$ |

-->


We can also find the Z score associated with a percentile. 
For example, to identify Z for the $80^{th}$ percentile, we use `qnorm()` which identifies the **quantile** for a given percentage.  The quantile represents the cutoff value.  (To remember the function `qnorm()` as providing a cutoff, notice that both `qnorm()` and "cutoff" start with the sound "kuh".  To remember the `pnorm()` function as providing a probability from a given cutoff, notice that both `pnorm()` and probability start with the sound "puh".) 
We determine the Z score for the $80^{th}$ percentile using `qnorm()`: 0.84.


```r
qnorm(0.80, m = 0, s = 1)
#> [1] 0.842
openintro::normTail(0.84162, m = 0, s = 1)
```

<img src="05-inference-foundations_files/figure-html/unnamed-chunk-73-1.png" width="70%" style="display: block; margin: auto;" />


\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">Determine the proportion of SAT test takers who scored better than Ann on the SAT.^[If 84% had lower scores than Ann, the number of people who had better scores must be 16%. (Generally ties are ignored when the normal model, or any other continuous distribution, is used.)]</div>\EndKnitrBlock{guidedpractice}


#### Normal probability examples {-}

Cumulative SAT scores are approximated well by a normal model, $N(\mu=1500, \sigma=300)$.

\BeginKnitrBlock{example}<div class="example">Shannon is a randomly selected SAT taker, and nothing is known about Shannon's SAT aptitude. What is the probability that Shannon scores at least 1630 on her SATs?

---

First, always draw and label a picture of the normal distribution. (Drawings need not be exact to be useful.) We are interested in the chance she scores above 1630, so we shade the upper tail.  See the normal curve below.


The picture shows the mean and the values at 2 standard deviations above and below the mean. The simplest way to find the shaded area under the curve makes use of the Z score of the cutoff value. With $\mu=1500$, $\sigma=300$, and the cutoff value $x=1630$, the Z score is computed as
\begin{eqnarray*}
Z = \frac{x - \mu}{\sigma} = \frac{1630 - 1500}{300} = \frac{130}{300} = 0.43
\end{eqnarray*}
We use software to find the percentile of $Z=0.43$, which yields 0.6664. However, the percentile describes those who had a Z score *lower* than 0.43. To find the area *above* $Z=0.43$, we compute one minus the area of the lower tail, as seen below.

The probability Shannon scores at least 1630 on the SAT is 0.3336.</div>\EndKnitrBlock{example}

<img src="05-inference-foundations_files/figure-html/satAbove1630-1.png" width="70%" style="display: block; margin: auto;" />


<img src="05-inference-foundations_files/figure-html/subtractingArea-1.png" width="70%" style="display: block; margin: auto;" />


\BeginKnitrBlock{tipbox}<div class="tipbox">**always draw a picture first, and find the Z score second.**

For any normal probability situation, *always always always* draw and label the normal curve and shade the area of interest first. 
The picture will provide an estimate of the probability.

After drawing a figure to represent the situation, identify the Z score for the observation of interest.</div>\EndKnitrBlock{tipbox}


\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">If the probability of Shannon scoring at least 1630 is 0.3336, then what is the probability she scores less than 1630? Draw the normal curve representing this exercise, shading the lower region instead of the upper one.^[We found the probability to be 0.6664. A picture for this exercise is represented by the shaded area below "0.6664".]</div>\EndKnitrBlock{guidedpractice}

\BeginKnitrBlock{todo}<div class="todo">the example and guided practice below both have figures in them.  How do we put figures in footnotes?</div>\EndKnitrBlock{todo}

\BeginKnitrBlock{example}<div class="example">Edward earned a 1400 on his SAT. What is his percentile?

---
 
First, a picture is needed. Edward's percentile is the proportion of people who do not get as high as a 1400. These are the scores to the left of 1400.

Identifying the mean $\mu=1500$, the standard deviation $\sigma=300$, and the cutoff for the tail area $x=1400$ makes it easy to compute the Z score:
\begin{eqnarray*}
Z = \frac{x - \mu}{\sigma} = \frac{1400 - 1500}{300} = -0.33
\end{eqnarray*}
Using the normal probability table, identify the row of $-0.3$ and column of $0.03$, which corresponds to the probability $0.3707$. Edward is at the $37^{th}$ percentile.</div>\EndKnitrBlock{example}


<img src="05-inference-foundations_files/figure-html/satBelow1400-1.png" width="70%" style="display: block; margin: auto;" />


\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">Use the results of the previous example to compute the proportion of SAT takers who did better than Edward. Also draw a new picture.^[If Edward did better than 37% of SAT takers, then about 63% must have done better than him. 
\includegraphics[height=14mm]{02/figures/satBelow1400/satAbove1400}]</div>\EndKnitrBlock{guidedpractice}


\BeginKnitrBlock{tipbox}<div class="tipbox">**areas to the right.**

The normal probability table in most books gives the area to the left. If you would like the area to the right, first find the area to the left and then subtract this amount from one.</div>\EndKnitrBlock{tipbox}

\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">Stuart earned an SAT score of 2100. Draw a picture for each part. (a) What is his percentile? (b) What percent of SAT takers did better than Stuart?^[Numerical answers: (a) 0.9772. (b) 0.0228.]</div>\EndKnitrBlock{guidedpractice}

Based on a sample of 100 men,^[This sample was taken from the USDA Food Commodity Intake Database.] the heights of male adults between the ages 20 and 62 in the US is nearly normal with mean 70.0'' and standard deviation 3.3''.

\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">Mike is 5'7'' and Jim is 6'4''. (a) What is Mike's height percentile? (b) What is Jim's height percentile? Also draw one picture for each part.^[First put the heights into inches: 67 and 76 inches. Figures are shown below. (a) $Z_{Mike} = \frac{67 - 70}{3.3} = -0.91\ \to\ 0.1814$. (b) $Z_{Jim} = \frac{76 - 70}{3.3} = 1.82\ \to\ 0.9656$. \\\includegraphics[height=14mm]{02/figures/mikeAndJimPercentiles/mikeAndJimPercentiles}]</div>\EndKnitrBlock{guidedpractice}

The last several problems have focused on finding the probability or percentile for a particular observation. 
What if you would like to know the observation corresponding to a particular percentile?


\BeginKnitrBlock{example}<div class="example">Erik's height is at the $40^{th}$ percentile. How tall is he?

---

As always, first draw the picture (see below).

In this case, the lower tail probability is known (0.40), which can be shaded on the diagram. We want to find the observation that corresponds to this value. As a first step in this direction, we determine the Z score associated with the $40^{th}$ percentile.

Because the percentile is below 50%, we know $Z$ will be negative. Looking in the negative part of the normal probability table, we search for the probability *inside* the table closest to 0.4000. We find that 0.4000 falls in row $-0.2$ and between columns $0.05$ and $0.06$. Since it falls closer to $0.05$, we take this one: $Z=-0.25$.

Knowing $Z_{Erik}=-0.25$ and the population parameters $\mu=70$ and $\sigma=3.3$ inches, the Z score formula can be set up to determine Erik's unknown height, labeled $x_{Erik}$:
\begin{eqnarray*}
-0.25 = Z_{Erik} = \frac{x_{Erik} - \mu}{\sigma} = \frac{x_{Erik} - 70}{3.3}
\end{eqnarray*}
Solving for $x_{Erik}$ yields the height 69.18 inches. That is, Erik is about 5'9'' (this is notation for 5-feet, 9-inches).</div>\EndKnitrBlock{example}


```r
qnorm(0.4, m = 0, s = 1)
#> [1] -0.253
```

<img src="05-inference-foundations_files/figure-html/height40Perc-1.png" width="70%" style="display: block; margin: auto;" />


\BeginKnitrBlock{example}<div class="example">What is the adult male height at the $82^{nd}$ percentile?
 
---
 
Again, we draw the figure first (see below).

Next, we want to find the Z score at the $82^{nd}$ percentile, which will be a positive value. Using `qnorm()`, the $82^{nd}$ percentile corresponds to $Z=0.92$. Finally, the height $x$ is found using the Z score formula with the known mean $\mu$, standard deviation $\sigma$, and Z score $Z=0.92$:
\begin{eqnarray*}
0.92 = Z = \frac{x-\mu}{\sigma} = \frac{x - 70}{3.3}
\end{eqnarray*}
This yields 73.04 inches or about 6'1'' as the height at the $82^{nd}$ percentile.</div>\EndKnitrBlock{example}


```r
qnorm(0.82, m = 0, s = 1)
#> [1] 0.915
```

<img src="05-inference-foundations_files/figure-html/height82Perc-1.png" width="70%" style="display: block; margin: auto;" />


\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">(a) What is the $95^{th}$ percentile for SAT scores?   
(b) What is the $97.5^{th}$ percentile of the male heights? As always with normal probability problems, first draw a picture.^[Remember: draw a picture first, then find the Z score. (We leave the pictures to you.) The Z score can be found by using the percentiles and the normal probability table. (a) We look for 0.95 in the probability portion (middle part) of the normal probability table, which leads us to row 1.6 and (about) column 0.05, i.e., $Z_{95}=1.65$. Knowing $Z_{95}=1.65$, $\mu = 1500$, and $\sigma = 300$, we setup the Z score formula: $1.65 = \frac{x_{95} - 1500}{300}$. We solve for $x_{95}$: $x_{95} = 1995$. (b) Similarly, we find $Z_{97.5} = 1.96$, again setup the Z score formula for the heights, and calculate $x_{97.5} = 76.5$.]</div>\EndKnitrBlock{guidedpractice}

\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">(a) What is the probability that a randomly selected male adult is at least 6'2'' (74 inches)?  
(b) What is the probability that a male adult is shorter than 5'9'' (69 inches)?^[Numerical answers: (a) 0.1131. (b) 0.3821.]</div>\EndKnitrBlock{guidedpractice}

\BeginKnitrBlock{example}<div class="example">What is the probability that a random adult male is between 5'9'' and 6'2''?
 
---
 
These heights correspond to 69 inches and 74 inches. First, draw the figure. The area of interest is no longer an upper or lower tail.  


The total area under the curve is 1. If we find the area of the two tails that are not shaded (from the previous Guided Practice, these areas are $0.3821$ and $0.1131$), then we can find the middle area:
  
That is, the probability of being between 5'9'' and 6'2'' is 0.5048.</div>\EndKnitrBlock{example}


<img src="05-inference-foundations_files/figure-html/unnamed-chunk-91-1.png" width="70%" style="display: block; margin: auto;" />



<img src="05-inference-foundations_files/figure-html/unnamed-chunk-92-1.png" width="70%" style="display: block; margin: auto;" />




\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">What percent of SAT takers get between 1500 and 2000?^[This is an abbreviated solution. (Be sure to draw a figure!) First find the percent who get below 1500 and the percent that get above 2000: $Z_{1500} = 0.00 \to 0.5000$ (area below), $Z_{2000} = 1.67 \to 0.0475$ (area above). Final answer: $1.0000-0.5000 - 0.0475 = 0.4525$.]</div>\EndKnitrBlock{guidedpractice}

\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">What percent of adult males are between 5'5'' and 5'7''?^[5'5'' is 65 inches. 5'7'' is 67 inches. Numerical solution: $1.000 - 0.0649 - 0.8183 = 0.1168$, i.e., 11.68%.]</div>\EndKnitrBlock{guidedpractice}

#### 68-95-99.7 rule {-}

Here, we present a useful general rule for the probability of falling within 1, 2, and 3 standard deviations of the mean in the normal distribution. The rule will be useful in a wide range of practical settings, especially when trying to make a quick estimate without a calculator or Z table.


<div class="figure" style="text-align: center">
<img src="05-inference-foundations_files/figure-html/er6895997-1.png" alt="Probabilities for falling within 1, 2, and 3 standard deviations of the mean in a normal distribution." width="70%" />
<p class="caption">(\#fig:er6895997)Probabilities for falling within 1, 2, and 3 standard deviations of the mean in a normal distribution.</p>
</div>



\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">Use `pnorm()` (or a  Z table) to confirm that about 68%, 95%, and 99.7% of observations fall within 1, 2, and 3, standard deviations of the mean in the normal distribution, respectively. For instance, first find the area that falls between $Z=-1$ and $Z=1$, which should have an area of about 0.68. Similarly there should be an area of about 0.95 between $Z=-2$ and $Z=2$.^[First draw the pictures. To find the area between $Z=-1$ and $Z=1$, use `pnorm()` or the normal probability table to determine the areas below $Z=-1$ and above $Z=1$. Next verify the area between $Z=-1$ and $Z=1$ is about 0.68. Repeat this for $Z=-2$ to $Z=2$ and also for $Z=-3$ to $Z=3$.]</div>\EndKnitrBlock{guidedpractice}

It is possible for a normal random variable to fall 4, 5, or even more standard deviations from the mean. However, these occurrences are very rare if the data are nearly normal. The probability of being further than 4 standard deviations from the mean is about 1-in-30,000. For 5 and 6 standard deviations, it is about 1-in-3.5 million and 1-in-1 billion, respectively.

\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">SAT scores closely follow the normal model with mean $\mu = 1500$ and standard deviation $\sigma = 300$.   
(a) About what percent of test takers score 900 to 2100?  
(b) What percent score between 1500 and 2100?^[(a) 900 and 2100 represent two standard deviations above and below the mean, which means about 95% of test takers will score between 900 and 2100. (b) Since the normal model is symmetric, then half of the test takers from part (a) ($\frac{95\%}{2} = 47.5\%$ of all test takers) will score 900 to 1500 while 47.5% score between 1500 and 2100.]</div>\EndKnitrBlock{guidedpractice}

<!--
#### Evaluating the normal approximation {-}

Many processes can be well approximated by the normal distribution. We have already seen two good examples: SAT scores and the heights of US adult males. While using a normal model can be extremely convenient and helpful, it is important to remember normality is always an approximation. Testing the appropriateness of the normal assumption is a key step in many data analyses.

A previous example suggests the distribution of heights of US males is well approximated by the normal model. We are interested in proceeding under the assumption that the data are normally distributed, but first we must check to see if this is reasonable.

There are two visual methods for checking the assumption of normality, which can be implemented and interpreted quickly. The first is a simple histogram with the best fitting normal curve overlaid on the plot, as shown in the left panel of Figure \@ref(fig:fcidMHeights). The sample mean $\bar{x}$ and standard deviation $s$ are used as the parameters of the best fitting normal curve. The closer this curve fits the histogram, the more reasonable the normal model assumption. Another more common method is examining a **normal probability plot**.^[Also commonly called a **quantile-quantile plot**.], shown in the right panel of Figure \@ref(fig:fcidMHeights). The closer the points are to a perfect straight line, the more confident we can be that the data follow the normal model.




<div class="figure" style="text-align: center">
<img src="05-inference-foundations_files/figure-html/fcidMHeights-1.png" alt="A sample of 100 male heights. The observations are rounded to the nearest whole inch, explaining why the points appear to jump in increments in the normal probability plot." width="70%" /><img src="05-inference-foundations_files/figure-html/fcidMHeights-2.png" alt="A sample of 100 male heights. The observations are rounded to the nearest whole inch, explaining why the points appear to jump in increments in the normal probability plot." width="70%" />
<p class="caption">(\#fig:fcidMHeights)A sample of 100 male heights. The observations are rounded to the nearest whole inch, explaining why the points appear to jump in increments in the normal probability plot.</p>
</div>



\BeginKnitrBlock{example}<div class="example">Three data sets of 40, 100, and 400 samples were simulated from a normal distribution, and the histograms and normal probability plots of the data sets are shown in Figure \@ref(fig:normalExamples). These will provide a benchmark for what to look for in plots of real data. 

---

The left panels show the histogram (top) and normal probability plot (bottom) for the simulated data set with 40 observations. The data set is too small to really see clear structure in the histogram. The normal probability plot also reflects this, where there are some deviations from the line. However, these deviations are not strong.

The middle panels show diagnostic plots for the data set with 100 simulated observations. The histogram shows more normality and the normal probability plot shows a better fit. While there is one observation that deviates noticeably from the line, it is not particularly extreme.

The data set with 400 observations has a histogram that greatly resembles the normal distribution, while the normal probability plot is nearly a perfect straight line. Again in the normal probability plot there is one observation (the largest) that deviates slightly from the line. If that observation had deviated 3 times further from the line, it would be of much greater concern in a real data set. Apparent outliers can occur in normally distributed data but they are rare.

Notice the histograms look more normal as the sample size increases, and the normal probability plot becomes straighter and more stable.</div>\EndKnitrBlock{example}


<div class="figure" style="text-align: center">
<img src="05-inference-foundations_files/figure-html/normalExamples-1.png" alt="Histograms and normal probability plots for three simulated normal data sets; $n=40$ (left), $n=100$ (middle), $n=400$ (right)." width="70%" />
<p class="caption">(\#fig:normalExamples)Histograms and normal probability plots for three simulated normal data sets; $n=40$ (left), $n=100$ (middle), $n=400$ (right).</p>
</div>


\BeginKnitrBlock{example}<div class="example">Are NBA player heights normally distributed? Consider all 435 NBA players from the 2008-9 season presented in Figure \@ref(fig:nbaNormal).^[These data were collected from http://www.nba.com.]

---
 
We first create a histogram and normal probability plot of the NBA player heights. The histogram in the left panel is slightly left skewed, which contrasts with the symmetric normal distribution. The points in the normal probability plot do not appear to closely follow a straight line but show what appears to be a "wave". We can compare these characteristics to the sample of 400 normally distributed observations in the previous example and see that they represent much stronger deviations from the normal model. NBA player heights do not appear to come from a normal distribution.</div>\EndKnitrBlock{example}


<div class="figure" style="text-align: center">
<img src="05-inference-foundations_files/figure-html/nbaNormal-1.png" alt="Histogram and normal probability plot for the NBA heights from the 2008-9 season." width="70%" /><img src="05-inference-foundations_files/figure-html/nbaNormal-2.png" alt="Histogram and normal probability plot for the NBA heights from the 2008-9 season." width="70%" />
<p class="caption">(\#fig:nbaNormal)Histogram and normal probability plot for the NBA heights from the 2008-9 season.</p>
</div>



\BeginKnitrBlock{example}<div class="example">Can we approximate poker winnings by a normal distribution? We consider the poker winnings of an individual over 50 days. A histogram and normal probability plot of these data are shown in Figure \@ref(fig:pokerNormal).

---
 
The data are very strongly **right skewed** in the histogram, which corresponds to the very strong deviations on the upper right component of the normal probability plot. If we compare these results to the sample of 40 normal observations in the previous example, it is apparent that these data show very strong deviations from the normal model.</div>\EndKnitrBlock{example}




<div class="figure" style="text-align: center">
<img src="05-inference-foundations_files/figure-html/pokerNormal-1.png" alt="A histogram of poker data with the best fitting normal plot and a normal probability plot." width="70%" /><img src="05-inference-foundations_files/figure-html/pokerNormal-2.png" alt="A histogram of poker data with the best fitting normal plot and a normal probability plot." width="70%" />
<p class="caption">(\#fig:pokerNormal)A histogram of poker data with the best fitting normal plot and a normal probability plot.</p>
</div>


\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">Determine which data sets represented in Figure \@ref(fig:normalQuantileExer) plausibly come from a nearly normal distribution. Are you confident in all of your conclusions? There are 100 (top left), 50 (top right), 500 (bottom left), and 15 points (bottom right) in the four plots.^[Answers may vary a little. The top-left plot shows some deviations in the smallest values in the data set; specifically, the left tail of the data set has some outliers we should be wary of. The top-right and bottom-left plots do not show any obvious or extreme deviations from the lines for their respective sample sizes, so a normal model would be reasonable for these data sets. The bottom-right plot has a consistent curvature that suggests it is not from the normal distribution. If we examine just the vertical coordinates of these observations, we see that there is a lot of data between -20 and 0, and then about five observations scattered between 0 and 70. This describes a distribution that has a strong right skew.]</div>\EndKnitrBlock{guidedpractice}


<div class="figure" style="text-align: center">
<img src="05-inference-foundations_files/figure-html/normalQuantileExer-1.png" alt="Four normal probability plots for the Guided Practice." width="70%" /><img src="05-inference-foundations_files/figure-html/normalQuantileExer-2.png" alt="Four normal probability plots for the Guided Practice." width="70%" /><img src="05-inference-foundations_files/figure-html/normalQuantileExer-3.png" alt="Four normal probability plots for the Guided Practice." width="70%" /><img src="05-inference-foundations_files/figure-html/normalQuantileExer-4.png" alt="Four normal probability plots for the Guided Practice." width="70%" />
<p class="caption">(\#fig:normalQuantileExer)Four normal probability plots for the Guided Practice.</p>
</div>




\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">Figure \@ref(fig:normalQuantileExerAdditional) shows normal probability plots for two distributions that are skewed. One distribution is skewed to the low end (left skewed) and the other to the high end (right skewed). Which is which?^[Examine where the points fall along the vertical axis. In the first plot, most points are near the low end with fewer observations scattered along the high end; this describes a distribution that is skewed to the high end. The second plot shows the opposite features, and this distribution is skewed to the low end.]</div>\EndKnitrBlock{guidedpractice}

\index{normal distribution|)}


\BeginKnitrBlock{todo}<div class="todo">Normal probability plots for Guided Practice \ref{normalQuantileExerciseAdditional}.</div>\EndKnitrBlock{todo}

<div class="figure" style="text-align: center">
<img src="05-inference-foundations_files/figure-html/normalQuantileExerAdditional-1.png" alt="Normal probability plots for the additional Guided Practice." width="70%" /><img src="05-inference-foundations_files/figure-html/normalQuantileExerAdditional-2.png" alt="Normal probability plots for the additional Guided Practice." width="70%" />
<p class="caption">(\#fig:normalQuantileExerAdditional)Normal probability plots for the additional Guided Practice.</p>
</div>

-->

###  Hypothesis testing case studies {#ApplyingTheNormalModel}

The approach for using the normal model in the context of inference is very similar to the practice of applying the model to individual observations that are nearly normal. We will replace null distributions we previously obtained using the randomization or simulation techniques and verify the results once again using the normal model. When the sample size is sufficiently large, the normal approximation generally provides us with the same conclusions as the simulation model.

#### Standard error {-}

Point estimates vary from sample to sample, and we quantify this variability with what is called the **standard error (SE)**\index{standard error}. The standard error is equal to the standard deviation associated with the estimate. So, for example, if we used the standard deviation to quantify the variability of a point estimate from one sample to the next, this standard deviation would be called the standard error of the point estimate.



<!--
% The large the sample size, the smaller our standard error a statistic like the sample proportion or sample mean will b smaller. This is consistent with intuition: the more data we have, the more accurate an estimate will tend to be, i.e., the standard error of the estimate becomes smaller when we have more data.
-->


The way we determine the standard error varies from one situation to the next. However, typically it is determined using a formula based on the Central Limit Theorem.


#### Opportunity cost {-}


##### Observed data {-}

In Section \@ref(caseStudyOpportunityCost) we were introduced to the opportunity cost study, which found that students became thriftier when they were reminded that not spending money now means the money can be spent on other things in the future. Let's re-analyze the data in the context of the normal distribution and compare the results.

##### Variability of the statistic {-}
Figure \@ref(fig:OpportunityCostDiffs-w-normal) summarizes the null distribution as determined using the randomization method. The best fitting normal distribution for the null distribution has a mean of 0. We can calculate the standard error of this distribution by borrowing a formula that we will become familiar with in Section \@ref(diff-two-prop), but for now let's just take the value $SE = 0.078$ as a given. Recall that the point estimate of the difference was 0.20, as shown in the plot. Next, we'll use the normal distribution approach to compute the two-tailed p-value.


<div class="figure" style="text-align: center">
<img src="05-inference-foundations_files/figure-html/OpportunityCostDiffs-w-normal-1.png" alt="Null distribution of differences with an overlaid normal curve for the opportunity cost study. 10,000 simulations were run for this figure." width="70%" />
<p class="caption">(\#fig:OpportunityCostDiffs-w-normal)Null distribution of differences with an overlaid normal curve for the opportunity cost study. 10,000 simulations were run for this figure.</p>
</div>



##### Observed statistic vs. null statistics {-}

As we learned in Section \@ref(normalDist), it is helpful to draw and shade a picture of the normal distribution so we know precisely what we want to calculate. Here we want to find the area of the tail beyond 0.2, representing the p-value.


<img src="05-inference-foundations_files/figure-html/OpportunityCostDiffs_normal_only-1.png" width="70%" style="display: block; margin: auto;" />


Next, we can calculate the Z score using the observed difference, 0.20, and the two model parameters. The standard error, $SE = 0.078$, is the equivalent of the model's standard deviation.
\begin{align*}
Z = \frac{\text{observed difference} - 0}{SE} = \frac{0.20 - 0}{0.078} = 2.56
\end{align*}
We can either use statistical software or look up $Z = 2.56$ in the normal probability table to determine the right tail area: 0.0052, which is about the same as what we got for the right tail using the randomization approach (0.006). Using this area as the p-value, we see that the p-value is less than 0.05, we conclude that the treatment did indeed impact students' spending.


\BeginKnitrBlock{onebox}<div class="onebox">**Z score in a hypothesis test.**

In the context of a hypothesis test, the Z score for a point estimate is
\begin{align*}
Z = \frac{\text{point estimate} - \text{null value}}{SE}
\end{align*}
The standard error in this case is the equivalent of the standard deviation of the point estimate, and the null value comes from the null hypothesis.</div>\EndKnitrBlock{onebox}

We have confirmed that the randomization approach we used earlier and the normal distribution approach provide almost identical p-values and conclusions in the opportunity cost case study. Next, let's turn our attention to the medical consultant case study.


#### Medical consultant {-}

##### Observed data {-}

In Section \@ref(sec-med-consult) we learned about a medical consultant who reported that only 3 of her 62 clients who underwent a liver transplant had complications, which is less than the more common complication rate of 0.10. 
In that work, we did not model a null scenario, but we will discuss a simulation method for a one proportion null distribution in Section \@ref(one-prop-null-boot), such a distribution is provided in Figure \@ref(fig:MedConsNullSim-w-normal). 
We have added the best-fitting normal curve to the figure, which has a mean of 0.10. Borrowing a formula that we'll encounter in Chapter \@ref(inference-cat), the standard error of this distribution was also computed: $SE = 0.038$.

<!--
%\begin{align*}
%SE = \sqrt{\frac{p_0 (1 - p_0)}{n}} = \sqrt{\frac{0.1 (1 - 0.1)}{62}} = 0.038
%\end{align*}
%where $p_0$ is the null value for the hypothesis test and $n$ is the sample size. (We'll discuss this formula in greater detail in next chapter.) 

In the previous analysis, we obtained a p-value of 0.2444, and we will try to reproduce that p-value using the normal distribution approach. 
-->

##### Variability of the statistic {-}

Before we begin, we want to point out a simple detail that is easy to overlook: the null distribution we generated from the simulation is slightly skewed, and the histogram is not particularly smooth. In fact, the normal distribution only sort-of fits this model. 


<div class="figure" style="text-align: center">
<img src="05-inference-foundations_files/figure-html/MedConsNullSim-w-normal-1.png" alt="The null distribution for the sample proportion, created from 10,000 simulated studies, along with the best-fitting normal model." width="70%" />
<p class="caption">(\#fig:MedConsNullSim-w-normal)The null distribution for the sample proportion, created from 10,000 simulated studies, along with the best-fitting normal model.</p>
</div>

##### Observed statistic vs. null statistics {-}

As always, we'll draw a picture before finding the normal probabilities. Below is a normal distribution centered at 0.10 with a standard error of 0.038.

<img src="05-inference-foundations_files/figure-html/MedConsNullSim-normal-only-1.png" width="70%" style="display: block; margin: auto;" />


Next, we can calculate the Z score using the observed complication rate, $\hat{p} = 0.048$ along with the mean and standard deviation of the normal model. Here again, we use the standard error for the standard deviation.
\begin{align*}
Z = \frac{\hat{p} - p_0}{SE_{\hat{p}}} = \frac{0.048 - 0.10}{0.038} = -1.37
\end{align*}
Identifying $Z = -1.37$ using statistical software or in the normal probability table, we can determine that the left tail area is 0.0853 which is the estimated p-value for the hypothesis test. There is a small problem:  the p-value of 0.0853 is slightly different from the simulation p-value or 0.1222 which will be calculated in Section \@ref(one-prop-null-boot).

The discrepancy is explained by normal model's poor representation of the null distribution in Figure \@ref(fig:MedConsNullSim-w-normal). 
As noted earlier, the null distribution from the simulations is not very smooth, and the distribution itself is slightly skewed. 
That's the bad news. The good news is that we can foresee these problems using some simple checks. We'll learn about these checks in the following chapters.

In Section \@ref(CLTsection) we noted that the two common requirements to apply the Central Limit Theorem are (1) the observations in the sample must be independent, and (2) the sample must be sufficiently large. 
The guidelines for this particular situation -- which we will learn in Section \@ref(single-prop) -- would have alerted us that the normal model was a poor approximation.


#### Conditions for applying the normal model {-}

The success story in this section was the application of the normal model in the context of the opportunity cost data. However, the biggest lesson comes from our failed attempt to use the normal approximation in the medical consultant case study.

Statistical techniques are like a carpenter's tools. When used responsibly, they can produce amazing and precise results. However, if the tools are applied irresponsibly or under inappropriate conditions, they will produce unreliable results. For this reason, with every statistical method that we introduce in future chapters, we will carefully outline conditions when the method can reasonably be used. These conditions should be checked in each application of the technique. 

<!--
% CONSIDER: adding a note about advanced methods here. (Term box?)
-->

### Confidence interval case study


A point estimate is our best guess for the value of the parameter, so it makes sense to build the confidence interval around that value. The standard error, which is a measure of the uncertainty associated with the point estimate, provides a guide for how large we should make the confidence interval.  The 68-95-99.7 rule tells us that, in general, 95% of observations are within 2 standard errors of the mean.  Here, we use the value 1.96 to be slightly more precise.

\BeginKnitrBlock{onebox}<div class="onebox">**Constructing a 95% confidence interval.**

When the sampling distribution of a point estimate can reasonably be modeled as normal, the point estimate we observe will be within 1.96 standard errors of the true value of interest about 95% of the time. Thus, a **95% confidence interval** for such a point estimate can be constructed:

\begin{align}
\text{point estimate}\ \pm\ 1.96 \times SE
\label{95PercentConfidenceIntervalFormula}
\end{align}
We can be **95% confident** this interval captures the true value.</div>\EndKnitrBlock{onebox}

\index{95% confidence interval}



\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">Compute the area between -1.96 and 1.96 for a normal distribution with mean 0 and standard deviation 1.^[We will leave it to you to draw a picture. The Z scores are $Z_{left} = -1.96$ and $Z_{right} = 1.96$. The area between these two Z scores is $0.9750 - 0.0250 = 0.9500$. This is where "1.96" comes from in the 95% confidence interval formula.]</div>\EndKnitrBlock{guidedpractice}

\BeginKnitrBlock{example}<div class="example">The point estimate from the opportunity cost study was that 20% fewer students would buy a DVD if they were reminded that money not spent now could be spent later on something else. The point estimate from this study can reasonably be modeled with a normal distribution, and a proper standard error for this point estimate is $SE = 0.078$. Construct a 95% confidence interval.^[We've used $SE = 0.078$ from the last section. However, it would more generally be appropriate to recompute the $SE$ slightly differently for this confidence interval using the technique introduced in Section \@ref(diff-two-prop). Don't worry about this detail for now since the two resulting standard errors are, in this case, almost identical.]

---
 
Since the conditions for the normal approximation have already been verified, we can move forward with the construction of the 95% confidence interval:
\begin{align*}
\text{point estimate}\ \pm\ 1.96 \times SE \quad \rightarrow \quad
0.20\ \pm\ 1.96 \times 0.078 \quad \rightarrow \quad
(0.047, 0.353)
\end{align*}
We are 95% confident that the DVD purchase rate resulting from the treatment is between 4.7% and 35.3% lower than in the control group. Since this confidence interval does not contain 0, it is consistent with our earlier result where we rejected the notion of "no difference" using a hypothesis test.</div>\EndKnitrBlock{example}

<!--
% Using the old SE intentionally to keep things consistent / simple.
% Correct SE: p1 <- 56 / 75; p2 <- 41 / 75; sqrt(p1 * (1 - p1) / 75 + p2 * (1 - p2) / 75)
-->

#### Stents {-}

##### Observed data {-}

Consider an experiment that examined whether implanting a stent in the brain of a patient at risk for a stroke helps reduce the risk of a stroke. The results from the first 30 days of this study, which included 451 patients, are summarized in Table \@ref(tab:stentStudyResultsCIsection). These results are surprising! The point estimate suggests that patients who received stents may have a *higher* risk of stroke: $p_{trmt} - p_{ctrl} = 0.090$.


<table class="table" style="margin-left: auto; margin-right: auto;">
<caption>(\#tab:stentStudyResultsCIsection)Descriptive statistics for 30-day results for the stent study.</caption>
 <thead>
  <tr>
   <th style="text-align:left;">  </th>
   <th style="text-align:left;"> stroke </th>
   <th style="text-align:left;"> no event </th>
   <th style="text-align:left;"> Total </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> treatment </td>
   <td style="text-align:left;"> 33 </td>
   <td style="text-align:left;"> 191 </td>
   <td style="text-align:left;"> 224 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> control </td>
   <td style="text-align:left;"> 13 </td>
   <td style="text-align:left;"> 214 </td>
   <td style="text-align:left;"> 227 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Total </td>
   <td style="text-align:left;"> 46 </td>
   <td style="text-align:left;"> 405 </td>
   <td style="text-align:left;"> 451 </td>
  </tr>
</tbody>
</table>

##### Variability of the statistic {-}

\BeginKnitrBlock{example}<div class="example">Consider the stent study and results. The conditions necessary to ensure the point estimate $p_{trmt} - p_{ctrl} = 0.090$ is nearly normal have been verified for you, and the estimate's standard error is $SE = 0.028$. Construct a 95% confidence interval for the change in 30-day stroke rates from usage of the stent.

---

The conditions for applying the normal model have already been verified, so we can proceed to the construction of the confidence interval:
\begin{align*}
\text{point estimate}\ \pm\ 1.96 \times SE \quad \rightarrow \quad
0.090\ \pm\ 1.96 \times 0.028 \quad \rightarrow \quad
(0.035, 0.145)
\end{align*}
We are 95% confident that implanting a stent in a stroke patient's brain increased the risk of stroke within 30 days by a rate of 0.035 to 0.145. This confidence interval can also be used in a way analogous to a hypothesis test: since the interval does not contain 0, it means the data provide statistically significant evidence that the stent used in the study *increases* the risk of </div>\EndKnitrBlock{example}

As with hypothesis tests, confidence intervals are imperfect. About 1-in-20 properly constructed 95% confidence intervals will fail to capture the parameter of interest, simply due to natural variability in the observed data. Figure \@ref(fig:95PercentConfidenceInterval) shows 25 confidence intervals for a proportion that were constructed from simulations where the true proportion was $p = 0.3$. However, 1 of these 25 confidence intervals happened not to include the true value.  The interval which does not capture $p=0.3$ is not due to bad science.  Instead, it is due to natural variability, and we should *expect* some of our intervals to miss the parameter of interest.  Indeed, over a lifetime of creating 95% intervals, you should expect 5% of your reported intervals to miss the parameter of interest (unfortunately, you will not ever know which of your reported intervals captured the parameter and which missed the parameter).


<div class="figure" style="text-align: center">
<img src="05-inference-foundations_files/figure-html/95PercentConfidenceInterval-1.png" alt="Twenty-five samples of size $n=300$ were simulated when $p = 0.30$. For each sample, a confidence interval was created to try to capture the true proportion $p$. However, 1 of these 25 intervals did not capture $p = 0.30$." width="70%" />
<p class="caption">(\#fig:95PercentConfidenceInterval)Twenty-five samples of size $n=300$ were simulated when $p = 0.30$. For each sample, a confidence interval was created to try to capture the true proportion $p$. However, 1 of these 25 intervals did not capture $p = 0.30$.</p>
</div>

\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">In Figure \@ref(fig:95PercentConfidenceInterval), one interval does not contain the true proportion, $p = 0.3$. Does this imply that there was a problem with the simulations run?^[No. Just as some observations occur more than 1.96 standard deviations from the mean, some point estimates will be more than 1.96 standard errors from the parameter. A confidence interval only provides a plausible range of values for a parameter. While we might say other values are implausible based on the data, this does not mean they are impossible.]</div>\EndKnitrBlock{guidedpractice}



#### Interpreting confidence intervals {-}

\index{confidence interval!interpretation|(}

A careful eye might have observed the somewhat awkward language used to describe confidence intervals. Correct interpretation:

> We are XX% confident that the population parameter is between...

*Incorrect* language might try to describe the confidence interval as capturing the population parameter with a certain probability. This is one of the most common errors: while it might be useful to think of it as a probability, the confidence level only quantifies how plausible it is that the parameter is in the interval.

Another especially important consideration of confidence intervals is that they *only try to capture the population parameter*. Our intervals say nothing about the confidence of capturing individual observations, a proportion of the observations, or about capturing point estimates. Confidence intervals provide an interval estimate for and attempt to capture population parameters.

\index{confidence interval!interpretation|)}
\index{confidence interval|)}



### Mathematical model summary


\BeginKnitrBlock{todo}<div class="todo">Math flow chart</div>\EndKnitrBlock{todo}



<table class="table" style="margin-left: auto; margin-right: auto;">
<caption>(\#tab:chp5-3-summary)Summary Mathematical Models as inferential statistical methods.</caption>
 <thead>
  <tr>
   <th style="text-align:left;">  </th>
   <th style="text-align:left;"> Mathematical Model </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> What does it do? </td>
   <td style="text-align:left;"> Uses theory (primarily the Central Limit Theorem) to describe the hypothetical variability resulting from either repeated randomized experiments or random samples. </td>
  </tr>
  <tr>
   <td style="text-align:left;"> What is the random process described? </td>
   <td style="text-align:left;"> either / both </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Is there flexibility? </td>
   <td style="text-align:left;"> Yes </td>
  </tr>
  <tr>
   <td style="text-align:left;"> What is it best for? </td>
   <td style="text-align:left;"> Quick analyses through, for example, calculating a Z score. </td>
  </tr>
  <tr>
   <td style="text-align:left;"> What physical object represents the simulation process? </td>
   <td style="text-align:left;"> NA </td>
  </tr>
</tbody>
</table>

## Chapter 5 review {#chp5-review}

In Chapter 5, we have provided three different methods for statistical inference.  We will continue to build on the methods throughout the text, and by the end, you should have an understanding of their similarities and differences.  Meanwhile, it is important to note that the methods are designed to mimic variability with data, and we know that variability can come from different sources (e.g., random sampling vs. random allocation).  In Table \@ref(tab:chp5summary), we have summarized some of the ways the inferential procedures feature specific sources of variability.  We hope that you refer back to the table often as you dive more deeply into the ideas in future chapters.




<table class="table" style="margin-left: auto; margin-right: auto;">
<caption>(\#tab:chp5summary)Summary and comparison of Randomization Tests, Bootstrapping, and Mathematical Models as inferential statistical methods.</caption>
 <thead>
  <tr>
   <th style="text-align:left;">  </th>
   <th style="text-align:left;">  Randomization Test  </th>
   <th style="text-align:left;"> Bootstrapping </th>
   <th style="text-align:left;"> Mathematical Model </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> What does it do? </td>
   <td style="text-align:left;"> Shuffles the explanatory variable to mimic the natural variability  found in a randomized experiment. </td>
   <td style="text-align:left;"> Resamples (with replacement) from the observed data to mimic the sampling variability found by collecting data. </td>
   <td style="text-align:left;"> Uses theory (primarily the Central Limit Theorem) to describe the hypothetical variability resulting from either repeated randomized experiments or random samples. </td>
  </tr>
  <tr>
   <td style="text-align:left;"> What is the random process described? </td>
   <td style="text-align:left;"> randomized experiment </td>
   <td style="text-align:left;"> random sampling </td>
   <td style="text-align:left;"> either / both </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Is there flexibility? </td>
   <td style="text-align:left;"> Yes, can be used to describe random sampling in an observational model </td>
   <td style="text-align:left;"> Yes, can be used to describe random allocation in an experiment </td>
   <td style="text-align:left;"> Yes </td>
  </tr>
  <tr>
   <td style="text-align:left;"> What is it best for? </td>
   <td style="text-align:left;"> Hypothesis Testing (can be used for Confidence Intervals, but not covered in this text). </td>
   <td style="text-align:left;"> Confidence Intervals (HT for one proportion covered in Chapter 6). </td>
   <td style="text-align:left;"> Quick analyses through, for example, calculating a Z score. </td>
  </tr>
  <tr>
   <td style="text-align:left;"> What physical object represents the simulation process? </td>
   <td style="text-align:left;"> shuffling cards </td>
   <td style="text-align:left;"> pulling balls from a bag </td>
   <td style="text-align:left;"> NA </td>
  </tr>
</tbody>
</table>


<div class="figure" style="text-align: center">
<img src="05/figures/randsampValloc.png" alt="Analysis conclusions should be made carefully according to how the data were collected.  Note that very few datasets come from the top left box because usually ethics require that randomly allocated treatments can only be given to volunteers." width="100%" />
<p class="caption">(\#fig:randsampValloc)Analysis conclusions should be made carefully according to how the data were collected.  Note that very few datasets come from the top left box because usually ethics require that randomly allocated treatments can only be given to volunteers.</p>
</div>

### Terms

We introduced the following terms in the chapter. 
If you're not sure what some of these terms mean, we recommend you go back in the text and review their definitions.
We are purposefully presenting them in alphabetical order, instead of in order of appearance, so they will be a little more challenging to locate. 
However you should be able to easily spot them as **bolded text**.

<table>
<tbody>
  <tr>
   <td style="text-align:left;"> 95% confidence interval </td>
   <td style="text-align:left;"> left skewed </td>
   <td style="text-align:left;"> percentile </td>
   <td style="text-align:left;"> standard error </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 95% confident </td>
   <td style="text-align:left;"> normal curve </td>
   <td style="text-align:left;"> permutation test </td>
   <td style="text-align:left;"> standard normal distribution </td>
  </tr>
  <tr>
   <td style="text-align:left;"> alternative hypothesis </td>
   <td style="text-align:left;"> normal distribution </td>
   <td style="text-align:left;"> point estimate </td>
   <td style="text-align:left;"> statistic </td>
  </tr>
  <tr>
   <td style="text-align:left;"> bootstrap sample </td>
   <td style="text-align:left;"> normal model </td>
   <td style="text-align:left;"> quantile-quantile plot </td>
   <td style="text-align:left;"> statistically significant </td>
  </tr>
  <tr>
   <td style="text-align:left;"> bootstrapping </td>
   <td style="text-align:left;"> normal probability plot </td>
   <td style="text-align:left;"> randomization </td>
   <td style="text-align:left;"> success </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Central Limit Theorem </td>
   <td style="text-align:left;"> normal probability table </td>
   <td style="text-align:left;"> randomization test </td>
   <td style="text-align:left;"> test statistic </td>
  </tr>
  <tr>
   <td style="text-align:left;"> confidence interval </td>
   <td style="text-align:left;"> null hypothesis </td>
   <td style="text-align:left;"> right skewed </td>
   <td style="text-align:left;"> Z score </td>
  </tr>
  <tr>
   <td style="text-align:left;"> hypothesis test </td>
   <td style="text-align:left;"> p-value </td>
   <td style="text-align:left;"> sampling with replacement </td>
   <td style="text-align:left;">  </td>
  </tr>
  <tr>
   <td style="text-align:left;"> independent </td>
   <td style="text-align:left;"> parameter </td>
   <td style="text-align:left;"> simulation </td>
   <td style="text-align:left;">  </td>
  </tr>
</tbody>
</table>