# A/B Testing Project

The following repository has the intention to run an A/B experiment from start to finish, from the metric definition to the business recommendation passing by aspects like the design and execute of the experiment and the analysis of results.

## Experiment Overview: Free Trial Screener
At the time of this experiment, Udacity courses currently have two options on the course overview page: "start free trial", and "access course materials". If the student clicks "start free trial", they will be asked to enter their credit card information, and then they will be enrolled in a free trial for the paid version of the course. After 14 days, they will automatically be charged unless they cancel first. If the student clicks "access course materials", they will be able to view the videos and take the quizzes for free, but they will not receive coaching support or a verified certificate, and they will not submit their final project for feedback.


In the experiment, Udacity tested a change where if the student clicked "start free trial", they were asked how much time they had available to devote to the course. If the student indicated 5 or more hours per week, they would be taken through the checkout process as usual. If they indicated fewer than 5 hours per week, a message would appear indicating that Udacity courses usually require a greater time commitment for successful completion, and suggesting that the student might like to access the course materials for free. At this point, the student would have the option to continue enrolling in the free trial, or access the course materials for free instead.


The hypothesis was that this might set clearer expectations for students upfront, thus reducing the number of frustrated students who left the free trial because they didn't have enough time—without significantly reducing the number of students to continue past the free trial and eventually complete the course. If this hypothesis held true, Udacity could improve the overall student experience and improve coaches' capacity to support students who are likely to complete the course.


The unit of diversion is a cookie, although if the student enrolls in the free trial, they are tracked by user-id from that point forward. The same user-id cannot enroll in the free trial twice. For users that do not enroll, their user-id is not tracked in the experiment, even if they were signed in when they visited the course overview page.


## 1. Metric Choice
Which of the following metrics would you choose to measure for this experiment and why? For each metric you choose, indicate whether you would use it as an invariant metric or an evaluation metric. The practical significance boundary for each metric, that is, the difference that would have to be observed before that was a meaningful change for the business, is given in parentheses. All practical significance boundaries are given as absolute changes.


Any place "unique cookies" are mentioned, the uniqueness is determined by day. (That is, the same cookie visiting on different days would be counted twice.) User-ids are automatically unique since the site does not allow the same user-id to enroll twice.


* Number of cookies: That is, number of unique cookies to view the course overview page. (dmin=3000)
* Number of user-ids: That is, number of users who enroll in the free trial. (dmin=50)
* Number of clicks: That is, number of unique cookies to click the "Start free trial" button (which happens before the free trial screener is trigger). (dmin=240)
* Click-through-probability: That is, number of unique cookies to click the "Start free trial" button divided by number of unique cookies to view the course overview page. (dmin=0.01)
* Gross conversion: That is, number of user-ids to complete checkout and enroll in the free trial divided by number of unique cookies to click the "Start free trial" button. (dmin= 0.01)
* Retention: That is, number of user-ids to remain enrolled past the 14-day boundary (and thus make at least one payment) divided by number of user-ids to complete checkout. (dmin=0.01)
* Net conversion: That is, number of user-ids to remain enrolled past the 14-day boundary (and thus make at least one payment) divided by the number of unique cookies to click the "Start free trial" button. (dmin= 0.0075)

You should also decide now what results you will be looking for in order to launch the experiment. Would a change in any one of your evaluation metrics be sufficient? Would you want to see multiple metrics all move or not move at the same time in order to launch? This decision will inform your choices while designing the experiment.


#### Invariant metrics: The following variables marked as invariant and should not change across groups in the experiment. 
* Number of cookies: This is the unit of diversion, since the user_id is greater than a cookie and the user_id is not stored for the users that are not enrolled to courses. Cookies will be randomly assigned to control or experiment group and both groups should have roughly the same amount number of cookies.
* Number of clicks: The number of clicks to the "Start free trial" button should also remain constant since the behavior of the button should not change in the experiment, since it is located before the changes we want to measure.
* Click-through-probability: The probability that a user clicks the "Start free trial" button should be the same in two groups for the same reason that the number of clicks should remain similar.


#### Evaluation metrics: Metrics that will be used to assess changes across the control and experiment groups. The metrics that should be chosen for evaluation are the ones that are expected to have an impact in the experiment group.
* Gross conversion: The total number of users that enroll in the free trial should be reduced since the message that is being shown in the experiment group should make less dedicated students to retract the enrolling decision in some cases. The total number is being normalized by the number of cookies that click the "Free trial" button
* Retention: The users that keep enrolled after the 14 day trial divided by the total number of users that started the free trial es expected to increase since it is expected that the more dedicated users are the ones that actually use the free trial.
* Net conversion: The net conversion, that is the number of users (ids) that remain enrolled and make a payment may be increase since we are expecting more dedicated users to be enrolled in the course, those users have more probability to keep being enrolled after the free trial. This metric is more likely to increase if the retention increases since the probability of enrollment further the 14 trial is even higher .Again this metric is being normalized by the number of cookies that click the "Free trial" button.


