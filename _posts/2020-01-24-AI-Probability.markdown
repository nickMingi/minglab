---
layout: "post"
title: "Dice, Cards, and Probabilities"
author: "mingi hong"
date: 2020-01-24 11:00:59 -0700
categories: AI Probability
permalink: /:categories/:title.html
---

## AI Basic Probability

# Probability Exercise

1. Dice Problems

![ProbabilityExercise1](/minglab/assets/ProbabilityExercise1.png)
![ProbabilityExercise2](/minglab/assets/ProbabilityExercise2.png)
![ProbabilityExercise3](/minglab/assets/ProbabilityExercise3.png)

2. Card Problems
- 52 cards in a standard deck
- 4 suits: spades,clubs,hearts,diamonds
- 13 cards of each suit
- 4 cards of each: Ace,2,...,10,Jack,Queen,King

We will assume perfect shufflling and honest dealing

When being dealt cards without seeing any of the cards,
The probability of being dealt:
- The ace of hearts 1/52
- heart 13/52 = 1/4
- Jack 4/52 = 1/13

When cards are exposed, this affects the probability of drawing the next card as cards are drawn without replacement.

ex) Suppose the first card dealt without replacement is the 4 of clubs. What is the probability that second card dealt is:
- a club 12/51
- a four 3/51

ex) What is the probability of the following:
- getting a 7 or a heart in a single draw
    - 4/52 + 12/52 (since we have heart 7)
- If 11 cards have been seen, exactly one of which is a 5, what is the probability of the next being a 5?
    - 3/41
- Being dealt a jack and then a second jack in two consecutive cards?
    - 4/52*3/51 = 1/221
