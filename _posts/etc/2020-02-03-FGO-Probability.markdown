---
layout: "post"
title: "Fate Grand Order Probability"
author: "mingi hong"
date: 2020-02-03 12:00:23 -0700
categories: "etc FGO"
permalink: /:categories/:title.html
---

## FGO Probability

# Summoning Rates

![FGOProbability](/minglab/assets/FGOSummon.png)

Total of 100%

5% for 5 stars(20:80)

15% for 4 stars(20:80)

80% for 3 stars(50:50)

# Servant

5star(0.043%) 
- 23        -> 1% and 1/23%

4star(0.060%)
- 50        -> 3% and 1/50%

3star(1.081%)
- 37        -> 40% and 1/37%

# Craft

5star(0.235%)
- 17        -> 4% and 1/17%

4star(0.480%)
- 25        -> 12% and 1/25%

3star(1.818%)
- 22        -> 40% and 1/22%

# Condition
1. One roll is ten tries
2. At least one card is above ****(Servant or craft)
3. Other nine tries are equal probabilities
4. Sample space is 174

# How do we calculate?
1. We have two conditions
    - Guaranteed card
    - Equal probabilities
2. Calculate each conditions

# Exercise
1. What is the probability of getting two ***** servants in a roll(10). 
    - P(Guaranteed card as 5star servant) * (1-P(No 5star servant)^9)
        - Sample Space for Guaranteed card : 15
        - 1:3:4:12
        - 5:15:20:60
        - P(guaranteed card as 5star servant) : 5%
    - 1-P(No ***** servant) 
        - No ***** servant is 99%
        - we've got 9 chances
        - 0.99^9
        - 0.91
        - 1-0.91 : 0.09 : 9%
    - multiply it together : 0.45%
2. What is the probability of not getting ***** servants or crafts at all in a roll(10).
    - P(Guaranteed card as No 5star card) * (P(No 5star card)^9)
        - We want to inversely caculate it like 1-P = A
        - P(Guaranteed card as 5star card) * (P(At least one 5star card)^9)
        - In other words, 1-P(No 5star card)^9
        - 20% * (1-((.95)^9))
        - 20% * 37%
        - 100% - (7.4%) : 92.6%
    
