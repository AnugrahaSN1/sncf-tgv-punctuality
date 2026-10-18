# sncf-tgv-punctuality

Where and why are TGV trains late? A Tableau dashboard built on nine years of SNCF punctuality data.

Interactive dashboard: https://public.tableau.com/app/profile/anugraha.sn/viz/NCFTGVPunctuality20182026/SNCFTGVPunctuality?publish=yes

![Dashboard](SNCF%20TGV%20Punctuality.png)

## The question

SNCF publishes monthly punctuality figures for every TGV route. I wanted to use them to answer three things someone running the network would care about:

1. Is punctuality getting better or worse?
2. Which routes are the problem?
3. What is actually causing the delays?

## The data

SNCF open data, monthly TGV punctuality by route, January 2018 to September 2026. That is 12,907 rows, one per route per month, covering about 3.4 million trains on 130 routes (each direction counts as its own route).

For each route and month the file gives the number of trains planned, cancelled and late, average delays, and the share of delays by cause.

The original file is in French. I renamed the columns in English inside Tableau and left the CSV as it was downloaded.

## How I measured punctuality

I counted a train as punctual if it arrived less than 15 minutes late, using the "trains late by more than 15 min" column.

The main thing I had to be careful about: every row is a route-month, and routes are very different sizes. Some run 30 trains a month, others 900. If you average the percentages row by row, a tiny route counts as much as Paris–Lyon. So every rate on the dashboard is calculated from totals first:

```
Punctuality % = 1 - SUM(trains late 15+ min) / SUM(trains planned - trains cancelled)
```

The average delay and the delay causes are weighted the same way, by the number of late trains on each route.

## What I found

- **2026 is the worst year in the data.** 85.6% of TGVs arrived less than 15 minutes late from January to September 2026, against 89.4% over the whole period.
- **Cancellations are mostly fixed, delays are not.** The cancellation rate went from 7.6% in 2018 to 0.8% in 2025.
- **Summer is the weak spot.** July is the worst month across the period. The single worst month is April 2020 (76.8%), during the first COVID lockdown.
- **International routes do worst.** Italy to Paris Lyon is the least punctual route overall, with about one train in three arriving 15+ minutes late. In 2026, Paris Est to Stuttgart is worse still at 47%.
- **There's no single cause.** External causes, traffic management, infrastructure and rolling stock each account for roughly a fifth of delays. In 2026, though, external causes rose to almost 30%.

## What I'd recommend looking at

If I were presenting this to SNCF, I'd point to two things. First, summer (June to August) was less punctual than the rest of the year in every year except 2020, so the drop is predictable and worth planning for. Second, a small group of international routes pulls the average down, and those depend partly on foreign networks, so improving them means working with partner operators rather than fixing things in France alone.

## Limitations

- This data only tells you *that* a train was late and the broad cause category, not the specific incident.
- The dataset doesn't say exactly which late trains the cause percentages refer to. I weighted them by trains late at arrival, so treat the cause split as approximate.
- 2026 only covers January to September, so it isn't directly comparable with full years.

## Files

- `SNCF TGV Punctuality 2018–2026.twbx`: the Tableau workbook (open with Tableau Public, free)
- `regularite-mensuelle-tgv-sncf (1).csv`: the source data from SNCF
- `SNCF TGV Punctuality.png`: image of the dashboard

## Tools

Tableau Public.
