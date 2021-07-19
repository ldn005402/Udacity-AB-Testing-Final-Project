# Udacity-AB-Testing-Final-Project-To Determine an Effective Approach to Reduce Early Udacity Course Cancellation

**Final Project Instructions

Experiment Overview: Free Trial Screener
At the time of this experiment, Udacity courses currently have two options on the course overview page: "start free trial", and "access course materials". If the student clicks "start free trial", they will be asked to enter their credit card information, and then they will be enrolled in a free trial for the paid version of the course. After 14 days, they will automatically be charged unless they cancel first. If the student clicks "access course materials", they will be able to view the videos and take the quizzes for free, but they will not receive coaching support or a verified certificate, and they will not submit their final project for feedback.


In the experiment, Udacity tested a change where if the student clicked "start free trial", they were asked how much time they had available to devote to the course. If the student indicated 5 or more hours per week, they would be taken through the checkout process as usual. If they indicated fewer than 5 hours per week, a message would appear indicating that Udacity courses usually require a greater time commitment for successful completion, and suggesting that the student might like to access the course materials for free. At this point, the student would have the option to continue enrolling in the free trial, or access the course materials for free instead. This screenshot shows what the experiment looks like.


The hypothesis was that this might set clearer expectations for students upfront, thus reducing the number of frustrated students who left the free trial because they didn't have enough time—without significantly reducing the number of students to continue past the free trial and eventually complete the course. If this hypothesis held true, Udacity could improve the overall student experience and improve coaches' capacity to support students who are likely to complete the course.

**Experimental Design

The unit of diversion is a cookie, although if the student enrolls in the free trial, they are tracked by user-id from that point forward. The same user-id cannot enroll in the free trial twice. For users that do not enroll, their user-id is not tracked in the experiment, even if they were signed in when they visited the course overview page.


**Metric Choice

Invariant Metrics: 
1). number of cookies: That is, number of unique cookies to view the course overview page. (dmin=3000)
2). number of clicks: That is, number of unique cookies to click the "Start free trial" button (which happens before the free trial screener is trigger). (dmin=240)
3). click-through-probability: That is, number of unique cookies to click the "Start free trial" button divided by number of unique cookies to view the course overview page. (dmin=0.01)
Evaluation Metrics: 
1). gross conversion: That is, number of user-ids to complete checkout and enroll in the free trial divided by number of unique cookies to click the "Start free trial" button. (dmin= 0.01)
2). retention: That is, number of user-ids to remain enrolled past the 14-day boundary (and thus make at least one payment) divided by number of user-ids to complete checkout. (dmin=0.01)
3). net conversion: That is, number of user-ids to remain enrolled past the 14-day boundary (and thus make at least one payment) divided by the number of unique cookies to click the "Start free trial" button. (dmin= 0.0075)

**Unused Metrics

Number of user-ids: The number of users who enroll in the free trial. User-ids are tracked only after enrolling in the free trial and equal distribution between the control and experimental branches would not be expected. User-id count could be used to evaluate how many enrollments stayed beyond the 14 day free trial boundary, but since it isn't normalized, I have elected not to use it.


**Sizing

Choosing Number of Samples given Power
Page views required for each evaluation metric is calculated separately using the online calculator. The alpha vaue of 0.05 and beta value of 0.2 is used in all the cases.

Gross Conversion
Baseline Conversion: 20.625%
Minimum Detectable Effect: 1%
Alpha: 5%
Beta: 20% -Sensitivity (1 - Beta): 80%
Sample Size = 25,835 enrollments/group
Number of groups = 2 (experiment and control)
Total sample size = 51,670 enrollments
Clicks/Pageview: 3200/40000 = 0.08 clicks/pageview
Pageviews Required = 6,45,875

Retention
Baseline Conversion: 53%
Minimum Detectable Effect: 1%
Alpha: 5%
Beta: 20%
Sensitivity (1 - Beta): 80%
Sample size = 39,155 enrollments/group
Number of groups = 2 (experiment and control)
Total sample size = 78,230 enrollments
Enrollments/pageview: 660/40000 = 0.0165 enrollments/pageview
Pageviews = 78,230/0.0165 = 47,41,212

Net Conversion
Baseline Conversion: 10.9313%
Minimum Detectable Effect: 0.75%
Alpha: 5% -Beta: 20%
Sensitivity (1 - Beta): 80%
Sample size = 27,413 enrollments/group
Number of groups = 2 (experiment and control)
Total sample size = 54,826 enrollments
Enrollments/pageview: 3200/40000 = 0.08 clicks/pageview
Pageviews = 6,85,325


Choosing Duration vs. Exposure
If we divert 100% of traffic, given 40,000 page views per day, the experiment would take ~ 119 days. If we eliminate retention, we are left with Gross Conversion and Net Conversion. This reduces the number of required pageviews to 685,325, and an ~ 18 day experiment with 100% diversion and ~ 35 days given 50% diversion.

A 119 day experiment with 100% diversion of traffic presents both a business risk (potential for: frustrated students, lower conversion and retention, and inefficient use of coaching resources) and an opportunity risk (performing other experiments). However, in general, this is not a risky experiment as the change would not be expected to cause a precipitous drop in enrollment. In terms of timing, an 18 day experiment is more reasonable, but % diversion may be scaled down depending on other experiments of interest to be performed concurrently.



**Analysis

Sanity Checks

