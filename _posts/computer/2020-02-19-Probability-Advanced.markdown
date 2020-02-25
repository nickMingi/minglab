---
layout: "post"
author: "mingi hong"
title: "Probability Advanced"
categories: "Computer AI"
permalink: /:categories/:title.html
---

# Craps

Basics: One player(shooter) rolls two dice

The shooter is a winner if they roll a sum of 7 or 11 (natural)

The shooter loses if they roll sum of 2, 3 or 12(craps)

If the shooter fails to roll a natural or craps, then the sum they roll becomes the so called point

The shooter then begins on a series of rolls to either roll the point again and win(or pass) or roll a 7 and lose(don't pass)

The shooter continues to roll until one of these outcomes occurs

# Street craps

Betting that the shooter wins is called betting the pass line
and betting the shooter loses is called betting the don't pass line.

Since there is no house, someone must be willing to take the bet

In street craps it is an advantage to bet against the shooter

# Casino craps

It's the common to change the initial roll of double 6 from a losing
roll to a push, neither a win or a loss. Shooter loses along with those
betting on the pass line, those betting along the don't pass line receive no payment, their bet is returned to them.

# Craps cont'd

From last time: It is common for casinos to change an initial roll of 
double 6's from a losing roll to a push, neither a win or loss - shooter
loses along with those betting the pass line, those betting the don't
pass line receive no payment, just their bet returned. This decreases the probability of winning on the don't pass line by 1/36 = 0.0278

Expected values for craps: per dollar

EV (pass line in street craps) = 0.4929(1$)+0.5071(-$1) = -0.0143

So we are losing 1 cent

EV (don't pass line in street craps) = 0.5071($1)+0.4929(-$1) = 0.0143

So we are winning 1 cent

EV (don't pass line in casino craps) = 0.0143 - 0.0278 = -0.0135

So we are losing 1 cent 

# Permutations, Combinations, and Applications

ex) Suppose you're chosen nine players on your baseball team to form a batting order. How many possible batting orders are there? 

- So there are nine choices for first spot, 
- 8 choices for second, ...
- So multiplication principle says there are 9*8...1 = 362,880 possible batting orders

ex) suppose you need to select a 9-payer batting order from
your team of 24. How many different batting orders can be selected assuming that any player can bat at any position?

24*23....16 = 474,467,051,520

A permutation is an ordered arrangement of object in a set

A combination is an arrangement of objects in a set where order does not matter

ex) How many different two-card hands are possible in Texas Holdem?

- 52 * 51
- How we can get rid of redundancy 
- Since we have two hands, divide it by 2
- 52 * 51 / 2 = 1326
- Order doesn't matter 

ex) In how many ways can the letters of me word
Mississippi be arranged?

11 total letters, and how many repetition for each letter?
- i is repeated 4 times
- s is repeated 4 times
- p is repeated twice

11*10....1

Since we've got identical letters, we have to get rid of redundancy

- 11*10...1 / 4! * 4! * 2 = 34,650

ex) In how many ways can the letters of your first name be rearranged?

- Mingi
- 5 letters
- two i's
- 5*4...1 / 2 =  60 ways

ex) In our class of 24 students, how many different trios can be formed?

- order does not matter
- Trio can be ordered in six ways
- 24*23 .. 22 / 6 = 2024 ways that trios can be formed

# Factorials and other notation

Let n be a positive integer

Then n factorial, denoted by n!, is given by n!=n(n-1)(n-2)...2*1

We have the convention 0! = 1

We can rewrite n! = n*(n-1)!



The number of permutations of n objects taken r at a time 
denoted by Pn,r is given by 

```
Pn,r = n!/(n-r)!
```

The number of combinations of n objects taken r at a time, 
denoted Cn,r is given by 

```
Cn,r = n!/(n-r)!r!
```
