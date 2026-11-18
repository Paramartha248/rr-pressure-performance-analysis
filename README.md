# Rajasthan Royals: Identifying the Most Reliable Players Under Pressure

**IPL 2027 Auction Analysis | Author: Paramartha Kumar Mohajon**

[![GitHub](https://img.shields.io/badge/GitHub-Repository-181717?logo=github)](https://github.com/Paramartha248/rr-pressure-performance-analysis)

## Project Overview

This project analyses ball-by-ball performances in **Indian Premier League (IPL) and T20 International (T20I) matches from 2022 onwards** to identify batters and bowlers who perform effectively in high-pressure situations.

The analysis develops a delivery-level pressure index using required run rate, wickets lost and innings stage. Player performances in pressure deliveries are compared with their performances in normal chase situations. The findings are intended to support Rajasthan Royals' player evaluation ahead of the IPL 2027 auction.

The primary analysis focuses on **second-innings chases**. It should therefore be interpreted as an assessment of chase-pressure performance, not as a complete measure of a player's performance in every match situation.

## Project Objectives

- Define and quantify pressure at the delivery level.
- Identify batters who score effectively under pressure.
- Identify bowlers who control scoring and take wickets under pressure.
- Compare pressure performance with normal chase performance.
- Test how player shortlists change under different minimum-sample thresholds.
- Conduct a supplementary Rajasthan Royals (RR) analysis.

## Dataset Description

The project uses the supplied ball-by-ball dataset together with match and competition metadata.

The working data includes delivery-level information such as:
- Batter and bowler details
- Runs scored and extras
- Wickets and dismissal information
- Innings and match identifiers
- Match-state information

**Competition coverage:**
- IPL seasons 2022–2026
- T20 International matches from 2022 onwards

The original source data is not included in this repository. Do not upload proprietary, confidential or third-party data unless you have permission to redistribute it.

## Methodology

### 1. Data Preparation

- IPL and T20I matches from 2022 onwards were retained.
- Only the first and second innings were used; Super Overs were excluded.
- Matches flagged with revised targets or revised overs were excluded from the primary analysis to maintain comparability with a standard 20-over chase.
- The pressure analysis uses second-innings chase deliveries.
- Completed chases, all-out innings and states with no balls remaining were excluded from the active chase-pressure dataset.

The working analysis contained 837 matches before the revised/shortened-match exclusion. Thirty-four revised or shortened matches were excluded.

The qualifying chase dataset contained **90,116 deliveries across 790 matches**.

### 2. Pressure Index

Pressure is represented by a project-defined score from 0 to 100, calculated before each delivery.

| Component | Calculation | Weight |
|---|---|---:|
| Required run-rate pressure | `min(required_run_rate, 15) / 15` | 50% |
| Wicket pressure | `wickets_lost_before_delivery / 9`, capped between 0 and 1 | 30% |
| Stage pressure | `legal_balls_bowled_before_delivery / 120`, capped between 0 and 1 | 20% |

**Pressure Score:**

`100 × (0.50 × RRR pressure + 0.30 × wicket pressure + 0.20 × stage pressure)`

Classification:
- **Pressure:** Score greater than or equal to 60.
- **Normal:** Score below 60.

The weights, required-run-rate cap and threshold are analytical assumptions for this project, not an official cricket standard.

### 3. Batting Evaluation

Batting strike rate is calculated as:

`Batter runs / balls faced × 100`

The ranking uses two indicators:

1. Strike rate in pressure deliveries.
2. Strike-rate improvement: `Pressure strike rate − Normal strike rate`.

Both indicators are standardised using z-scores and combined with equal weights.

**Raw batting score:**

`0.50 × z(Pressure strike rate) + 0.50 × z(Strike-rate improvement)`

A reliability adjustment is applied:

`Pressure balls / (Pressure balls + 60)`

The final batting score is the raw score multiplied by this reliability adjustment.

**Eligibility:** At least 60 pressure balls and 30 normal balls.

Sensitivity checks were conducted at 30, 60 and 90 pressure balls.

### 4. Bowling Evaluation

Bowling economy is calculated as:

`Bowler runs conceded / Legal balls × 6`

Wicket-taking rate is calculated as:

`Bowler wickets / Legal balls × 100`

The ranking uses:

1. Economy improvement: `Normal economy − Pressure economy`.
2. Wicket-rate improvement: `Pressure wickets per 100 balls − Normal wickets per 100 balls`.

Both indicators are standardised using z-scores and combined with equal weights.

**Raw bowling score:**

`0.50 × z(Economy improvement) + 0.50 × z(Wicket-rate improvement)`

The reliability adjustment is:

`Pressure legal balls / (Pressure legal balls + 60)`

The final bowling score is the raw score multiplied by this reliability adjustment.

**Eligibility:** At least 90 pressure legal balls and 30 normal legal balls.

Sensitivity checks were conducted at 60, 90 and 120 pressure legal balls.

## Key Results

The qualifying chase dataset contained:

| Delivery category | Deliveries | Share |
|---|---:|---:|
| Pressure | 25,549 | 28.35% |
| Normal | 64,567 | 71.65% |
| **Total** | **90,116** | **100%** |

The final player rankings are provided in the corresponding CSV outputs.

Rankings should be interpreted alongside the underlying pressure-ball sample sizes and eligibility thresholds.

## Rajasthan Royals Supplementary Analysis

A separate analysis isolates deliveries where Rajasthan Royals were batting and where RR were bowling.

RR team ID 3 was used for this analysis. It applies the same chase-only pressure labels as the main analysis.

### Batting Observations

- **Dhruv Jurel:** 335 pressure runs from 174 balls; pressure strike rate 192.53 versus normal strike rate 131.73.
- **Sanju Samson:** 184 pressure runs from 92 balls; pressure strike rate 200.00.
- **Shimron Hetmyer:** 374 pressure runs from 224 balls; pressure strike rate 166.96.
- **Riyan Parag:** 186 pressure runs from 108 balls; pressure strike rate 172.22.

Jos Buttler's pressure strike rate was 202.63 from 38 balls in one pressure match, so this should be treated as a small-sample observation.

### Bowling Observations

- **Brijesh Sharma:** Economy improvement of 2.54 runs per over, based on 63 pressure legal balls across six matches.
- **Yuzvendra Chahal:** 231 pressure legal balls across 19 matches. His pressure economy was 7.71 compared with 7.35 in normal situations, while his wicket-taking rate improved by 9.14 per 100 balls.
- **Jofra Archer:** Showed improvement in both economy and wicket-taking rate in the observed sample.

These are descriptive comparisons, not definitive assessments of player quality or predictions of future performance.

## Visualisations

The project includes three Rajasthan Royals supplementary charts.

### 1. Batting Performance Under Pressure

![RR batting pressure comparison](outputs/rr_analysis/charts/rr_batting_pressure_chart.png)

Positive values indicate a higher strike rate in pressure deliveries than in normal chase deliveries.

### 2. Bowling Economy Under Pressure

![RR bowling economy comparison](outputs/rr_analysis/charts/rr_bowling_economy_chart.png)

Positive values indicate a lower economy rate under pressure than in normal chase deliveries.

### 3. Wicket-Taking Under Pressure

![RR wicket-taking comparison](outputs/rr_analysis/charts/rr_bowling_wicket_rate_chart.png)

Positive values indicate a higher wicket-taking rate per 100 legal balls under pressure.

## Tools and Technologies

- Python
- Pandas
- NumPy
- Matplotlib
- Jupyter Notebook
- Git and GitHub

## Limitations and Assumptions

- The analysis focuses only on second-innings chases; it does not measure pressure while setting a target or defending one.
- Pressure weights, the required-run-rate cap and the classification threshold are project assumptions. Different choices may change the classification and rankings.
- Revised or shortened matches flagged in the supplied metadata were excluded.
- Minimum sample thresholds and reliability adjustments reduce, but do not eliminate, small-sample uncertainty.
- Z-score-based rankings are relative to the eligible player pool and are not probabilities or absolute skill ratings.
- Auction price, availability, injury history, age, role scarcity and squad balance are not included.
- Results are descriptive and should not be interpreted as causal evidence or a guarantee of future performance.

## Project Report

The accompanying methodology report documents the data preparation, pressure index, ranking methods, assumptions, limitations and Rajasthan Royals supplementary findings.

## Author

**Paramartha Kumar Mohajon**

GitHub: [Paramartha248](https://github.com/Paramartha248)

Project Repository: [RR Pressure Performance Analysis](https://github.com/Paramartha248/rr-pressure-performance-analysis)
