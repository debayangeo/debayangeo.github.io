---
layout: post
title: "A dumb introduction to league football: Using PCA to understand how a football league works"
date: 2026-05-11
---

This blog post works best for those who know nothing about league football. In fact, the less you know about how a football league works, the fewer preconceived notions you'll have about it, and the grander the outcomes of our statistical analysis would sound.

But this post is not just about football leagues. In the end, it is for those who (like me) are starting to analyze multidimensional data (or, more simply, data with many columns). Let's get started by looking at our data.

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
| **MP** | Matches Played |
| **W** | Wins |
| **D** | Draws |
| **L** | Losses |
| **GF** | Goals For (Goals scored by the team) |
| **GA** | Goals Against (Goals scored by opponents) |
| **GD** | Goal Difference (GF minus GA) |
