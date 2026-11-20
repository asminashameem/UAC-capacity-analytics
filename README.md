# System Capacity & Care Load Analytics: Unaccompanied Children Program

[![Streamlit App](https://static.streamlit.io/badges/streamlit_badge_black_white.svg)](https://<your-app-name>.streamlit.app)

A data analytics project on daily operational counts from the U.S. Unaccompanied Alien Children (UAC) program, January 2023 to December 2025. Children apprehended by CBP are held in CBP custody, transferred to HHS care, and later discharged to sponsors. This project turns those raw counts into capacity metrics, finds periods of strain and relief, and checks whether recorded transfers and discharges explain the number of children in care.

## Quick links

| What | Link |
|---|---|
| Live Streamlit app | https://<your-app-name>.streamlit.app |
| Power BI report (file) | [powerbi/UAC_Dashboard.pbix](powerbi/UAC_Dashboard.pbix) (open in Power BI Desktop) |
| Power BI report (PDF preview) | [powerbi/UAC_Dashboard.pdf](powerbi/UAC_Dashboard.pdf) |
| Power BI walkthrough video | <add link, or delete this row> |
| Research paper | [reports/research_paper.pdf](reports/research_paper.pdf) |
| Executive summary (1 page) | [reports/executive_summary.pdf](reports/executive_summary.pdf) |
| Analysis notebook | [notebooks/UAC_Care_Load_Analytics.ipynb](notebooks/UAC_Care_Load_Analytics.ipynb) |

## Questions

1. How heavy was the combined load (children in CBP custody plus children in HHS care) over time?
2. When was the system under sustained strain, and when did it ease?
3. Do recorded transfers and discharges explain the changes in the number of children in care?

## Screenshots

| Power BI: System load overview | Power BI: CBP vs HHS |
|---|---|
| ![Page 1](reports/screenshots/powerbi_page1_system_load.png) | ![Page 2](reports/screenshots/powerbi_page2_cbp_vs_hhs.png) |

| Power BI: Net intake and backlog | Power BI: Strain and volatility |
|---|---|
| ![Page 3](reports/screenshots/powerbi_page3_net_intake.png) | ![Page 4](reports/screenshots/powerbi_page4_strain.png) |

![Streamlit app](reports/screenshots/streamlit_app.png)

## Main findings

- **Load fell by about 72%.** Average combined load was about 8,000 children from January 2023 to December 2024, about 4,000 in the January to February 2025 transition, and about 2,300 from March 2025. HHS care makes up almost all of it (about 33 times CBP custody in 2023 to 2024, and 82 times from March 2025).
- **Sustained high load was in 2023.** HHS care peaked at 11,516 on 20 December 2023 and total load at 11,762. Load stayed above its 75th percentile (8,248) for 167 days, from 1 August 2023 to 14 January 2024. Four strain windows cover 176 days in total.
- **A structural break at the start of 2025.** Apprehensions fell from a monthly mean of 158 per reported day in December 2024 to 16 in February 2025 and stayed low. Children in HHS care drained from about 6,600 to about 2,400 over roughly nine weeks and then levelled off. Eight relief windows cover 253 days, the longest from 1 January to 19 March 2025 (78 days). The volatility index peaked at 0.124 on 10 February 2025.
- **Recorded flows do not explain the load before 2025.** Discharges were higher than recorded transfers in every period (offset ratio 1.33, 2.15 and 1.30). But for 2023, transfers minus discharges imply a fall of about 43,000 to 47,000 children in HHS care (depending on whether January is included), while the number actually rose by about 3,200. By the end of 2024 the cumulative gap is about 47,000. In 2024 the monthly pattern lines up (correlation 0.97) with flows running about 500 to 1,100 a month too high, and in 2025 the two agree closely. For this reason total load, a count of children actually in care, is the main strain measure, and net intake is a secondary indicator.
- **No seasonality can be claimed.** The 2023 and 2024 monthly profiles are negatively correlated (-0.61).

## Data

Daily counts from 12 January 2023 to 21 December 2025: 1,075 calendar days, of which 720 have a report.

| Column | Short name | Type | Meaning |
|---|---|---|---|
| Date | date | | Reporting date |
| Children apprehended and placed in CBP custody | apprehended | flow | Daily intake |
| Children in CBP custody | in_cbp | stock | Active CBP care load |
| Children transferred out of CBP custody | transferred | flow | Flow into the HHS system |
| Children in HHS Care | in_hhs | stock | Active HHS care load |
| Children discharged from HHS Care | discharged | flow | Successful sponsor placements |

Source: <add the source or link here>.

**Data quality**

- 355 days (33.0%) have no report. All five columns are blank together, and 306 of those days are Fridays or Saturdays. Stock columns were interpolated. Flow columns were left empty and are analysed on reported days only.
- Reporting is uneven across the week (discharges average about 206 on Thursdays and Sundays against 136 to 166 on Mondays to Wednesdays), so flows are read as weekly or period averages.
- 86 reported days (11.9%) show more transfers than children in CBP custody. A likely explanation is that custody is an end-of-day count and transfers happen during the day, but this is not confirmed. These days are flagged and kept. No day has discharges above the HHS count, and there are no negative values.

## Approach

1. **Cleaning and validation.** Parse dates, convert counts, drop duplicates, build a complete daily calendar, check the logical rules from the brief, and document missing and anomalous days.
2. **Three periods.** The series breaks at the start of 2025, so results are compared across A (Jan 2023 to Dec 2024, 720 days), B (Jan to Feb 2025, 59 days) and C (Mar to Dec 2025, 296 days). B is short, so its averages are descriptive.
3. **Metrics.**

| Metric | Definition |
|---|---|
| Total load | Children in CBP custody + children in HHS care |
| Net intake | Transfers minus discharges (reported days) |
| Care load growth | Day-over-day percentage change in total load |
| Backlog indicator | 7-day sum of net intake above zero (at least 4 reported days) |
| Discharge offset ratio | Total discharges / total transfers. Above 1 means more discharges than recorded transfers |
| Volatility index | 14-day rolling standard deviation / rolling mean of total load |

4. **Strain and relief.** Strain is total load above its overall 75th percentile (8,248) while the 7-day average is not falling. Relief is a fall of 5% or more over 14 days. Only runs of 7 days or more count. These cutoffs are my choices, so the notebook includes a sensitivity check (four windows and 176 days at the 75th percentile, two windows and 74 days at the 90th).
5. **Reconciliation.** Monthly change in HHS care compared with the change implied by transfers minus discharges (mean per reported day times days in the month, so an estimate).
6. **Dashboards.** A four-page Power BI report and a Streamlit app that use the same definitions.

## Power BI report

| Page | What it shows |
|---|---|
| 1. System load overview | KPI cards, total load with 14-day average and strain/relief bands, average load by period, period summary table |
| 2. CBP vs HHS | HHS care and CBP custody on separate scales, average daily flows by period |
| 3. Net intake and backlog | Net intake with colour by sign, backlog gauge, actual vs implied change in HHS care, discharge offset by period |
| 4. Stress, volatility and strain windows | Load with threshold, strain and relief, volatility index, tables of strain and relief windows |

Slicers (synced on all pages): date range, period, and time granularity (day, week, month). To open it, use Power BI Desktop. If it asks where the data is, point it to the CSV files in `data/processed/`.

## Streamlit app

Run locally with `streamlit run streamlit_app.py` or open the live link above.

| Tab | Content |
|---|---|
| System load | Total load, HHS care and CBP custody, 14-day average, threshold, strain and relief bands, optional 8-week outlook |
| CBP vs HHS | Separate-scale charts, average daily flows and a period table |
| Net intake & backlog | Net intake, backlog gauge, reconciliation chart, offset ratio by period |
| Strain & volatility | Load with strain and relief windows, volatility index, window tables |
| Data notes | Definitions, data quality and limitations |

Controls: date range, period, time granularity (daily, weekly, monthly), and metric toggles. The 8-week outlook is experimental: a damped-trend smoothing on weekly load that has not been validated, and the 2025 break means it should not be read as a projection.

## Repository structure

```
uac-capacity-analytics/
├── README.md
├── LICENSE
├── streamlit_app.py            # Streamlit app
├── utils.py                    # data loading, filtering, KPIs
├── requirements.txt            # packages for the app
├── .gitignore
├── .streamlit/
│   └── config.toml             # app colours
├── data/
│   ├── raw/                    # original CSV (see Data section)
│   └── processed/              # cleaned and derived CSV files used by the app
├── notebooks/
│   └── UAC_Care_Load_Analytics.ipynb
├── powerbi/
│   ├── UAC_Dashboard.pbix
│   └── UAC_Dashboard.pdf
└── reports/
    ├── figures/                # charts saved by the notebook
    ├── screenshots/            # dashboard screenshots
    ├── research_paper.pdf
    └── executive_summary.pdf
```

Files in `data/processed/` that the app reads: `uac_features.csv`, `reconciliation_monthly.csv`, `strain_windows.csv` and `relief_windows.csv`.

## How to run

**Streamlit app**

```
pip install -r requirements.txt
streamlit run streamlit_app.py
```

**Notebook**

```
pip install pandas numpy matplotlib seaborn jupyter
jupyter notebook
```

Open `notebooks/UAC_Care_Load_Analytics.ipynb`. It looks for the raw CSV in `data/raw/` and saves its outputs to `data/processed/` and `reports/figures/`. If you run it from inside the `notebooks` folder, add these two lines to the setup cell so the paths work:

```python
if Path.cwd().name == "notebooks":
    os.chdir(Path.cwd().parent)
```

## Check your results

With the full date range, all periods and monthly granularity, these should match across the notebook, Power BI and Streamlit:

| Measure | Value |
|---|---|
| Total children under care (latest day) | 2,502 |
| Net intake per reported day | -44.7 |
| Net intake (total) | -32,212 |
| Volatility index (mean) | 0.024 |
| Backlog days | 34.3% |
| Discharge offset ratio | 1.35 |
| Peak total load | 11,762 |
| Strain days / relief days | 193 / 258 |

## Limitations

- The data is aggregated. There is no age, region or facility detail, so the analysis cannot say who is affected or where.
- Flow data is missing on about a third of days, and daily flows are not comparable across weekdays.
- Recorded transfers and discharges do not match the actual change in HHS care before 2025. A possible reason is that HHS receives children through routes that are not in the transfer column, but this could not be tested with the data available.
- The explanation for the days with transfers above custody is likely but not confirmed.
- The strain and relief cutoffs and the period boundaries are analyst choices.
- The analysis shows timing and association, not cause.

## Possible next steps

- Check the data source's documentation for how the transfer column is defined.
- Add a forecast and validate it on a hold-out period.
- Extend the app with downloads of the filtered data.

## Tools

Python (pandas, NumPy, matplotlib, seaborn, Plotly, statsmodels), Power BI Desktop, Streamlit, Git and GitHub.

## License and data

The code in this repository is released under the MIT License (see `LICENSE`). The dataset is not covered by that license. See the Data section for its source and terms.

## Author

Asmina K T

- LinkedIn: <your link>
- Email: <your email>
