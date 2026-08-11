# HK Rent vs Buy Model

Excel decision model for comparing **renting vs buying** a home in Hong Kong — aimed at someone with little/no assets who is saving for a first purchase over the next few years, and wants to know **when (if ever) buying beats lifelong renting + investing**.

## Files

| File | Purpose |
|------|---------|
| `HK_Rent_vs_Buy_Model.xlsx` | The workbook — start here |
| `build_hk_rent_vs_buy.py` | Regenerates all calculated sheets from Assumptions |

## Quick start

1. Open `HK_Rent_vs_Buy_Model.xlsx`.
2. Read **Readme**, then edit blue cells on **Assumptions** (your surplus, rent, target price, mortgage, etc.).
3. Save the file.
4. Refresh calculations:

```bash
python3 build_hk_rent_vs_buy.py
```

5. Decide from **TimingMatrix** + **Sensitivity** (see **Dashboard** for a snapshot).

## Sheets

- **Dashboard** — affordability, wealth gap, break-even year, chart
- **Assumptions** — all inputs (HKD)
- **TimingMatrix** — buy in year 0…15 vs never buy
- **YearByYear** — annual cashflows for rent-forever vs selected buy year
- **StampDuty** — HK Scale 2 AVD reference (HKPR sole residential)
- **Sensitivity** — bear/base/bull property growth × investment returns

## Defaults (placeholders — replace with yours)

- HK$0 starting savings, HK$45k/month after non-housing costs, HK$22k rent
- HK$7.5m target flat, 80% LTV, 3.2% mortgage, 30-year tenor
- 2% property growth, 2.5% rent growth, 5.5% liquid investment return
- Scale 2 stamp duty, rates/management/maintenance allowances

Under these placeholders, the model typically says you need ~**8 years** to afford the purchase, and **renting + investing can still win on pure net worth** if markets return ~5.5% while prices only grow ~2%. That is intentional: HK buying is often a housing/lifestyle decision, not an automatic wealth win.

## Disclaimer

Educational only — not financial, tax, or mortgage advice. Confirm stamp duty, LTV and mortgage insurance with IRD / HKMA / your bank.
