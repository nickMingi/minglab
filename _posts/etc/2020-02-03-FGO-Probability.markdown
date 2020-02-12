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

*****(0.043%) 
- 23        -> 1% and 1/23%
****(0.060%)
- 50        -> 3% and 1/50%
***(1.081%)
- 37        -> 40% and 1/37%

# Craft

*****(0.235%)
- 17        -> 4% and 1/17%
****(0.480%)
- 25        -> 12% and 1/25%
***(1.818%)
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
    - P(Guaranteed card as ***** servant) * (P(***** servant)^9)
        - Sample Space for Guaranteed card : 15
        - 1:3:4:12
        - 5:15:20:60
        - P(guaranteed card as ***** servant) : 5%
    - P(***** servant) : 0.043% -> specific one character
        - But I'd like to know ***** servant in general -> 1%
    - multiply it together : 0.05%
    - So, If you roll 20000 times, you would get 2 ***** servants in a roll.
2. What is the probability of not getting ***** servants or crafts at all in a roll(10).
    - P(Guaranteed card as not ***** card) * (P(Not ***** card)^9)
        - We want to inversely caculate it like 1-P = A
        - P(Guaranteed card as ***** card) * (P(At least one ***** card)^9)
        - 20% * (1-(.95)^9)
        - 20% * 37%
        - 100% - (7.4%) : 92.6%
    
