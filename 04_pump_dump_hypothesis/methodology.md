# Methodology

## Day convention
Day N = exact calendar day N after the listing date. If the exchange was closed on that day, the next trading day's close within 10 calendar days is used. Day 1 is treated as the listing date unless the notebook's check shows the database uses the following day.

## Returns
`return(Day N) = close(Day N) / issue price - 1`. Window lifts use the ratio of two closes, for example `W1 lift = max(close D30, close D35) / close D7 - 1`.

## Clusters and defaults
| Item | Rule | Default |
|---|---|---|
| Early-weak | abs(D1 return) <= band AND D7 return <= cutoff AND D15 return <= cutoff | band 10%, cutoff 0% |
| W1 pump | early-weak AND W1 lift >= threshold | 10% |
| Fate after W1 | Day 100 (else Day 90) vs W1 peak | Sustained >= -10%; Fade between; Collapse <= -35% |
| W2 pump | D60 return < 0 AND max(D90, D100) / D60 - 1 >= threshold | 10% |
| W3 pump | D100 return < 0 AND max(D365, D400) / D100 - 1 >= threshold | 25% |
| Template | mean (or median) return of a chosen cluster at each day-point | W1 pump group |
| Band | template +/- width (a tolerance, not a standard deviation) | 10 return points |
| Lookalike | inside the band at 80% or more of day-points, at least 4 available | not in the cohort |

## Statistics
- 95% Wilson score interval for each proportion.
- Fisher exact test: pump rate for anchor-backed vs non-anchor early-weak IPOs.
- Placebo windows: compare the Day 27 to 35 return with other 8-day windows from the same stocks.
- Event study: cumulative return indexed to calendar day 20, with bootstrap 90% bands, anchor-backed vs non-anchor.
- Sensitivity: pump rate across Day-1 band (10, 15, 20%), Day-7 cutoff (0, -5%) and lift (5 to 20%).

## Known issues
- Thresholds were chosen by the author; the sensitivity table shows how fragile conclusions are.
- The pump label uses future prices.
- Some exchange closes may be adjusted for corporate actions and differ from the database.
