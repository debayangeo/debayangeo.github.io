---
layout: post
title: "A dumb introduction to league football: Using PCA to understand how a football league works"
date: 2026-05-11
---

### Prologue

This blog post works best for those who know nothing about league football. In fact, the less you know about how a football league works, the fewer preconceived notions you'll have about it, and the grander the outcomes of our statistical analysis would sound.

But this post is not just about football leagues. In the end, it is for those who (like me) are starting to analyze multidimensional data (or, more simply, data with many columns). Let's get started by looking at our data.

### The ISL data

The table below shows the outcomes of the 2024-25 season of the Indian Super League (ISL). The teams are arranged in alphabetical order of names.

| Club                |   MP |   W |   D |   L |   GF |   GA |   GD |
|:--------------------|-----:|----:|----:|----:|-----:|-----:|-----:|
| Bengaluru FC        |   24 |  11 |   5 |   8 |   40 |   31 |    9 |
| Chennaiyin FC       |   24 |   7 |   6 |  11 |   34 |   39 |   -5 |
| East Bengal FC      |   24 |   8 |   4 |  12 |   27 |   33 |   -6 |
| FC Goa              |   24 |  14 |   6 |   4 |   43 |   27 |   16 |
| Hyderabad FC        |   24 |   4 |   6 |  14 |   22 |   47 |  -25 |
| Jamshedpur FC       |   24 |  12 |   2 |  10 |   37 |   43 |   -6 |
| Kerala Blasters FC  |   24 |   8 |   5 |  11 |   33 |   37 |   -4 |
| Mohammedan SC       |   24 |   2 |   7 |  15 |   12 |   43 |  -31 |
| Mohun Bagan SG      |   24 |  17 |   5 |   2 |   47 |   16 |   31 |
| Mumbai City FC      |   24 |   9 |   9 |   6 |   29 |   28 |    1 |
| NorthEast United FC |   24 |  10 |   8 |   6 |   46 |   29 |   17 |
| Odisha FC           |   24 |   8 |   9 |   7 |   44 |   37 |    7 |
| Punjab FC           |   24 |   8 |   4 |  12 |   34 |   38 |   -4 |


| Abbreviation | Meaning |
| :--- | :--- |
| MP | Matches Played |
| W | Wins |
| D | Draws |
| L | Losses |
| GF | Goals For (Goals scored by the team) |
| GA | Goals Against (Goals scored by opponents) |
| GD | Goal Difference (GF minus GA) |

### The objective

Which team do you think won the ISL? The one with the most W? Or maybe, the one with the least GA? In the end, it all depends on how the league committee ranks the teams. It can also be something non-intuitive (only to those who *know* league football), like the team with the least D would be crowned the champions.

Our goal here is to figure out the ranking criteria that the league committee uses to decide the final standings.

### Preprocessing the data

There are 13 teams, and every team played 24 matches. Apart from the MP column (which is the same across all teams), we have six columns. Are all of them independent of each other?

Well, at least one is not. GD is a linear combination of GF and GA, and we'll leave it for our further analysis. Thus, the (apparently) independent data looks like:

| Club                |   W |   D |   L |   GF |   GA |
|:--------------------|----:|----:|----:|-----:|-----:|
| Bengaluru FC        |  11 |   5 |   8 |   40 |   31 |
| Chennaiyin FC       |   7 |   6 |  11 |   34 |   39 |
| East Bengal FC      |   8 |   4 |  12 |   27 |   33 |
| FC Goa              |  14 |   6 |   4 |   43 |   27 |
| Hyderabad FC        |   4 |   6 |  14 |   22 |   47 |
| Jamshedpur FC       |  12 |   2 |  10 |   37 |   43 |
| Kerala Blasters FC  |   8 |   5 |  11 |   33 |   37 |
| Mohammedan SC       |   2 |   7 |  15 |   12 |   43 |
| Mohun Bagan SG      |  17 |   5 |   2 |   47 |   16 |
| Mumbai City FC      |   9 |   9 |   6 |   29 |   28 |
| NorthEast United FC |  10 |   8 |   6 |   46 |   29 |
| Odisha FC           |   8 |   9 |   7 |   44 |   37 |
| Punjab FC           |   8 |   4 |  12 |   34 |   38 |

Let's look at the numbers in each column. Every column has a different range. For example, W can only have values between 0 and 24 (a team can win none or all of their matches). We can apply the same logic to L and D as well. However, there's no theoretical lower or upper limit to GF and GA. Broadly speaking, they are higher than W, L, or D in our data. Throwing columns with very different value ranges into a variance-finder algorithm like PCA confuses it, as it tries to over-value the columns with a higher range. We'll be careful about this and scale the numbers in a way that we're only dealing with "how" they are distributed over their range, and not their actual values.
