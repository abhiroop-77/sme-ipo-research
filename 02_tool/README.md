# Performance tool and leaderboards

The tool is built into the master database workbook: `01_database/SME_IPO_Database_v8.xlsx` (sheets named below).

## Anchor / BRLM Performance Calculator
Sheet: `Perfomance Calculation_Tool`
- Pick Anchor 1 (required), optionally Anchor 2 (co-investment pair), and optionally a BRLM from the dropdowns.
- Leave Anchor 1 blank and pick only a BRLM to see that BRLM's full track record.
- The summary shows the number of IPOs matched, average returns at Day 1, 7, 30, 35 and later, the number of winning companies, and the top 3 performers.

## Leaderboards
Each sheet ranks by average since-listing return and shows total IPOs, win rate, IPOs with 100-day data, average 100-day return and 100-day win rate, against a benchmark row:
- `Anchor_Leaderboard`: anchors
- `BRLM_Leaderboard`: BRLMs (companies with anchor data only)
- `Sector_Leaderboard`: sectors
- `Anchor_BRLM_Leaderboard`: anchor and BRLM pairs
- `Anchor_Anchor_Leaderboard`: anchors that invest together

## Note
The workbook was built in Google Sheets. The dropdown tool and leaderboards use Google Sheets formulas (FILTER, LET), so open the file in Google Sheets for it to work live. In Excel you may only see the last saved values.
