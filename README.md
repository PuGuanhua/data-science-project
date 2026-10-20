# NBA Claim Lab — Did High-Scoring Players Become More Common?

A small exploratory data analysis (EDA) notebook investigating whether the share of
NBA players averaging **20 or more points per game** grew across the ten regular
seasons from **2015-16 to 2024-25**.

This project was completed as **Exercise 04 ("NBA Claim Lab")** for CISC7204 at the
University of Macau, and is published as a portfolio piece.

---

## Research question

> **Claim:** *"High-scoring players became more common."*

We operationalise this as: **among eligible player-seasons, what percentage averaged
at least 20 points per game in each season, and does that percentage rise across the
ten-year window?**

Eligibility rule (a classroom choice, not an NBA official award rule):

- `games >= 40` AND `minutes_per_game >= 15`

A sensitivity check relaxes `games` to `>= 20` while keeping minutes fixed.

---

## Dataset

- **File:** `nba_player_seasons.csv`
- **Shape:** 5,386 rows × 13 columns; one row = one player in one season
  (key: `player_id` × `season`).
- **Coverage:** NBA regular seasons 2015-16 through 2024-25.
- **Columns:** `season`, `player_id`, `player`, `position`, `team_id`, `games`,
  `minutes_per_game`, `fg_pct`, `ft_pct`, `rebounds`, `assists`, `turnovers`,
  `points` (the per-game stats — `points`, `rebounds`, `assists`, `turnovers` —
  are **per-game averages**, not season totals; `fg_pct` / `ft_pct` are proportions).
- **Known gaps:** the dataset does **not** include age, wins, possessions, usage
  rate, 3-point statistics, playoffs, or individual game results.
- **Source / usage permissions:** snapshot of publicly available NBA box-score
  aggregates bundled with the CISC7204 course materials (slide03). Used here for
  non-commercial teaching and portfolio demonstration only. No personal data,
  credentials, or restricted datasets are included.

---

## Methods and tools

- **Language:** Python 3.11+
- **Libraries:** `pandas` (data loading, filtering, `groupby` aggregation),
  `matplotlib` (two labelled charts), `numpy` (bar-chart layout).
- **Workflow:**
  1. Load the CSV and inspect shape, dtypes, missing values, duplicate keys.
  2. Apply the eligibility filter and build a binary flag `points >= 20`.
  3. Aggregate by season → counts, denominators, and percentages (evidence table).
  4. Plot the ten-year trend (line chart).
  5. Independently re-verify one season (2024-25) from the filtered frame.
  6. Re-run the analysis under a relaxed `games >= 20` cutoff and plot both
     rules side by side (grouped bar chart) as a robustness check.

---

## Key findings

| Season | Eligible player-seasons | High-scorers (20+ PPG) | Share |
|---|---:|---:|---:|
| 2015-16 | 284 | 20 | **7.04%** |
| 2022-23 | 298 | 54 | **18.12%** (peak) |
| 2024-25 | 295 | 44 | **14.92%** |

- The share of eligible player-seasons reaching 20+ PPG roughly **doubled** from
  ~7% in 2015-16 to ~15% in 2024-25, peaking at 18.12% in 2022-23.
- The upward pattern is **robust to the games-played cutoff**: relaxing the rule
  from 40 to 20 games moves 2015-16 from 7.04% to 7.19% and 2024-25 from 14.92%
  to 14.12% — the verdict does not change.
- **Verdict: partly supported** — the trend is broadly upward but not perfectly
  linear, and the 2024-25 share has eased off the 2022-23 peak.

### Limitation (one thing the data cannot establish)

The dataset has **no usage rate, possessions, age, or opponent-strength fields**,
so we cannot tell whether the rising share reflects (a) a genuine league-wide
change in offensive style, (b) a different mix of players / roles across seasons,
or (c) rule changes that favour scoring. A group-level trend also does **not**
prove that individual players became better scorers.

---

## How to run

```bash
# 1. (Recommended) create an environment
python -m venv .venv
# Windows:  .venv\Scripts\activate
# macOS:    source .venv/bin/activate

# 2. Install dependencies
pip install -r requirements.txt

# 3. Launch Jupyter and run the notebook top to bottom
jupyter lab exercise04_nba_claim_lab.ipynb
```

The notebook loads `nba_player_seasons.csv` from the same folder; no configuration
or secrets are required.

---

## Project layout

```
data-science-project/
├── exercise04_nba_claim_lab.ipynb   # main analysis (run top to bottom)
├── nba_player_seasons.csv            # dataset (5,386 rows)
├── requirements.txt
└── README.md
```

---

## Declaration of AI usage

- **Did I use an AI LLM?** Yes
- **Exact LLM used:** ChatGPT (planning + critical review of results); Doubao
  (help restructuring the public README and adding the second comparison chart)
- **Web Browser used:** Google Chrome
- **What I did myself:** ran every cell, manually re-verified the 2024-25
  percentage from the filtered frame, chose the eligibility rule, wrote the
  evidence brief, and reviewed / revised all AI-generated code and wording.
