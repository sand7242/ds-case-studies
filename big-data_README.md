# **Spark, Big Data Case study**
# The Tipping Point
#### Christopher Lawton, Paul Sandoval, Joe Shull, Jane Stout


The data used in this case study were collected and provided to the NYC Taxi and Limousine Commission (TLC) by technology providers authorized under the Taxicab & Livery Passenger Enhancement Programs (TPEP/LPEP). The dataset includes:
- fields capturing pick-up and drop-off dates/times
- pick-up and drop-off locations
- trip distances
- itemized fares
- rate types
- payment types
- driver-reported passenger counts

## Current Analysis

We explored data on:

- Tipping behavior
- Pick up location
- Trip distance
- Payment Type

![](feb_2016_pickup_map.png)

## Samples:

We ran our analyses on a variety of subsamples. One analysis worked with ~113m rows. Another analysis worked with ~100k rows. Most analyses were run on ~9k rows.

## Analysis

We first explored the type of payment people used as a function of their pick up location (see Figure 1). We did run this analysis using Spark; the analysis was run on ~113m rows of data.

### Figure 1

![](paytypebyborough.png)

Then, we started exploring tipping behavior using a subsample of the data (~9k rows). We found that there are only data on tips for credit cards (see Figure 2). In fact, this was an artifact of the data collection methodology; tipping data were only collected for credit card transactions.

### Figure 2

![](boxplot_payment.png)

Next, we observed tipping behavior as a function of pick up location (See Figures 3 and 4). It appeared that people coming from Queens tip better than people who are picked up outside of Queens.

### Figure 3

![](violin_tip.png)

### Figure 4

![](20_largefont.png)

Table 1. Total Tips in Februrary 2016

|Tipping Behavior|Count   |
|---|---|
|tips < 20%  | 1,001,756  |
|tips >= 20%  | 504,929  |

But, people in Queens may just be tipping more because they are traveling a great distance. Indeed, Figure 5 illustrates this.

### Figure 5

![](violin_dist.png)

We ran a multiple linear regression so that we could run inferential statistics to test our hypothesis. Our first model regressed tipping on boroughs, using Queens as a refernce group. Here we see people in Queens tip significantly more than people in the other boroughs (see Model 1, Table 1).

In a second model, we included trip distance. This canceled out most of the effects that we had seen before. That is, people in Queens no longer tip more than people in most other boroughs, once we control for trip distance. This means trip distance largely explains the fact that people coming from Queens tend to tip more than people coming from other boroughs.  

### Table 1. Regression Models Predicting Tipping Behavior.
| Model 1: Train |       |      |     |        |   | Model 2: Train |       |      |      |       |
|----------------|-------|------|-----|--------|---|----------------|-------|------|------|-------|
| R2: .11        |   b   |  SE  |  p  |    t   |   | R2: .30        |   b   |  SE  |   p  |   t   |
|                |       |      |     |        |   |                |       |      |      |       |
| Constant       |  5.47 | 0.11 | .00 |  50.40 |   |       Constant |  1.11 | 0.13 | 0.00 |  8.56 |
| Trip Distance  |       |      |     |        |   |  Trip Distance |  0.39 | 0.01 | 0.00 | 50.54 |
| Bronx          | -5.27 | 0.81 | .00 |  -6.49 |   |          Bronx | -1.66 | 0.73 | 0.02 | -2.29 |
| Brooklyn       | -3.51 | 0.23 | .00 | -15.00 |   |       Brooklyn | -0.38 | 0.22 | 0.08 | -1.76 |
| Manhattan      | -3.93 | 0.11 | .00 | -35.30 |   |      Manhattan | -0.45 | 0.12 | 0.00 | -3.70 |
| Staten Island  | -5.47 | 2.41 | .02 |  -2.27 |   |  Staten Island | -1.36 | 2.15 | 0.53 | -0.63 |
| Unknown        | -3.69 | 0.22 | .00 | -16.60 |   |        Unknown | -0.37 | 0.21 | 0.08 | -1.78 |

## What did we learn?
- Just do everything in Pandas until you can not. Spark is hard and it breaks.
- When you are spinning up an AWS cluster, just write to the mnp directory -- not the mnt1 file that's show in our assignments!!  

| Filesystem |   | Size | Used | Avail | Use% | Mounted on |
|------------|---|------|------|-------|------|------------|
| devtmpfs   |   | 7.9G | 72K  | 7.9G  | 1%   | /dev       |
| tmpfs      |   | 7.9G | 0    | 7.9G  | 0%   | /dev/shm   |
| /dev/xvda1 |   | 9.8G | 6.7G | 3.0G  | 70%  | /          |
| /dev/xvdb1 |   | 5.0G | 34M  | 5.0G  | 1%   | /emr       |
| /dev/xvdb2 |   | 27G  | 4.1G | 23G   | 15%  | /mnt       |
