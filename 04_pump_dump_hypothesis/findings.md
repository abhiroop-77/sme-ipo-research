# Findings: pump and dump module

Run of 31 Oct 2026, default thresholds, daily closes from NSE and BSE bhavcopy files.

## Conditional probability tree
| Step | k | n | Probability | 95% CI |
|---|---|---|---|---|
| Early-weak among IPOs with Day-1 and Day-7 data | 88 | 441 | 20.0% | 16.5 to 23.9 |
| W1 pump given early-weak | 9 | 84 | 10.7% | 5.7 to 19.1 |
| ... anchor-backed | 8 | 56 | 14.3% | 7.4 to 25.7 |
| ... no anchors | 1 | 28 | 3.6% | 0.6 to 17.7 |
| Fate after W1: Sustained | 5 | 9 | 55.6% | 26.7 to 81.1 |
| Fate after W1: Fade | 4 | 9 | 44.4% | 18.9 to 73.3 |
| Fate after W1: Collapse | 0 | 9 | 0.0% | 0.0 to 29.9 |
| W2 pump given underwater at Day 60 (all IPOs) | 43 | 189 | 22.8% | 17.4 to 29.2 |
| ... early-weak | 10 | 56 | 17.9% | 10.0 to 29.8 |
| ... not early-weak | 33 | 133 | 24.8% | 18.2 to 32.8 |
| W3 pump given underwater at Day 100 (all IPOs) | 20 | 105 | 19.0% | 12.7 to 27.6 |
| ... early-weak | 7 | 37 | 18.9% | 9.5 to 34.2 |

## Median return vs issue price by cluster (selected day-points)
| Day | Early-weak, no pump (n=79) | W1 Sustained (n=5) | W1 Fade (n=4) |
|---|---|---|---|
| 7 | -10.6% | -2.0% | -7.7% |
| 15 | -12.8% | -2.0% | -3.1% |
| 21 | -15.1% | +15.2% | -3.4% |
| 30 | -20.0% | +22.6% | -0.5% |
| 60 | -16.9% | +33.4% | -6.4% |
| 100 | -17.7% | +32.5% | -13.6% |
| 365 | -34.5% | +24.1% | -7.6% |

## Entry at the Day-15 close (early-weak cohort, n=88)
| Horizon | Median forward return | Share of entries with a gain |
|---|---|---|
| Day 30 | -6.5% | 23% |
| Day 60 | -5.8% | 27% |
| Day 100 | -5.6% | 30% |
| Day 365 | -19.7% | 16% |

## Event study (Day-30 unlock)
Cumulative return indexed to calendar day 20. Anchor-backed early-weak IPOs (n=48) stay within about -2% to +1% through Day 45 with no visible spike. IPOs with no anchors (n=25) fall about 8% by Days 27 to 29 and recover to about -4% by Day 45. The bands overlap heavily, so no clear difference in timing can be claimed.

## Reading
1. The early-weak start is persistent for most issues: 79 of 88 never reach the pump definition and keep drifting lower.
2. The 9 pumps include some strong winners (for example Tipco Engineering) and some that fade; the group is too small for firm conclusions.
3. Several pumps rose before Day 30, so the data does not show a rise that follows the anchor unlock.
4. The Day 90 and Day 365 repeats occur at similar rates in IPOs that did not start weak.

## Caveats
Small samples; thresholds chosen by the author; the anchor vs no-anchor gap shrinks under stricter Day-7 cutoffs; 45 IPOs have a Day-1 mismatch between database and exchange files; recent listings lack late day-points.
