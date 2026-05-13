---
layout: post
title: "A dumb introduction to league football and PCA"
date: 2026-05-11
---

### Prologue

This blog post works best for those who know nothing about league football. In fact, the less you know about how a football league works, the fewer preconceived notions you'll have about it, and the grander the outcomes of our statistical analysis would sound.

But this post is not just about football leagues. In the end, it is for those who (like me) are starting to analyze multidimensional data (or, more simply, data with many columns). Let's get started by looking at our data.

### The ISL 2024-25 data

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
| GA | Goals Against (Goals conceded by the team) |
| GD | Goal Difference (GF minus GA) |

### The objective

Which team do you think won the ISL? The one with the most W? Or maybe, the one with the least GA? In the end, it all depends on how the league committee ranks the teams. It can also be something non-intuitive and bizarre (only to those who *know* league football), like the team with the fewest Ds would be crowned champions.

Our goal here is to figure out the ranking criteria that the league committee uses to decide the final standings.

### Getting the data ready

There are 13 teams, and every team played 24 matches. Apart from the MP column (which is the same across all teams), we have six columns. Are all of them independent of each other?

Well, at least one is not. GD is a linear combination of GF and GA, and we'll leave it for our further analysis. Thus, the (apparently) 5D independent data looks like:

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

Let's look at the numbers in each column. Every column has a different range. For example, W can only have values between 0 and 24 (a team can win none or all of their matches). We can apply the same logic to L and D as well. However, there's no theoretical lower or upper limit to GF and GA. Broadly speaking, they are higher than W, L, or D in our data. Throwing columns with very different value ranges into a *variance-finder* algorithm like PCA confuses it, as it tries to over-value the columns with higher magnitudes. We'll be careful about this and scale the numbers in a way that we're only dealing with "how" they are distributed, and not their actual magnitudes.

The league table now looks like the one below. A value near 0.000 means it is close to the league average.

| Club | W | D | L | GF | GA |
|:---|---:|---:|---:|---:|---:|
| Bengaluru FC | 0.510 | -0.433 | -0.286 | 0.567 | -0.433 |
| Chennaiyin FC | -0.551 | 0.079 | 0.510 | -0.047 | 0.568 |
| East Bengal FC | -0.286 | -0.944 | 0.775 | -0.764 | -0.183 |
| FC Goa | 1.305 | 0.079 | -1.346 | 0.875 | -0.933 |
| Hyderabad FC | -1.346 | 0.079 | 1.305 | -1.276 | 1.568 |
| Jamshedpur FC | 0.775 | -1.967 | 0.245 | 0.260 | 1.068 |
| Kerala Blasters FC | -0.286 | -0.433 | 0.510 | -0.150 | 0.317 |
| Mohammedan SC | -1.876 | 0.590 | 1.570 | -2.301 | 1.068 |
| Mohun Bagan SG | 2.101 | -0.433 | -1.876 | 1.284 | -2.309 |
| Mumbai City FC | -0.020 | 1.613 | -0.816 | -0.559 | -0.808 |
| NorthEast United FC | 0.245 | 1.102 | -0.816 | 1.182 | -0.683 |
| Odisha FC | -0.286 | 1.613 | -0.551 | 0.977 | 0.317 |
| Punjab FC | -0.286 | -0.944 | 0.775 | -0.047 | 0.443 |

### PCA-ing

Imagine a 5D space. It's not easy to imagine one, but we'll have to pretend that we can for a while. Each dimension of this 5D space is defined by an axis that corresponds to a column in our data, and each team is a data point. The idea is to draw lines through the data cloud along which the cloud is the widest, or, in other words, *varies* the most. These lines need to be orthogonal to each other so that they represent variations originating from completely independent factors. We call these lines Principal Components, or simply, PCs.

PC1 depicts the direction along which the cloud is the widest, PC2 depicts the direction along which the cloud is the second-widest, and so on. Can you guess the directions along which the first two PCs might be oriented in the image below?

<img width="600" height="400" alt="ChatGPT Image May 13, 2026, 03_35_57 PM" src="https://github.com/user-attachments/assets/9df7a4bf-8cb5-487a-a274-4e257c2f7e67" />

Let's first look at how much variation in the ISL data our PCs capture.

<img width="800" height="477" alt="ScreePlot" src="https://github.com/user-attachments/assets/f5d1f417-3a01-40f9-b46c-8a5906791319" />

