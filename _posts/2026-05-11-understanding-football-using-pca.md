---
layout: post
title: "A dumb introduction to league football and Principal Components Analysis (PCA)"
date: 2026-05-11
tags: [pca, football, isl]
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

<figure>
  <img width="600" height="350" alt="5DSpace" src="https://github.com/user-attachments/assets/2ed446df-1181-4e27-badc-527bd28bcf7b" />
  <figcaption align="center">This image is AI generated</figcaption>
</figure>

Let's first look at the amount of variation in the ISL data our PCs capture. You can think of the numbers on the y-axis as R-squared we commonly use in regression models.

<img width="600" height="350" alt="ScreePlot1" src="https://github.com/user-attachments/assets/477237be-7dd3-4202-bb5d-4273c46ce16e" />

The first two PCs capture a little more than 90% of the variation. **In other words, most of the patterns in our data can be explained by just two lines, instead of five!** The remaining variance (<10%) is likely statistical noise and doesn't mean anything in real life, so we won't worry about it much.

What are the PCs composed of? Seems like a weird question to ask: they are just lines depicting the direction of maximum variation, right? Why should they be "composed of" anything?

But every PC can also be thought of as a weighted linear combination of the five axes (or our five data columns). In fact, this is what PCA does: it calculates these weights automatically from our data. We call these weights "loadings". We'll now look at the recipe for the first two PCs.

<img width="450" height="350" alt="LoadingsMatrix1" src="https://github.com/user-attachments/assets/5b5ffa4b-066a-407b-b532-38babc0cc36e" />

PC1 has a strong positive dependence on W and GF, and a strong negative dependence on L and GA. This is the strongest pattern in the league: on average, teams that score more goals and concede fewer, win more and lose less (note that the number of matches is fixed). Makes sense, right? PC1 discovers this pattern and clumps all of these four columns into a single PC.

PC2 is heavily dominated by D, the only major leftover pattern in the data. In fact, PC2 -- by nature -- isn't much different from the D column of our dataset.

Another cool geometric way to look at the (i) mutual relationship between the columns, as well as (ii) their relationship to the PCs, is the biplot. The smaller the angle between the lines, the more positively correlated they are. Interestingly, L and GA are even more positively correlated than W and GF. As the angle increases to 90°, the correlation drops to zero. For example, GF and D are at about 90° and aren't correlated. As we go past 90°, the negative correlation goes up. Lines opposite to each other (or, in other words, at an angle of 180°) are perfectly negatively correlated. In our data, teams that win more lose less. Also note that the D arrow is aligned along PC2 and almost perpendicular to PC1; the others are aligned along PC1 but perpendicular to PC2.

<img width="600" height="350" alt="LoadingsBiplot1" src="https://github.com/user-attachments/assets/350fd335-1f24-43b5-9004-2a116730a671" />

### Predicting the ISL champions

That's what there is to PCA. It identifies correlations among the columns of the data (if any) and reduces the number of columns needed to describe the major patterns in the data. In our case, we have reduced the 5D ISL data to a more manageable 2D data. But do the first two PCs have any predictive ability? Can they predict who the champions were, and who finished at the bottom?

The short answer is: not necessarily. PCA doesn't know how the league committee ranks the teams.

Nevertheless, let's calculate each team's PC scores and arrange them in descending order. More simply, let's evaluate the following equations for each team, and arrange the numbers in descending order:

*PC1 score = 0.507W + 0.036D - 0.526L + 0.480GF - 0.485GA*

*PC2 score = 0.301W - 0.925D + 0.178L + 0.077GF + 0.128GA*

We get the following table. I've also included the actual ISL standings in the last column. Yes, Mohun Bagan SG were the champions of India in the 2024-25 season!

| Standings based on PC1 | Standings based on PC2 | Real standings |
|:---|:---|---:|
| Mohun Bagan SG | Jamshedpur FC | Mohun Bagan SG |
| FC Goa | Punjab FC | FC Goa |
| NorthEast United FC | East Bengal FC | NorthEast United FC |
| Bengaluru FC | Mohun Bagan SG | Bengaluru FC |
| Mumbai City FC | Bengaluru FC | Jamshedpur FC |
| Odisha FC | Kerala Blasters FC | Mumbai City FC |
| Jamshedpur FC | FC Goa | Odisha FC |
| Kerala Blasters FC | Chennaiyin FC | Kerala Blasters FC |
| Punjab FC | Hyderabad FC | Punjab FC |
| Chennaiyin FC | Mohammedan SC | East Bengal FC |
| East Bengal FC | NorthEast United FC | Chennaiyin FC |
| Hyderabad FC | Odisha FC | Hyderabad FC |
| Mohammedan SC | Mumbai City FC | Mohammedan SC | 

Wow, PC1 is very accurate at predicting the actual standings! Is this a mere coincidence? Yes and no. We were both careful and lucky.

### Coincidence is just being careful and lucky

The league calculates the points for every team using the following equation (three points for a win, one for a draw, and none for losses or goals scored/conceded):

*point = 3W + 1D + 0L + 0GF + 0GA*

It also employs tiebreaks when two or more teams have the same points. Goal difference (GF minus GA) is the first tiebreak. If the teams are still tied, GF is taken into account.

The PC1 score equation resembles this ranking criterion. This happens partly for two reasons:

(i) We've been careful while picking the columns on which we performed the PCA. If we decided to use data columns such as mean home stadium attendance or mean grass length on the training pitch, the PCs wouldn't make much sense when it comes to predicting the final standings. We also didn't leave out any necessary column.

(ii) We're lucky that the league uses a linear combination of columns as the ranking criteria. If they used a system that's something like the one below, we'd have run into trouble, as PCA can only linearly combine the columns.

*point = 3W^3 + 1D^2 + ...*

But the resemblance is still uncanny. **Why would the direction of maximum variance be such a good predictor of the league standings?**

We've actually figured out the fundamentals of league football. To repeat our observations from the biplot: since the number of matches is fixed, a team winning essentially means a team loses. Also -- on average -- to win games, you need to have a higher GF than GA. Humans deliberately decided to set this pattern as *the* ranking criterion: teams that score more goals and concede fewer, win more and lose less -- and are essentially considered *better*.

### Epilogue

---
**Tags:** {% for tag in page.tags %} `{{ tag }}` {% endfor %}