#### Number of user-ids:
* This variable will not be used in this experiment as there is no expectancy of this metric to be constant across experiments and it does not give us enough information as out other metrics have to be an evaluation metric.

## 2. Measuring Variability
This spreadsheet contains rough estimates of the baseline values for these metrics (again, these numbers have been changed from Udacity's true numbers).


For each metric you selected as an evaluation metric, estimate its standard deviation analytically. Do you expect the analytic estimates to be accurate? That is, for which metrics, if any, would you want to collect an empirical estimate of the variability if you had time?

<!-- variability results -->

```python
# Variability results
f = 0.1  # sample size
for i in range(1, 10):
    s = f*i  # fraction of the sample being used
    p = 0.206250  # probability of enrollment given click
    n = 3200*s  # cookies that click button times the sample size that is being used
    V = np.sqrt((p*(1-p))/n)
    print(f"Variability with sample size {int(s*4000)}: {V}")
```
```python
# Gross conversion
Variability with sample size 4000: 0.022618503038165455
Variability with sample size 8000: 0.015993696878575322
Variability with sample size 12000: 0.013058798817751193
Variability with sample size 16000: 0.011309251519082728
Variability with sample size 20000: 0.010115302068524696
Variability with sample size 24000: 0.009233965198182738
Variability with sample size 28000: 0.008548990581077812
Variability with sample size 32000: 0.007996848439287661
Variability with sample size 36000: 0.007539501012721819
```

```python
# Retention
Variability with sample size 4000: 0.06143486326380506
Variability with sample size 8000: 0.04344100841510487
Variability with sample size 12000: 0.035469434842985696
Variability with sample size 16000: 0.03071743163190253
Variability with sample size 20000: 0.02747450608925454
Variability with sample size 24000: 0.025080677902329592
Variability with sample size 28000: 0.02322019571789801
Variability with sample size 32000: 0.021720504207552435
Variability with sample size 36000: 0.020478287754601684
```

```python
# Net conversion
Variability with sample size 4000: 0.017443092116848864
Variability with sample size 8000: 0.012334128720685442
Variability with sample size 12000: 0.010070773929162129
Variability with sample size 16000: 0.008721546058424432
Variability with sample size 20000: 0.0078007879422129535
Variability with sample size 24000: 0.0071211125371072334
Variability with sample size 28000: 0.006592869119596187
Variability with sample size 32000: 0.006167064360342721
Variability with sample size 36000: 0.005814364038949621
```

One expected behaviour that we see when we perform the variability for different sample sizes is that the variance decreases are more data is used in the experiment, the variability is proportional to the square root of N.

The only metric which will be interesting to assess the empirical variability is the Retention, since the unit of analysis (user_id) is not the same as the unit of diversion (cookie)

## 3. Sizing
### Choosing Number of Samples given Power
Using the analytic estimates of variance, how many pageviews total (across both groups) would you need to collect to adequately power the experiment? Use an alpha of 0.05 and a beta of 0.2. Make sure you have enough power for each metric.


The following data power will be computed using an online calculator (https://www.evanmiller.org/ab-testing/sample-size.html)

<!-- code for collecting the number of samples -->

```python
# Collecting number of samples
p = 0.206250 # probability of enrollment given click
b = 0.2
a = 0.05
d_min = 0.01
samples = 25835 # from online calculator
print(f"Number of samples needed for Gross conversion: {samples}")

groups = 2
click_per_view = 3200/40000 # 3200 clicks every 40000 pageviews
page_views = samples/click_per_view # total page views needed
total_views = groups*page_views
print(f"A total of {int(total_views)} page_views needs to be taken.")
```

```python
Number of samples needed for Gross conversion: 25835
A total of 645875 page_views needs to be taken.
```

```python
Number of samples needed for Retention: 39115
A total of 4741212 page_views needs to be taken.
```

```python
Number of samples needed for Net conversion: 27413
A total of 685325 page_views needs to be taken.
```

In order for the experiment to work for all three cases, the total number of pagevies that need to be accounted for the experiment are: 4741212 

### Choosing Duration vs. Exposure
What percentage of Udacity's traffic would you divert to this experiment (assuming there were no other experiments you wanted to run simultaneously)? Is the change risky enough that you wouldn't want to run on all traffic?


Given the percentage you chose, how long would the experiment take to run, using the analytic estimates of variance? If the answer is longer than a few weeks, then this is unreasonably long, and you should reconsider an earlier decision.

The change is considerable risky since it may change the probability of enrollment, meaning changes in the revenue of the company. Technical risk are not asumed high since the only changes are an "alert" modal with a redirect button.

<!-- code explaining the traffic -->
```python
# 100% traffic
Days to measure Retention: 119
Days to measure Gross conversion: 18
Days to measure Net conversion: 17
```

```python
# 80% traffic
Days to measure Retention: 149
Days to measure Gross conversion: 22
Days to measure Net conversion: 21
```

Using 100% of the traffic in the experiment will need 119 days to accout for all the pageviews for the three metrics that we are assessing. Since there is a big difference between the pageviews needed for the conversion and retention metrics, it may be a good idea to drop the retention metric. The sorter term coversion metrics only need 18 days to run the experiment on 100% of the traffic but because it is not recommended, because the experiment may be risky and also to allow room for more experiments to be performed, only the 80% of the traffic will be used. A total of 22 days will be needed to collect all the pageviews for our test.


## Analysis
The data for you to analyze is here. This data contains the raw information needed to compute the above metrics, broken down day by day. Note that there are two sheets within the spreadsheet - one for the experiment group, and one for the control group.


The meaning of each column is:

* Pageviews: Number of unique cookies to view the course overview page that day.
* Clicks: Number of unique cookies to click the course overview page that day.
* Enrollments: Number of user-ids to enroll in the free trial that day.
* Payments: Number of user-ids who who enrolled on that day to remain enrolled for 14 days and thus make a payment. (Note that the date for this column is the start date, that is, the date of enrollment, rather than the date of the payment. The payment happened 14 days later. Because of this, the enrollments and payments are tracked for 14 fewer days than the other columns.)

### Sanity Checks
Start by checking whether your invariant metrics are equivalent between the two groups. If the invariant metric is a simple count that should be randomly split between the 2 groups, you can use a binomial test as demonstrated in Lesson 5. Otherwise, you will need to construct a confidence interval for a difference in proportions using a similar strategy as in Lesson 1, then check whether the difference between group values falls within that confidence level.


If your sanity checks fail, look at the day by day data and see if you can offer any insight into what is causing the problem.

<!-- output for the sanity check of each metric -->
```python
# Number of cookies
Count in control group: 345543
Count in experiment group: 344660
Standard error: 0.000602
Margin of error: 0.00118
Confidence Interval: Lower bound = 0.49882
Confidence Interval: Upper bound = 0.50118
Observed probability: 0.49936
Sanity check passed! (0.49936 in [0.49882,0.50118])
```

```python
# Number of clicks
Count in control group: 28378
Count in experiment group: 28325
Standard error: 0.0021
Margin of error: 0.004116
Confidence Interval: Lower bound = 0.495884
Confidence Interval: Upper bound = 0.504116
Observed probability: 0.499533
Sanity check passed! (0.499533 in [0.495884,0.504116])
```

```python
# CTR
Pooled probability: 0.082154
Standard error: 0.000331
Margin of error: 0.000648
Confidence Interval: Lower bound = 0.081506
Confidence Interval: Upper bound = 0.082802
Observed probability: 0.082182
Sanity check passed! (0.082182 in [0.081506,0.082802])
```

```python
# Enrollments
Count in control group: 3785.0
Count in experiment group: 3423.0
Standard error: 0.005889
Margin of error: 0.011543
Confidence Interval: Lower bound = 0.488457
Confidence Interval: Upper bound = 0.511543
Observed probability: 0.474889
Sanity check not passed! (0.474889 not in [0.488457,0.511543])
```

```python
# Payments
Count in control group: 2033.0
Count in experiment group: 1945.0
Standard error: 0.007928
Margin of error: 0.015538
Confidence Interval: Lower bound = 0.484462
Confidence Interval: Upper bound = 0.515538
Observed probability: 0.488939
Sanity check passed! (0.488939 in [0.484462,0.515538])
```

The results seen in the sanity check work in accordance as we can expect, all the metrics pass the sanity check as there is no significant difference between the experiment and control groups. The only metric that shows an statistical significant difference is the "Enrollments" metric, which is not a invariant metric but is one metric where we want to see an actual difference. Having less observations in the experiment group means that the expected behaviour of the modal is as intended and reducing the enrollments for less dedicated users.


### Check for Practical and Statistical Significance
Next, for your evaluation metrics, calculate a confidence interval for the difference between the experiment and control groups, and check whether each metric is statistically and/or practically significance. A metric is statistically significant if the confidence interval does not include 0 (that is, you can be confident there was a change), and it is practically significant if the confidence interval does not include the practical significance boundary (that is, you can be confident there is a change that matters to the business.)


If you have chosen multiple evaluation metrics, you will need to decide whether to use the Bonferroni correction. When deciding, keep in mind the results you are looking for in order to launch the experiment. Will the fact that you have multiple metrics make those results more likely to occur by chance than the alpha level of 0.05?

<!-- statistical significance for each metric -->
```python
# Gross convertion
Pooled probability: 0.208607
Standard error: 0.004372
Margin of error: 0.008568
Confidence Interval: Lower bound = -0.029123
Confidence Interval: Upper bound = -0.011986
Difference of experiment and control probabilities: -0.020555
Difference is greater thatn the practical significance boundary
```

```python
# Retention
Pooled probability: 0.551887
Standard error: 0.01173
Margin of error: 0.02299
Confidence Interval: Lower bound = 0.008104
Confidence Interval: Upper bound = 0.054085
Difference of experiment and control probabilities: 0.031095
Difference is greater thatn the practical significance boundary
```

```python
# Net conversion
Pooled probability: 0.115127
Standard error: 0.003434
Margin of error: 0.006731
Confidence Interval: Lower bound = -0.011605
Confidence Interval: Upper bound = 0.001857
Difference of experiment and control probabilities: -0.004874
The difference is not greater than the practival significance boundary
```

After evaluating the significance of the metrics and assuming that we were able to collect all the necessary page views, the behavior of the evaluation metrics was the expected one. 
The Gross conversion metric show an statistical significant difference in the experiment group with respect to the control group, the confidence interval at 95% significance level, does not contain 0 and the difference is greater than the practical significance level. An important outcome of this metric what to contrast if the number of enrolled users would decrease and they in fact did since we observed a 2% difference.
The Retention, again given that we had enough power to collect data, also showed an statistical significant difference with respect the control group. We wanted for this metric to increase and it indeed increased with an augment of 3%, again greater than the practical significance level, meaning that the users that enroll to the free trial are more commited to the study and continue enrolled after the 14 day period.
The Net conversion did not showed statistical significance difference since the confidence interval contains the 0 value (we are not guaranteed that the difference will always be different than zero 95% percent of the time). The absolute difference is also not greater than the practical significance boundary. This metric not being significative is actually a good sign, although the number of users that enroll the free trial decreases, the number of users that keep enrolled after the 14 day period may be reduced a little but there is no significal difference so the revenue of the company will not be affected significantly and the instructurs will be focused on more commited students.


### Run Sign Tests
For each evaluation metric, do a sign test using the day-by-day breakdown. If the sign test does not agree with the confidence interval for the difference, see if you can figure out why.

<!-- results for each sign test -->
```python
# Gross covertion
# Wilcoxon test
statistic=35.0
pvalue=0.0009758472442626953
```

```python
# Retention
# Wilcoxon test
statistic=107.0 
pvalue=0.36039113998413086

```

```python
# Net convertion
# Wilcoxon test
statistic=120.0
pvalue=0.6010458469390869
```
Making the Sign test make us realize that there is not a really significant difference in the Retention metric. Initially we already knew that we needed more power in order to properly run this experiment. The results testing with confidence intervals were significative only by chance. Seeing the high variance that this metric has and being the condifence interval that close to zero was something that had to make us doubt about the significance of this metric.

Gross conversion and Net conversion are variables that behave like we expected.


### Make a Recommendation
Finally, make a recommendation. Would you launch this experiment, not launch it, dig deeper, run a follow-up experiment, or is it a judgment call? If you would dig deeper, explain what area you would investigate. If you would run follow-up experiments, briefIy describe that experiment. If it is a judgment call, explain what factors would be relevant to the decision.

I would recommend to retrieve more data for this experiment since it sounds promessing. The results that we have analyzed with the small number of samples that we have available showed that even if the proportion of enrolled studets decreased, the number of students that keep paying after being enrolled did not decrease, meaning that what we wanted to accomplish with this experiment which was to readuce the number of non-commited students has been accomplished.


## Follow-Up Experiment: How to Reduce Early Cancellations
If you wanted to reduce the number of frustrated students who cancel early in the course, what experiment would you try? Give a brief description of the change you would make, what your hypothesis would be about the effect of the change, what metrics you would want to measure, and what unit of diversion you would use. Include an explanation of each of your choices.

In order to reduce early cancellations, first we have to understand what is what cause the cancellation for students. It may be the difficulty of the courses (which can be increased if the students are not really prepared for this course), the performance of the page, maybe the personalized experience (teachers and additional rosources) are not responding correctly. 

After studying some of the reasons that can make a user not be interested in the subscription and therefore cancelling it, we have proposed the following added feature: Congratulatory message and reward points every daily login.

The hypothesis is that this change should decrease the number of early cancellations since the user would be more interested in the education page.

In order to study these metrics we can collect the following metrics:
* Daily logins (user diversion metric): this metric will help us to measure the daily logins -> It should increase in the experiment in order to validate this change
* Number of lessons a student does every day (user diversion metric): this number also can increase if the user will receive more rewards
* Average time of lesson (user diversion metric): this metric should not change since the change should not increase the difficulty of the courses