---
layout: post
title:  "The Best You Ever Had"
date:   2018-05-06 2:50:40 -0800
categories: probability choice decision
excerpt: An exploration of optimal stopping policies
---

Pretend for a minute that you are a recruiter whose looking for people to fill a position. Maybe you're reading this post because I have applied for a position at your company and you're vetting me or something. Anyways the point is I'm one candidate among many. Even if you interview me and like me how do you know there's not someone better out there to fill the position? <br><br>
More formally, you are going to interview $N$ candidates for a job. The order they come is random with all orderings will be equally likely. After each interview with a candidate you will come up with a quality score (a real number between 0 and 1) for them and then have to choose whether to hire them on the spot or reject them and hope a better fit comes along. Your goal is to hire the best of the best (the candidate who has the highest score among all N candidates) but you have no idea what the job market looks like (no information about the distribution from which these candidates are drawn). What is the probability that you succeed in your task given that you use the optimal hiring strategy <br><br>
This problem is know sometimes as the best choice problem or the secretary problem. It is a very famous and well studied problem even outside of math because of its very surprising solution, <br><br>
<div style="text-align:center;"> $\lim_{N \to \infty}Pr(\mbox{best is selected out of N candidates}) \to \frac{1}{e}$ </div><br>
This is a really cool result for several reasons. First of all it's not even clear that there should be any kind of one fits all strategy. Secondly $\frac{1}{e} = 0.367879...$ which seems absurdly high. If have a million candidates you have around a one third chance of choosing __the__ very best one. The strategy is also fairly simple and seemingly bizarre<br><br>
**Strategy:** For every N there is an $r_N^\* $ such that the following strategy is optimal. Reject the first $r_N^\* - 1$ candidates then choose the next candidate who is better all of then them. <br><br>
This type of strategy is called a cutoff rule. The final surprising fact is that <br>
<div style="text-align:center;"> $$\lim_{N \to \infty} r_N^* \to \frac{1}{e}$$ </div>
This means that you see less than half of the many candidates while making your decision. <br><br>
**Analysis**<br>
Analyzing the cutoff rule approach to this problem is actually fairly straight forward. Consider the cutoff rule with a variable $r$ which defines the cutoff point. The probability of finding the max can be though as follows, <br>
<div style="text-align:center;"> $$P(\mbox{success}) = \sum_{i=1}^N P(\mbox{applicant i is selected} \cap \mbox{applicant i is the best})$$ </div>
<div style="text-align:center;"> $$P(\mbox{success}) = \sum_{i=1}^N P(\mbox{applicant i is selected} \vert \mbox{applicant i is the best}) \times P(\mbox{applicant i is the best})$$ </div>
The probability of any applicant being the best if $\frac{1}{n}$ because our candidates are equally likely to appear in any order. So
<div style="text-align:center;"> $$P(\mbox{success}) = \sum_{i=1}^N(\mbox{applicant i is selected} \vert \mbox{applicant i is the best}) \times \frac{1}{n}$$ </div> <br>
[Continue explaining basic secretary problem]
<br><br>
So in the case where we know nothing about the distribution, the cutoff rule is optimal. What about if we do know something? This time we know our distribution is uniform on $[0,1]$. Now let's try to come up with an optimal strategy for this version. After each interview we have to accept or reject the candidate we just interviewed (let's call this candidate the ith one). To gain some insight we can take a look at the following statement, <br>
<div style="text-align:center;">$$P(\mbox{best candidate was in past}) + P(\mbox{candidate i is best}) + P(\mbox{best candidate is in future}) = 1$$</div>
Now we don't have control over the past but we do have the choice between taking i and rejecting i. Therefore we pick choose the option between those two which has the highest probability of giving us the best candidate. Let's make a few observations about these probabilities. <br><br>
For example that if candidate i's score is less than any of the previous candidates we know that:
<div style="text-align:center;">$$P(\mbox{candidate i is best}) = 0$$</div>
In this case we would always choose to move on because no matter what our chance is greater than or equal to 0 so we can't do any worse and we might do better. <br><br>
Now what if candidate i is better than all the other candidates we've seen so far? Off the bat we know that, <br>
<div style="text-align:center;">$$P(\mbox{best candidate was in past}) = 0$$</div>
This means that,
<div style="text-align:center;">$$P(\mbox{candidate i is best}) + P(\mbox{best candidate is in future}) = 1$$</div>
<div style="text-align:center;">$$P(\mbox{best candidate is in future}) = 1 - P(\mbox{candidate i is best})$$</div><br>
Let $q_i$ denote candidate i's quality score. That we want now is to know the probability that we will not find a better candidate in our last $N-i$ interviews. Since our candidate scores are drawn from a uniform distribution on $[0,1]$,<br>
<div style="text-align:center;">$$ P(\mbox{candidate j's score } \leq q_i) = q_i, \forall j $$ </div>
<div style="text-align:center;">$$ P(\mbox{last N-i candidates are all } \leq q_i) = \prod_{j = i+1}^{N}P(\mbox{candidate j's score } \leq q_i) = q_i^{N-i} $$ </div>
<div style="text-align:center;">$$ P(\mbox{best candidate in future}) = 1 - P(\mbox{last N-i candidates are all } \leq q_i) = 1 - q_i^{N-i} $$ </div>
So when is it optimal to choose to accept candidate i? It is when,
<div style="text-align:center;">$$P(\mbox{candidate i is best}) \geq P(\mbox{best candidate is in future})$$</div>
Equivalently this when,
<div style="text-align:center;">$$P(\mbox{best candidate is in future}) \leq \frac{1}{2}$$</div>
<div style="text-align:center;">$$1 - q_i^{N-i} \leq \frac{1}{2}$$</div>
Using this information we can solve for the threshold $q^*_i$ such that if $q_i \geq q^\*_i$ our best option is to choose candidate i.
<div style="text-align:center;">$$1 - {q^*_i}^{N-i} = \frac{1}{2}$$</div>
<div style="text-align:center;">$${q^*_i}^{N-i} = \frac{1}{2}$$</div>
<div style="text-align:center;">$${q^*_i}^{N-i} = \frac{1}{2}$$</div>
<div style="text-align:center;">$$q^*_i = \frac{1}{2}^\frac{1}{N-i}$$</div><br>

So now we want to calculate the total probability of success under this new optimal strategy <br>
Following a similar idea from our analysis of the more general optimal stopping problem we can say, <br>
<div style="text-align:center;"> $$P(\mbox{we find the max}) = \sum_{i=1}^{N} P(\mbox{we select the max during interview i})$$ </div>
Notice that this sum is okay because all of these events are mutually exclusive. <br>
<div style="text-align:center;"> $$P(\mbox{we find the max}) = \big(\sum_{i=1}^{N-1} P(\mbox{candidate i is selected} \cap \mbox{candidate i is max})\big) + P(we succeed at interview n)$$ </div>
<div style="text-align:center;"> $$P(\mbox{we find the max}) = \sum_{i=1}^N P(\mbox{candidate i is selected} \cap \mbox{candidate i is max})$$ </div>


<br><br><br>





