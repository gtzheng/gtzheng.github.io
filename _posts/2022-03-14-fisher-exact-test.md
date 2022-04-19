---
layout: post
title: Fisher's Exact Test
---
Fisher's exact test is a statistical significance test used to determine whether there are nonrandom associations between two categorical variables [[1]][1]. We usually start the test by assuming that there is no/random relation between the two variables. The assumption is called the null hypothesis of the test; it contains no interesting information about the two variables. Based on the assumption, then we calculate the probability of certain outcomes that would provide evidence to reject the null hypothesis and to accept that the two variables are associated. Each outcome is a contingency table showing the frequency of variables in different states in a matrix format. The simplest and the most commonly used table is a 2-by-2 table, i.e., each of the two variables has two states.

## Contingency Table
The 2-by-2 contingency table is shown below, where we have two variables $A\in\{a_1,a_2\}$ an $B\in\{b_1,b_2\}$. The length-4 tuple $(k,  K-k, n-k, N-K-n+k)$ specifies an outcome. One notable property of the contingency table is that given the marginals $N$, $K$, $n$ and one table entry $k$, the whole table can be uniquely determined. In other words, the degree of freedom of the table is one given $N$, $K$, and $n$. The goal is to test whether $A$ and $B$ are associated given the table. 

|        | $a_1$ | $a_2$  |        |
|:------:|:----:|:----:|:------:|
| $b_1$ |   $k$  |  $K-k$   |    $K$   |
|  $b_2$ |   $n-k$  |   $N-K-n+k$  |    $N-K$   |
|    |   $n$  |   $N-n$  |    $N$ |

## Test Method
The idea is that we change $k$ (increase or decrease) while keeping the marginals $N$ ,$K$, and $n$ unchanged. In this way, we are changing the proportion of the value $b_1$ occurring in all the samples of $B$ given that $A=a_1$ (there are $n$ samples). Note that given $N$ ,$K$, and $n$, specifying $k$ only can uniquely determine the table. Hence, by changing $k$, we are creating many new outcomes that are making the two variables $A$ and $B$ more or less associated. The final goal is to determine the "position" of the given outcome (the 2-by-2 contingency table) in the distribution of all possible outcomes (i.e., with different $k$). For example, in the following figure, $k$ ranges from 0 to 8, and $k=7$ in the given outcome/contingency table. We calculate the probability of $k$ being greater or equal to 7 under the null hypothesis, i.e., $A$ and $B$ are not associated, and the probability is 0.00988. 

The value is called the <span style="color:blue">$p$-value</span> which gives us a sense of how likely for $k$ going to the extreme. We see that this value is very small, indicating that variable $A$ and $B$ being associated under the null hypothesis is very unlikely. Or more specifically, the hypothesis that $a_1$ associates with $b_1$ more than $a_2$ does (since we increase $k$ in the $p$-value calculation) under the null hypothesis is very unlikely; however, in reality, we have observed this outcome. Hence, we have to reject the null hypothesis and conclude that the hypothesis that $a_1$ associates with $b_1$ more than $a_2$ does shows significance beyond random chance.

In the above example, we are doing the right-tail test since we increase $k$; in contrast, we are doing the left-tail test if we decrease $k$. We can conduct a two-tailed test if we consider the some of the left-most and right-most tails of the distribution. Two-tailed test is usually preferred to the single-tailed test since a single-tailed (left or right) test has a stronger assumption than the two tailed test which assumes relation in both directions [[2]][2]. Different directions give different questions or hypotheses that we are interested in, and we will demonstrate this in the example below.

![illustration](/assets/images/fisher_exact_test_illustration.png)
(Hypergeometric distribution with $N = 20$, $K = 10$, $n = 8$, and $k$ from 0 to 8)


## Null Hypothesis and Hypergeometric Distribution
The null hypothesis is the assumption we make when calculating the $p$-value. It assumes there is no/random association between two variables. The null hypothesis leads to a hypergeometric distribution defined as follows:
$$p(k)=\frac{\binom{K}{k}\binom{N-K}{n-k}}{\binom{N}{n}},$$
where $\binom{K}{k}$ represents the total number of possibilities when choosing $k$ samples from a total of $K$ samples, and $k\in[0,\min(K,n)]$. The value $p(k)$ is the probability of having a table with a specific $k$.

## An Example
I have two cats, Summer and Shirley, and two plants, cat grass and pink rose. I count how many times they approach to each of the plants in a day, and the contingency table is shown below. Here, the two variables we want to test are MY_CAT and MY_PLANT, and each of the variables has two states: MY_CAT contains Summer and Shirley; MY_PLANT contains cat grass and pink rose. I want to know whether MY_CAT has association with MY_PLANT. If there is no association, then no matter what the value of MY_CAT is (i.e., either Summer or Shirley), the preferences of the two cats to the two plants are the same. Hence, the null hypothesis for this setting is Summer and Shirley equally like Cat grass, or more broadly, we can state that there is no association between MY_CAT and MY_PLANT. In contrast, if there is an association, we will observe difference in preferences of the two cats to the two plants. Here, we can give three different hypotheses: (1) Summer likes Cat grass more than Shirley does; (2) Summer likes Cat grass less than Shirley does; (3) At least one cat shows preference to the cat grass. The above three hypotheses correspond to right-tailed test, left-tailed test, and two-tailed test, respectively. 

For Hypothesis (1), we increase $k$ and calculate $p(k\geq 9)=\sum_{k=9}^{10}\text{Hyper}(k,N,K,n)$, where $N=18$, $K=10$, and $n=11$. The $p$-value is $p(k\geq 9)=0.0090$.

For Hypothesis (2), we decrease $k$ and calculate $p(k\leq 9)=\sum_{k=0}^{9}\text{Hyper}(k,N,K,n)=0.9997$.

For Hypothesis (3), we follow the common practice by first calculating $p(k==9)$, then sum all other $p(k)$ whose value is lower than $p(k==9)$. Hence, the $p$-value is $\sum_{k=0,p(k)\leq p(k==9)}^{10}\text{Hyper}(k,N,K,n)=0.0128$.

A common criterion for rejecting the null hypothesis is when $p$-value is lower than 0.05. So we accept Hypothesis (1) and (3) and reject the corresponding null hypothesis. We keep the null hypothesis in Hypothesis (2).


|        | Summer | Shirley  |        |
|:------:|:----:|:----:|:------:|
| Cat grass |   9  |   1  |    10   |
|  Pink rose |   2  |   6  |    8   |
|        |   11 |  7 |    18   |

## Comments
As the name suggests, Fisher's exact test calculates the exact probability of a contingency table under the null hypothesis. When the sample size is large, then it is difficult to calculate the probability. Hence, it is suggested to use Fisher's exact test when the total sample size is less than 1000, and use the chi-square or G–test for larger sample sizes [[3]][3].
Moreover, from the example above, we can conclude that there is a significant difference between the two
cats with respect to the preference to cat grass or pink rose, but we can draw no conclusions about the possible size of the difference [[4]][4].



[1]: <https://mathworld.wolfram.com/FishersExactTest.html> "Fisher's Exact Test"

[2]: https://stats.oarc.ucla.edu/other/mult-pkg/faq/general/faq-what-are-the-differences-between-one-tailed-and-two-tailed-tests/ "Single- and Two-Tailed Tests"

[3]: http://www.biostathandbook.com/fishers.html

[4]: https://www.sheffield.ac.uk/polopoly_fs/1.43998!/file/tutorial-9-fishers.pdf