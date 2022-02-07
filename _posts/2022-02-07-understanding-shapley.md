---
layout: post
title: Understanding Shapley Value
---

The Shapley value for player $$i$$ is defined as 

$$\phi(i)(v)=\sum_{S\subseteq N\backslash\{i\}}\frac{|S|!(n-|S|-1)!}{n!}(v(S\cup\{i\}-v(S)),$$

where $$v(\cdot)$$ is a value function, $$N=\{1,\ldots,n\}$$ is the set of all $$n$$ players, 
the Shapley value $\phi(i)(v)$ is the average return for player $$i$$ when it cooperates with other players under the value function $$v$$. 
The difference $v(S\cup\{i\})-v(S)$ is the net gain obtained when player $$i$$ cooperate with players in the set $$S$$.

All the symbols and notations make sense except the weight before the difference term. The weight at first glance is hard to understand. 
However, if we reformulate it a little bit as follows

$$\frac{|S|!(n-|S|-1)!}{n!}=\frac{|S|!(n-1-|S|)!}{n(n-1)!}=\frac{1}{n}\cdot\frac{1}{C_{n-1}^{|S|}},$$

then it becomes much easier to digest.

Now if we rewrite the summation over all subsets of $$N\backslash \{i\}$$ into the following and plug in the new form of the weight, we can get

$$\phi(i)(v)=\frac{1}{n}\sum_{m=0}^{n-1}\sum_{S\subseteq N\backslash\{i\},|S|=m}\frac{1}{C_{n-1}^{|S|}}(v(S\cup\{i\}-v(S)).$$

Let us define 

$$\phi(i)(v,m)=\sum_{S\subseteq N\backslash\{i\},|S|=m}\frac{1}{C_{n-1}^{|S|}}(v(S\cup\{i\}-v(S)),$$

then the Shapley value for player $$i$$ is

$$\phi(i)(v)=\frac{1}{n}\sum_{m=0}^{n-1}\phi(i)(v,m).$$

Let's look closely at what $$\phi(i)(v,m)$$ actually represents: it is the average net gain when player $$i$$ joins a group of $$m$$ players and cooperate with
them. Then, the Shapley value for player $$i$$ is the average net gain when player $$i$$ joins all kinds of groups, ranging from zero player to $$n-1$$ players. 
So, at high-level, the Shapley value calculates the average net gain from cooperation at different scales.

Here is a simple example, where we have three players $x_1,x_2,x_3$. Then the Shapley value for player $$x_i$$ can be calculated as follows:
- $$x_1$$ playes sole, $$\phi(x_1)(v,0)=v(x_1)-v(\emptyset)$$
- $$x_1$$ cooperates with one player. There are two options: cooperate with $$x_2$$ or $$x_3$$, then 
  $$\phi(x_1)(v,1)=\frac{1}{2}(v(x_1,x_2)-v(x_2))+\frac{1}{2}(v(x_1,x_2)-v(x_2))$$
- $$x_1$$ cooperates with two players. 
  $$\phi(x_1)(v,2)=v(x_1,x_2,x_3)-v(x_2,x_3)$$
  
Summing these values together and average them, we get 

  $$\phi(x_1)(v)=\frac{1}{3}(\phi(x_1)(v,0)+\phi(x_1)(v,1)+\phi(x_1)(v,2))$$
  
It is easy to verify that the weight before each difference term is exactly the weight in the standard Shapley formula.

Shaley value can be applied to many areas. In natural language processing, we can use it as a model interpretation tool. 
For example, in a text classification task, we can use Shapley value to characterize the contribution of each input word to the final prediction, if we
consider each input word as a player.