|Metric                                |Expected Value|Observed Value|CI Lower Bound|CI Upper Bound|Result|
|--------------------------------------|--------------|--------------|--------------|--------------|------|
|Number of Cookies                     |0.5000        |0.5006        |0.4988        |0.5012        |Pass  |
|Number of Clicks on "Start Free Trail"|0.5000        |0.5005        |0.4959        |0.5042        |Pass  |
|Click-Through-Probability             |0.0821        |0.0822        |0.0812        |0.0830        |Pass  |



**Summary

An experiment was conducted in which potential Udacity students were diverted by cookie into two groups, experiment and control. The experiment group was asked to input the amount of time they are willing to devote to study, after clicking a "start free trial button", whereas the control group was not. Three invariant metrics (Number of Cookies, Number of clicks on "start free trial", and Click-Through-Probability) were chosen for purposes of validation and sanity checking while Gross Conversion (enrollment/cookie) and Net Conversion (payments/cookie) served as evaluation metrics. The null hypothesis is that there is no difference in the evaluation metrics between the groups, futhermore, a practical signifcance threshold was set for each metric. The requirement for launching the experiment is that the null hypothesis must be rejected for ALL evaluation metrics and that the difference between branches must meet or exceed the practical signficance threshold. Because our acceptance criteria requires statiscally signifcant differences for ALL evaluation metrics, the use of the Bonferonni correction is not appropriate. The Bonferonni correction is a method for controlling for type I errors (false positives) when using multiple metrics in which relevance of ANY of the metrics matches the hypothesis. In this case the risk of type I errors increases as the number of metrics increases (signifcance by random chance). In our case in which ALL metrics must be relevant to launch, the risk of type II errors (false negatives) increases as the number of metrics increases, so it stands to reason that controlling for false positives is not consistent with our acceptance criteria.

Analysis revealed the expected equal distribution of cookies into the control and experimental groups, for the invariant metrics, at the 95% CI. A difference in gross conversion was found to be statistically signficant at the 95% CI, and the null hypothesis was rejected. Gross conversion also met the practical signficance threshold. Net Conversion was found to be neither statistically nor practically signficant at the 95% CI.

**Make a Recommendation

This experiment was designed to determine whether filtering students as a function of study time commitment would improve the overall student experience and the coaches' capacity to support students who are likely to complete the course, without significantly reducing the number of students who continue past the free trial. A statistically and practically signficant decrease in Gross Conversion was observed but with no significant differences in Net Conversion. This translates to a decrease in enrollment not coupled to an increase in students staying for the requisite 14 days to trigger payment. Considering this, my recomendation is not to launch, but rather to pursue other experiments.


**Follow-Up Experiment: How to Reduce Early Cancellations

The construct of student frustration could be assigned an operational definition of "cancel early," where a convenient definition and measure of early cancellation is prior to the end of the 14 day trial period in which payment is triggered. An early cancellation is not necessarily indicative of frustration but could be from other causes, such as a course not being aligned to the students needs or expectations in terms of content. For preventng early cancellation there are two primary logical timepoint opportunities for intervention, (1) pre-enrollment, and (2) post-enrollment but pre-payment.

The first opportunity for intervention was explored above wherein a poll regarding time commitment was used as to filter out students likely to become frustrated. This filter focused only on time commitment to the class and did not address other reasons why a student might become frustrated and cancel early. Even if the student was sincere in their response and dilligent in their study, they may become frustrated if they don't have the suggested pre-requisite skills and experience. That is, their committed time may not be enough if they don't come in with the pre-requisite skill set. Adding a checklist of pre-requisite skills to the popup regarding time commitment may be informative. This experiment would leverage the infrastrucure and data pipeline of the original experiment and be set up in the same way as the original, including the unit of diversion. The only difference would be the information in the form. If the student's answer meets the time and pre-requisite requirements (radiobox checklist) they are directed to enroll in the free trial, otherwise they are encouraged to access the free version. This experiment would be low cost in terms of resources and may increase the selectivity of the pre-enrollment filter. A succesful experiment would be one in which there is a signficant decrease in Gross Conversion coupled to a significant increase in Net Conversion.

A variety of approaches could be used to intervene post-enrollment but pre-payment and could be deployed concurrently with pre-enrollment intervention. An ideal approach would be one which minimizes the use of additional coaching resources to best meet the original intent of the intervention. An effective approach may be to employ peer coaching/guidance by means of team formation. If a student has a team of other students which they could consult, discuss coursework and frustrations with, and be accountable to, they may be more likely to stick out the growing pains and stay for the long term. The experiment would function in the following manner.

Setup: Upon enrollment students will either be randomly assigned to a control group in which they are not funnelled into a team, or an experiment group in which they are.

Null Hypothesis: Participation in a team will not increase the number of students enrolled beyond the 14 day free trial period by a significant amount.

Unit of Diversion: The unit of diversion will be user-id as the change takes place after a student creates an account and enrolls in a course.

Invariant Metrics: The invariant metric will be user-id, since an equal distribution between experiment and control would be expected as a property of the setup.

Evaluation Metrics: The evaluation metric willl be Retention. A statistically and practically significant increase in Retention would indicate that the change is succesful.

If a statistically and practically signifcant positive change in Retention is observed, assuming an acceptable impact on overall Udacity resources (setting up and maintaining teams will require resource use), the experiment will be launched.
