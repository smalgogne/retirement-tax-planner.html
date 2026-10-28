# retirement-tax-planner.html
A locally run web app to ingest financial data and perform portfolio modeling. Not a financial advise


RETIREMENT TAX & ROTH CONVERSION PLANNER
========================================

            ***** Not a financial advise *****
      ***** Read assumptions and limitations *****

A single-file, browser-based tool for projecting retirement income, taxes,
withdrawals, and Roth conversions from today through your expected age of death.
No server, no build step, no dependencies. All calculations run locally in the
browser.

DISCLAIMER: This is an estimation tool for planning and education. It is not
tax, legal, or financial advice. Tax law is simplified (see "Assumptions and
limitations"). Consult a qualified tax professional before acting on results.


QUICK START
-----------
1. Download retirement-tax-planner.html.
2. Open it in any modern browser (Chrome, Edge, Firefox, Safari).
3. Enter your profile, accounts, and milestones. Results update as you type.

Optional: host it with GitHub Pages by placing the file in your repo (for
example as index.html) and enabling Pages in the repo settings.


FEATURES
--------
Profile
  - Date of birth (current age is calculated), gender, employment start date
  - Current income and expected growth rate
  - Other ongoing income with optional end age
  - Current federal tax rate (used for working years), optional state rate
  - Inflation (default 2%)

Funding
  - Add-row accounts: name, type (Pre-tax, Roth, Taxable, HSA, Cash),
    balance, return %, contribution amount and frequency (yearly/monthly)
  - Balance as-of age: balances recorded at a younger age are grown to today
    using the return rate and contributions
  - Default Cash row plus an "Add cash row" button
  - CSV import (see "CSV import format")
  - Pre-tax (traditional) contributions reduce taxable wages in working
    years; all contributions end at retirement

Milestones
  - Retirement age
  - Social Security claim age, with expected annual benefit shown (reduced
    before 67, 8% per year delayed credit to 70); spouse benefit supported
  - Pension: manual amount, or FERS (OPM formula)
  - RMD age (73 or 75)
  - Expected age of death (default 85 male, 88 female)
  - Filing status (S, MFJ, MFS, HOH, QW) with ages of status change
  - Age of loss of spouse (switches MFJ to S; survivor takes the higher
    Social Security benefit)
  - Dependents with the age each loses dependent status

Schedules
  - Tax events: one time, recurring, or time period (description, amount,
    frequency, start/end date). Amount is taxable income; negative values
    act as deductions.
  - Planned spending (college, car, etc.): same schedule types, entered in
    today's dollars and inflated, funded from accounts using the chosen
    withdrawal order

Income strategy
  - Desired yearly income (today's dollars), or a withdrawal rate
    (default 3.9%) of the balance at retirement, inflated afterward
  - Withdrawal order: evenly pulled, specific order (cash, taxable, pre-tax,
    Roth), or optimized (fill low brackets with pre-tax first)

Roth conversion
  - Modes: none, fixed amount per year, or fill up to a chosen bracket
  - Start and end age
  - Conversion tax paid from converted funds or from external sources
    (cash/taxable accounts)
  - Side-by-side comparison against no conversion: lifetime tax, after-tax
    estate, and a plain-language assessment

Results
  - Projection chart with y-axis, gridlines, and hover tooltips
  - Income and inferred tax rates (effective and marginal) at milestone ages
  - Year-by-year table (age through total balances)
  - Monte Carlo analysis: configurable trials and volatility, success rate
    with and without conversion, median and percentile after-tax estate, and
    a percentile-band chart with hover tooltips

Snapshots
  - Save, load, and delete named snapshots in browser storage
  - Export/Import JSON for backup and moving between devices


CSV IMPORT FORMAT
-----------------
One account per line, no header row required:

  name,type,balance,return%,contribution,frequency,balance-as-of-age

  type:       pretax | roth | taxable | hsa | cash
  frequency:  1 (yearly) or 12 (monthly)
  as-of age:  optional; blank means current age

Example:
  401(k),pretax,500000,6,20000,1,58
  Roth IRA,roth,100000,6,0,1
  Savings,cash,25000,3,0,1


HOW THE MODEL WORKS
-------------------
Each year from current age to age of death:
  1. Contributions are added (working years only).
  2. Wages, other income, pension, Social Security, tax events, and cash
     interest are computed.
  3. Needed spending (desired income or withdrawal rate, plus planned
     spending) is funded by withdrawals in the selected order.
  4. RMDs are enforced from the RMD age; any excess goes to taxable.
  5. Roth conversions are applied per the chosen mode, with incremental tax
     computed against the no-conversion baseline.
  6. Federal and state tax are calculated, then balances grow at each
     account's return rate.

The Monte Carlo run repeats this with random annual returns around each
account's rate (cash stays fixed). Non-cash accounts share the same random
shock each year. A fixed random seed makes results repeatable. "Success" means
total savings never hit zero from retirement through the age of death. Both
strategies use the same random returns so they are directly comparable.


ASSUMPTIONS AND LIMITATIONS
---------------------------
  - Federal brackets and standard deductions use 2025 values, indexed
    forward by your inflation rate. Tax-law changes are not modeled.
  - "MFJ" means married filing jointly. QW uses joint brackets.
  - Working years use your entered federal rate times taxable income;
    retirement years use actual brackets.
  - Social Security taxation uses the standard provisional-income formula,
    simplified (thresholds not indexed).
  - State tax is a flat rate on ordinary income.
  - Dependents give a flat $2,000 credit each while they count as dependents.
  - Taxable-account withdrawals are taxed at an effective 7.5%.
  - After-tax estate values pre-tax balances at a 24% heir tax rate.
  - IRA deduction limits, contribution limits, IRMAA, NIIT, AMT, and capital
    gains bracket interaction are not modeled.
  - FERS: 1% (1.1% if retiring at 62+ with 20+ years) x high-3 x years of
    service, with FERS COLA from age 62. The FERS supplement, early-
    retirement reductions, and sick-leave credit are not modeled. High-3
    defaults to the average of projected salaries in the last three years
    before retirement unless you enter it.
  - Shortfalls (accounts running out while funding spending) are reflected
    in balances going to zero; individual unfunded amounts are not flagged.


DATA AND PRIVACY
----------------
  - Inputs live in memory only and reset on refresh unless saved as a
    snapshot.
  - Snapshots are stored in your browser's local storage for this page
    (private to that browser and device). Clearing browser data or using
    private browsing will remove them. Use Export JSON for backups.
  - Nothing is transmitted to any server. There are no analytics or external
    requests.


FILES
-----
  retirement-tax-planner.html   The entire application (HTML, CSS, JavaScript)
  README.txt                    This file


CONTRIBUTING
------------
Issues and pull requests are welcome. Ideas for future work:
  - Autosave of current inputs
  - Per-account volatility and asset-class modeling
  - Account-level contribution end ages
  - Shortfall warnings for planned spending
  - Updated tax tables by year and state-specific tax models


LICENSE
-------
GNU General Public License v3.0
