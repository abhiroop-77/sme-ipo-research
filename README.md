# SME IPO Research

End-to-end research on Indian SME IPOs (NSE Emerge and BSE SME): how they perform after listing, who the anchors and BRLMs are, and whether weak starters get lifted around lock-in expiry dates (Day 30, 90, 180, 365). R analysis, Python notebooks, Excel tools and interactive HTML dashboards.

> **Status:** work in progress (last updated 31 Oct 2026). Sections marked `TO FILL` are completed from the R and dashboard work.
> **Not investment advice.** This is research on historical data. Small samples, see [Limitations](#limitations).

---

## Contents
1. [Why this project](#why-this-project)
2. [Research questions](#research-questions)
3. [Background: how SME IPOs and lock-ins work](#background-how-sme-ipos-and-lock-ins-work)
4. [Repository structure](#repository-structure)
5. [Data](#data)
6. [Methods](#methods)
7. [Findings so far](#findings-so-far)
8. [How to reproduce](#how-to-reproduce)
9. [Limitations](#limitations)
10. [Roadmap](#roadmap)
11. [Sources, licence and contact](#sources-licence-and-contact)

---

## Why this project
SME IPOs are small, thinly traded and often priced aggressively. Some large investors (anchors) participate in issues that look unattractive on listing performance alone. The project asks what the data say about who invests, how prices behave afterwards, and whether the price path around lock-in expiries looks different from ordinary drift.

## Research questions
1. **Performance:** how do SME IPOs perform vs issue price at Day 1, 7, 30, 60, 90, 100, 365 and 400, by sector, BRLM and anchor?
2. **Participants:** which anchors and BRLMs appear most often, and how do their issues perform? (leaderboards)
3. **Pump and dump hypothesis (new module):** do IPOs that stay close to issue price on Day 1 and drift lower over Days 7 to 15 tend to rise into the Day 30 to 35 window (anchor 50% unlock), and does the same repeat at Day 90, 180 and 365?
4. **Conditional probabilities:** of the IPOs that show the early-weak pattern, what share recover, hold, fade or collapse, and are anchor-backed issues different from issues with no anchors?
5. **Entry timing:** is entering near Day 7 to 15 a good entry price, or does the weak start continue?

## Background: how SME IPOs and lock-ins work
Lock-in periods as recorded in the author's tracker (verify against current SEBI ICDR rules before relying on them):

| Day (after listing) | Event | Who is released |
|---|---|---|
| about 30 | Anchor lock-in, first tranche | 50% of anchor shares |
| about 90 | Anchor lock-in, second tranche | remaining 50% of anchor shares |
| about 180 | Pre-IPO lock-in | non-promoter pre-IPO shareholders |
| about 365 | Promoter lock-in | promoter holding in excess of the minimum contribution |

BRLMs (book running lead managers) carry no lock-in of their own. Any link between a BRLM and post-listing price support would run through anchor allotment or market making. The master database has no market-maker column, so that link is not tested yet.

## Repository structure
The repository follows the order the research was done in. Each folder has its own short README.
```
sme-ipo-research/
├── README.md                          this file
├── 01_database/                       the master SME IPO database and what each sheet contains
├── 02_tool/                           the performance and leaderboard tool built on the database
├── 03_r_analysis/                     R scripts and outputs
└── 04_pump_dump_hypothesis/           the lock-in / pump-and-dump study
    ├── findings.md                    detailed results
    ├── methodology.md                 definitions, thresholds, day convention, tests
    ├── notebook/                      Python notebook: downloads prices, builds day-points, clusters, tests
    ├── dashboard/                     interactive HTML dashboard
    ├── workbook/                      Excel workbook with live formulas
    ├── outputs/                       Day_Points_Returns.xlsx, Clusters_and_Probabilities.xlsx
    └── figures/                       event study and lookalike charts
```

## Data
- **Master database** (`01_database/SME_IPO_Database_v8.xlsx`): about 450 SME IPOs listed from January 2025 to November 2026 with issue price, listing date, BRLM, anchor names and day-N closes (Day 1, 7, 30, 35, 60, 90, 100, 365, 400). Its sheets include the main database, a performance calculation tool, and anchor, BRLM and sector leaderboards.
- **Daily prices:** NSE and BSE bhavcopy files, downloaded by the notebook and cached locally (public exchange data). Alphabetic tickers are matched on NSE and numeric scrip codes on BSE.
- **Day convention:** Day N is the exact calendar day N after listing. If the market was shut that day, the next trading day within 10 calendar days is used.
- **Returns:** always % vs issue price unless stated.
- Details: [`01_database/README.md`](01_database/README.md).

## Methods
Full definitions are in [`04_pump_dump_hypothesis/methodology.md`](04_pump_dump_hypothesis/methodology.md). In short:

- **Early-weak cohort:** Day-1 return within +/-10% of issue, and Day-7 and Day-15 returns at or below 0%.
- **Window 1 (W1) pump:** the best close on Day 30 or 35 is at least 10% above the Day-7 close. Fate is judged at Day 100 against the W1 peak: Sustained (within 10%), Fade, or Collapse (below -35%).
- **Window 2 and 3:** the same test around Day 90 to 100 (underwater at Day 60) and Day 365 to 400 (underwater at Day 100, lift of at least 25%).
- **Conditional probabilities** with 95% Wilson confidence intervals; **Fisher exact test** for anchor-backed vs non-anchor pump rates; **placebo windows** and an **event study** around the Day-30 unlock.
- **Template and lookalikes:** the cluster's average path, with a +/- band (default 10 return points), and a search for other IPOs that stay inside the band at 80% or more of the day-points.
- Thresholds are parameters, and a sensitivity table shows how results move with them.

## Findings so far
Results of the notebook run on 31 Oct 2026 with default thresholds (435 of 450 IPOs complete; the rest are recent listings or have no exchange data). Full tables and caveats: [`04_pump_dump_hypothesis/findings.md`](04_pump_dump_hypothesis/findings.md).

| | Result |
|---|---|
| Early-weak cohort | 88 of 441 IPOs (20%) |
| W1 pump among early-weak | 9 of 84 (10.7%, 95% CI 5.7 to 19.1) |
| Anchor-backed vs non-anchor | 8/56 (14.3%) vs 1/28 (3.6%); fragile across thresholds |
| Fate of the 9 pumps at Day 100 | 5 Sustained, 4 Fade, 0 Collapse |
| Same pattern at Day 90 / 100 | 22.8% of all IPOs underwater at Day 60; not specific to early-weak (17.9% vs 24.8%) |
| Buying early-weak at Day 15 | median -6.5% to Day 30, -5.6% to Day 100, -19.7% to Day 365 |

What this says, carefully:
- A small group of early-weak IPOs does rally and some hold the gains, but it is a minority and cannot be identified in advance from the weak start alone.
- Where the Sustained group rallies, the rise tends to start by Day 21, before the Day-30 anchor unlock, not after it.
- In the event study there is no clear spike at Day 30 for anchor-backed issues. Anchor-backed issues mostly hold their price while non-anchor issues drift down.
- The Day 90 and Day 365 repeat is no more common among early-weak IPOs than among other IPOs that are underwater.

![Event study around the Day-30 unlock](04_pump_dump_hypothesis/figures/event_study_day30_unlock.png)
![Closest lookalikes to the W1 template](04_pump_dump_hypothesis/figures/lookalikes_w1_template.png)

**Interactive dashboard:** [`04_pump_dump_hypothesis/dashboard/pump_dump_dashboard.html`](04_pump_dump_hypothesis/dashboard/pump_dump_dashboard.html) (clusters, company picker, band slider). It was built on the earlier definition without the Day-15 condition and will be regenerated from the latest run.

**R analysis and leaderboards** (`03_r_analysis/`): `TO FILL` (key results from the R work: performance by sector, BRLM and anchor, and how they relate to the findings above).

## How to reproduce
**Pump and dump module (Python, Google Colab or local Jupyter):**
1. Download `01_database/SME_IPO_Database_v8.xlsx` (upload it to Colab when asked).
2. Open `04_pump_dump_hypothesis/notebook/01_fetch_day_points_and_cluster.ipynb` and choose Runtime > Run all.
3. The probe cell prints HTTP status for NSE and BSE. Both should show `[200]` on recent trading days. The full download takes 15 to 30 minutes and is cached in `bhav_cache2/`.
4. Outputs: `Day_Points_Returns.xlsx` (returns at every day-point and a Coverage sheet), `Clusters_and_Probabilities.xlsx`, and the PNG charts.
5. Check the Coverage sheet first: filter `Status` other than `complete`.

If NSE or BSE blocks Colab, run the notebook locally from an Indian internet connection.

**R analysis:** `TO FILL` (R version, packages, and the order in which to run the scripts).

## Limitations
- **Small samples:** only 9 IPOs in the W1 pump group, so percentages and cluster averages are unstable. Use the confidence intervals.
- **Forward-looking labels:** a pump is defined using later prices, so the label cannot be used as a signal on its own.
- **Choice of thresholds:** results move with the thresholds. See the sensitivity table in the workbook and in `04_pump_dump_hypothesis/findings.md`.
- **Survivorship and coverage:** 15 IPOs have incomplete data and recent listings have no Day 365 or 400 values.
- **Day-1 mismatch:** 45 IPOs have an exchange Day-1 close that differs from the master database by more than 1% (possible bonus or split adjustments, ticker changes or database errors). Under investigation.
- **One data point to verify:** Robokidz Eduventures shows a Day-1 close of 211.45 vs an issue price of 66.
- **No market-maker data**, so BRLM-linked market making is untested.
- **Associational, not causal:** the data cannot show intent by anchors, BRLMs or promoters.

## Roadmap
- [ ] Re-run with all IPOs complete and investigate the 45 Day-1 mismatches
- [ ] Regenerate the dashboard and workbook with Days 15 to 400 and the Day-15 definition
- [ ] Event study with abnormal returns against a market or SME index
- [ ] Reconcile the pump-and-dump results with the R findings at Day 30 and Day 90
- [ ] Add a market-maker column and test the BRLM link
- [ ] Out-of-sample test on IPOs listed after the current sample

## Sources, licence and contact
- Prices: NSE and BSE daily bhavcopy files (public exchange data; follow the exchanges' terms of use).
- IPO details, anchors and BRLMs: compiled by the author [add sources: prospectus, exchange filings, Chittorgarh].
- Licence: `TO DECIDE` (for example MIT for code, CC BY 4.0 for written analysis). Add a `LICENSE` file.
- Author: [name], [contact or LinkedIn].
