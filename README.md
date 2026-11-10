# Kenya Food Market Prices Analysis in Python

An analysis of 14 years (2006–2019) of real World Food Programme (WFP) maize and bean prices in Kenya. It answers two practical questions for a (fictional) NGO, **Pamoja Agri-Advisory**:

1. **For farmers:** is it better to sell maize straight after harvest, or store it and sell a few months later?
2. **For county officers:** which markets are consistently the most expensive for households?

> Pamoja Agri-Advisory is a fictional organisation created for this project. The prices are real.


---

## Key findings

| Question | Answer |
|---|---|
| Dataset size | 2,742 rows, 14 markets, 2 commodities (Maize, Beans), 15 Jan 2006 to 15 Dec 2019 |
| Market with no county | Hola (59 rows), filled as Tana River County, Coast region |
| Retail vs wholesale maize (per KG) | Retail averaged KES 47.92/kg vs KES 28.54/kg wholesale, about **68% higher** |
| Highest average wholesale maize price | **2017**, KES 3,743.68 per 90 KG bag |
| Biggest yearly rise / fall | Rise in **2011** (+KES 1,246.27); fall in **2018** (−KES 1,335.29) |
| Cheapest / dearest month for wholesale maize | **January** (KES 2,330) / **June** (KES 2,839) |
| Did holding maize from January to June pay off? | June was higher than January in **73.3%** of market-years; median gain **KES 356** per 90 KG bag |
| Most / least expensive retail market, 2015–2019 | **Lodwar** (KES 75.85/kg) / **Kitui** (KES 36.19/kg) |
| Top three most expensive every year | **Lodwar** and **Mandera** |

The full advice for Pamoja is in [`pamoja_note.md`](pamoja_note.md).

---

## Dataset

- **File:** `food_prices_kenya.csv` (2,742 rows × 9 columns), one row per commodity, per market, per month.
- **Source:** WFP Kenya – Food Prices, published on the Humanitarian Data Exchange (HDX). Licence: CC BY-IGO 3.0.
- **Coverage:** white maize and dry beans; five wholesale markets (Nairobi, Mombasa, Kisumu, Eldoret, Nakuru) and nine retail markets (Garissa, Hola, Kajiado, Kilifi, Kitui, Lodwar, Mandera, Marigat, Marsabit).
- **Two price types that are never averaged together:**
  - **Wholesale:** price for a 90 KG bag (what traders pay).
  - **Retail:** price per 1 KG (what households pay).
- Five retail markets only start in 2015, and Nakuru wholesale starts in 2015, so retail markets are compared over 2015–2019 only.

| Column | Description |
|---|---|
| `date` | Month of the price (15th of the month) |
| `region` | Former province of the market |
| `county` | County of the market |
| `market` | Market (town) where the price was collected |
| `commodity` | Maize or Beans |
| `price_type` | Wholesale or Retail |
| `unit` | `90 KG` (wholesale) or `KG` (retail) |
| `price_kes` | Price in Kenyan shillings for the stated unit |
| `price_usd` | Price in US dollars at that month's exchange rate |

Columns added in the notebook: `year`, `month`, `price_per_kg`.

---

## Method

1. **Load and clean:** parsed dates, added `year` and `month`, checked for duplicates (none), and filled the missing county and region for Hola.
2. **Compare like with like:** split wholesale (per 90 KG) from retail (per KG) and added `price_per_kg` (wholesale price ÷ 90) so trader and household prices can be compared per KG.
3. **Maize over the years:** yearly average wholesale maize price per bag and year-on-year change, plotted as line charts.
4. **Sell at harvest or hold?:** average price by month, then each market's January vs June price in every year to measure how often storing for five months paid off.
5. **Where households pay most:** average retail maize price per market for 2015–2019, ranked within each year.
6. **Extra:** a box plot of wholesale maize price distribution by market.

---

## Repository structure

```
food_prices_kenya/
├── README.md
├── pamoja_note.md
├── analysis.ipynb
├── pyproject.toml
├── uv.lock
├── .python-version
└── data/
    └── food_prices_kenya.csv
```

## How to run

This project is managed with [uv](https://docs.astral.sh/uv/).

1. Install uv if you don't have it (see the [uv installation guide](https://docs.astral.sh/uv/getting-started/installation/)).
2. Clone or download this repository and move into the project folder:
   ```bash
   cd food_prices_kenya
   ```
3. Create the environment and install the dependencies (pandas, NumPy, Matplotlib, Seaborn, Jupyter) from `pyproject.toml` and `uv.lock`:
   ```bash
   uv sync
   ```
4. Make sure the CSV is in the `data/` folder, then open and run the notebook:
   ```bash
   uv run jupyter notebook analysis.ipynb
   ```

---

## Limitations

- The January-to-June comparison uses **market prices only**. It leaves out storage losses (pests, mould, aflatoxin), weight loss, storage costs, theft, and the cost of waiting for cash.
- Wholesale prices are **market prices, not farm-gate prices**. Farmers usually sell to traders for less.
- The retail-vs-wholesale comparison averages all available years for each price type, and retail coverage is mostly 2015 onwards, so the 68% gap is a rough guide, not a like-for-like margin.
- WFP switched to a different set of markets from 2020, so this analysis stops at 2019.
- Prices are nominal (not adjusted for inflation).

## Data attribution

Data: WFP Kenya – Food Prices, via the Humanitarian Data Exchange (HDX), licensed CC BY-IGO 3.0. The file used here is a filtered version (white maize and dry beans, selected markets, 2006–2019, snake_case column names, shortened market names).
