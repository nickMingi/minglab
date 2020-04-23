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
denoted by Pn,r is given by (order matter)

```
Pn,r = n!/(n-r)!
```

The number of combinations of n objects taken r at a time, 
denoted Cn,r is given by (order doesn't matter)

```
Cn,r = n!/(n-r)!r!
```

ex) In the game of pinochle, each of the 4 players are dealt 12 cards from a 48-card deck. How many possible pinochle hands are there?

Order doesn't matter

C48,12 = 48!/(48-12)!12! = 48!/36!12! = 48*47.....37/12!

= 69,668,534,468

ex) How many ways are there to arrange a pinochle hand from left to right?

Order does matter

P12,12 = 12!/(12-12)! = 12!/0! = 12! = 479,001,600

ex) Your basketball team has twelve players: five guards, five forwards, and two centers. If you are going to start two guards, two forwards, and one center, how many starting lineups are possible?

Order doesn't matter

C5,2 times C5,2 times C2,1 = 5!/3!2! times 5!/3!2! times 2!/1!1!

= 10 times 10 times 2 = 200

ex) How many different five card poker hands are possible?

Order doesn't matter

C52,5 = 52!/(52-5)!5! = 2,598,960

# Probabilities in Poker

Recall, there are C52,5 = 2,598,960 different five card poker hands

ex) 5-card poker hands and their probabilities 

a) straight flush(5 cards in a row of the same suit but no wrap around)

number of hands = 4(# of straight flushes per suit)

= 40 

P(straight flush) = 40/C52,5 = 40/2,598,960 = 0.0000154

b) full house (3 of a kind and 2 of a kind)

number of hands = C13,1 . C12,1 . C4,3 . C4,2

= 13 . 12 . C4,3 . C4.2 = 3744

P(full house) = 3744 / C52,5 = 0.0014406

c) flush(not straight flush)(5 cards of the same suit)

Number of hands = 4 . C13,5 - 40(Straight flushes) = 5108

P(flush that is not a straight flush) = 5108 / C52,5 = 0.00196540

d) Straight(not a straight flush)

Number of hands = 10 * C4,1 . C4,1 . C4,1 . C4,1 - 40(Straight flushes) = 10,200

P(straight but not flush) = 0.0039246

e) 4 of a kind (aaaab)
 
number of hands to start

C13,1 . C4,4 . C48,1 = 624 hands

P(4 of a kind) = 624/C52,5 = 0.0002401

f) One pair, no better (aabcd)

number of hands = C13,1 . C4,2 . C12,3 . C4,1 . C4,1 . C4,1 = 1,098,240

P(one pair, no better) = 1098240/C52,5 = 0.4225690

Suppose you hold two aces, two queens, and an 8 in a 5-card poker game with one other player. Upon replacing one card, what is the probability that you will make a full house?

P(full house) = 4/47


# Review

If the numbers 3 and 4 appear consecutively(meant 3 and 4 must be together but can appear as 34 or 43) - Homework4

# Keno Type Games

Players select or are given a set of numbers drawn without replacement from a larger set of numbers

A player receives a card with the numbers 1-80 on it. Typically, a player marks 10 numbers on their card called a 10-spot ticket.

Twenty numbers are randomly drawn by the house.

If enough of their marked numbers are drawn, the player gets a payoff

C80,10 = 1646492119120 (P of how many different ways)

C20,10 = 184756 (P of how many ways all 10 marked can be drawn)

184756 / C80,10 = 0.0000001122

So, let's calculate P of having 9 marked numbers drawn

The number of ways this can happen is 

C20,9 * C60,1 = 10077600

10077600 / C80,10 = 0.0000061206 

8: C20,8 * C60,2 = 222966900

P(8) = 0.0001354194

7: C20,7 * C60,3 = 2652734400

P(7) = 0.0016111431

6: C20,6 * C60,4 = 18900732600

P(6) = 0.0114793946

5: C20,5 * C60,5 = 84675282048

P(5) = 0.0514276877

Total # of ways: 106461978304

P of receicing a payoff = 0.0646598776

P of losing your dollar is about 1 - 0.065 = 0.935

Let's calculate expected value

EV($1 Keno bet) = (P(10))(10000) + (P(9))(2600) + (P(8))(1300) + (P(7))(180)
 + (P(6))(18) + (P(5))(2) - 1

= -0.2074

What about we change rule as n-spot ticket

Suppose, we are matching 9

C20,k * C60,n-k / C80,n


# Bingo

Since order within columns matters for the purpose of lining up rows, we need permutations

Number of Bingo cards: P15,5 times P15,5 times P15,4 times P15,5 times P15,5

We have over counted a bit here. Swaping rows will not affect game here

EX) rows 1 and 2 with 5 and 4 so

Number of essentially different bingo cards

1/2 * above calculation

Exercise

a. In the first five draws of the 75 numbers in Bingo how many different sets of 5 numbers can be drawn? C75,5 = 17259390

b. By counting the number of ways to win on a particular Bingo card with five in a row, compute the probability of winning with a single card after just 5 numbers have been drawn.

4(C71,1) + 8 / C75,5

292 / 17259390

= 0.0000169183

# Probability

What is the probability of getting exactly three tails if I flip a fair coin eight times?

HHTHHTHT, We need to count all such outcomes

(1/2)^8

(1/2)^3 * (1/2)^5

P(exactly three tails) = C8,3 times (1/2)^3 times (1/2)^5

Determine the probability of obtaining exactly r tails in eight flips of a fair coin, for each r where 0 <= r <= 8

Number of Tails| Number of ways | Probability
----------------------------------------------
0              | C8,0           |    1/256
1              | C8,1           |    1/32
2              | C8,2           |    28/256
3              | C8,3           |    56/256
4              | C8,4           |    70/256
5              | C8,5           |    56/256
6              | C8,6           |    28/256
7              | C8,7           |    8/256
8              | C8,8           |    1/256

When 0 <= r <= 8

P(exactly r tails in 8 flips) = C8,r times (1/2)^r times (1/2)^8-r

When 0 <= r <= n

P(exactly r tails in n flips) = Cn,r times (1/2)^r times (1/2)^n-r

# Exercise

An honest die is rolled 10 times. Compute the probability that a 3 or 4 will turn up 5 or more times.

P = P(3 or 4) = P(3) + P(4) = 2/6 = 1/3

Q = 1 - P = 2/3

P(5 or more successes) = 1-(P(4)+P(3)+P(2)+P(1)+P(0))

= 1 - (C10,4.(1/3)^4.(2/3)^6 + C10,3.(1/3)^3.(2/3)^7 + C10,2.(1/3)^2.(2/3)^8 + C10,1.(1/3)^1.(2/3)^9 + C10,0.(1/3)^0.(2/3)^10)

= .213128

Flipping a honest coin 15 times compute the probability tails appears 12 or less times.

= 1 - (P(13) + P(14) + P(15))

= 1 - (C15,13 times . (1/2)^13 . (1/2)^2 + C15,14 . (1/2)^14 . (1/2)^1 + C15,15 . (1/2)^15 . (1/2)^0 )


# Exam3

