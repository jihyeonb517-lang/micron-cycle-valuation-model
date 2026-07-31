# Micron (MU) Cycle Valuation Model

A residual-income model for a memory cyclical, built so that **terminal ROE is derived from wafer allocation and segment margins rather than assumed.**

> Snapshot: 2026-07-31 · Share price $900 · 24 tabs · 1,486 formulas, zero errors

---

## Why this exists

Most retail valuation models end at `target price = BPS x assumed P/B`. The multiple is the input, so the answer follows whatever the author already believed.

This model inverts the information hierarchy. The most observable input — wafer allocation, published by TrendForce — drives the chain. The least observable — terminal ROE — falls out as an output. Consensus stops being the thing you trust and becomes the thing you test.

```
MixPath  ->  SegmentMargin  ->  ROE_Build  ->  TerminalROE  ->  RIM_3Stage  ->  PB_Bridge
   ^              ^                 ^              ^               ^
 wafers      ASP indices        buybacks     cycle weights    discount rate
```

Two cross-checks run against consensus along the way. Where the model and consensus disagree, the tab shows *which assumption* causes the gap rather than applying an arbitrary haircut.

---

## Browse without Excel

The `data/` folder holds CSV snapshots of the derived tabs. GitHub renders these as tables, so the logic is readable in the browser:

| File | What it shows |
|---|---|
| [`data/MixPath.csv`](data/MixPath.csv) | Wafer share to bit share to revenue mix |
| [`data/SegmentMargin.csv`](data/SegmentMargin.csv) | Segment OPM derived from ASP indices; cross-check 1 |
| [`data/ROE_Build.csv`](data/ROE_Build.csv) | Two-factor DuPont ROE with buyback adjustment; cross-check 2 |
| [`data/TerminalROE.csv`](data/TerminalROE.csv) | Through-cycle weighting with explicit trough build-up |
| [`data/DiscountRate.csv`](data/DiscountRate.csv) | CAPM build-up, beta derived from contract-fixed revenue share |
| [`data/PB_Bridge.csv`](data/PB_Bridge.csv) | P/B split into book value, explicit RI and terminal, plus news routing |
| [`data/Sensitivity.csv`](data/Sensitivity.csv) | One-way tornado ranking every input |
| [`data/AssumptionGrades.csv`](data/AssumptionGrades.csv) | A/B/C confidence grades and the update calendar |

CSVs are static exports. The workbook is the live model.

---

## Tabs

| Tab | Purpose |
|---|---|
| `Guide` | Legend, tab map, how to read the model |
| `Assumptions` | All top-level inputs |
| `CurrentData` / `HistoricalCycle` | Actuals and the FY2021-25 cycle record |
| `Consensus` | Analyst estimates FY2025(A)-FY2029(E), including estimate counts |
| `MixPath` | Wafer allocation to revenue mix |
| `SegmentMargin` | Segment margins from ASP indices; cross-check against consensus EBIT margin |
| `ROE_Build` | DuPont ROE with buyback adjustment; cross-check against consensus ROE |
| `TerminalROE` | Through-cycle ROE, feeding the RIM |
| `DiscountRate` | Three-stage discount rates from a CAPM build-up |
| `RIM_3Stage` | Residual income model — section 11 is the adopted structure |
| `PB_Bridge` | Why the market assigns the multiple it does |
| `Sensitivity` | Tornado: which assumption actually moves the answer |
| `AssumptionGrades` | What to fix first, and when it gets resolved |
| `Scenarios` | Bull/base/bear driven by three price indices |
| `ChinaRisk`, `Buybacks`, `PurchaseStrategy`, `TargetPriceCheck`, `ResearchSummary` | Supporting analysis |
| `PB_ROE_Model`, `NormalizedEarnings` | Superseded by RIM_3Stage; retained for comparison |

---

## Cell conventions

| Style | Meaning |
|---|---|
| Blue text | Hardcoded input or scenario lever |
| Black text | Formula — do not edit |
| Green text | Reference to another sheet |
| Yellow fill | Key assumption — these are the cells to change |
| `[*]` prefix | An input the user is expected to set |

---

## Where the leverage actually is

`Sensitivity` ranks every parameter by its swing in intrinsic value. As configured, the top four account for roughly **78% of total swing**:

| Rank | Parameter | Swing |
|---|---|---|
| 1 | Cumulative buyback spend | 24.8% |
| 2 | Commodity DRAM base OPM | 18.6% |
| 3 | Peak multiple | 17.8% |
| 4 | Terminal discount rate | 16.5% |

Every HBM-related parameter ranks in the bottom tier (2.5-2.9%). That is a deliberate finding, not an oversight — mix shift is close to margin-neutral, and commodity DRAM still carries more than half of revenue. Analytical effort and impact on the answer are misaligned, which is itself worth knowing.

---

## Assumption quality

**11 of 13 tracked inputs are grade C** (user estimate, unverified). The calculation structure is complete; input confidence is the binding constraint.

`AssumptionGrades` carries an update calendar showing which event upgrades which cell — for example, the Dec 9 CHIPS restriction lifting moves the highest-sensitivity input from grade C to grade A.

---

## Verification

- 1,486 formulas recalculate with **zero errors** under LibreOffice
- `Sensitivity` reproduces the RIM result through an **independent formula chain** with zero difference
- `MixPath` reproduces the actual FY2026 revenue mix (53/23/24) **exactly** under premium Case 3
- Discount rate and OPM refactors were calibrated to reproduce the prior manual values without changing any output

---

## Disclaimer

**This is not investment advice.** It is a framework for organizing assumptions and testing what a given price implies, published so the reasoning can be inspected and criticized.

Every grade C value is an unverified judgment and must be independently checked. Market data reflects a 2026-07-31 snapshot and is stale on arrival. Forward-looking figures are estimates that will be wrong in ways the model cannot anticipate. Nothing here constitutes a recommendation to buy or sell any security.

---

## License

MIT — see [LICENSE](LICENSE).
