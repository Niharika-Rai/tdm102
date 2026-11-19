# TDM 102: Data Analysis with Python at Scale

**Niharika Rai** · Business Analytics & Information Management · Purdue University
*The Data Mine (TDM 102), Spring 2024*

![Python](https://img.shields.io/badge/Python-3-3776AB?logo=python&logoColor=white)
![pandas](https://img.shields.io/badge/pandas-150458?logo=pandas&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-013243?logo=numpy&logoColor=white)
![Matplotlib](https://img.shields.io/badge/Matplotlib-11557C)
![Jupyter](https://img.shields.io/badge/Jupyter-F37626?logo=jupyter&logoColor=white)
![HPC](https://img.shields.io/badge/HPC-Anvil%20Supercomputer-555555)

This repository holds 12 Jupyter notebooks I completed in **The Data Mine**, Purdue's data science learning community. Each week I wrote Python to load, clean, analyze and visualize **real public datasets with millions of records**: NOAA climate data, U.S. airline flights, FEC campaign finance filings, Craigslist vehicle listings and IoT weather-sensor feeds. All of the work ran on **Anvil**, a National Science Foundation supercomputer.

Over the semester I went from my first `print("Hello World!")` to building **reusable, documented functions**, writing **object-oriented class hierarchies**, and **benchmarking vectorized NumPy against pandas**.

---

## At a glance

| | |
|---|---|
| **Projects** | 12 weekly notebooks, about 60 graded analysis tasks |
| **Datasets** | 6 real-world domains: climate, aviation, elections, e-commerce, IoT sensors, Airbnb |
| **Largest dataset** | 5.8 million rows (2014 U.S. domestic flights) |
| **Environment** | Jupyter on the Anvil HPC cluster (Purdue / NSF ACCESS) |
| **Core stack** | Python, pandas, NumPy, Matplotlib, `pathlib`, `os`, `time` |

---

## Technical skills shown

| Area | What I did | Where |
|---|---|---|
| **Data ingestion** | Read CSV, pipe-delimited TXT, Parquet and remote URL data; set column names, `dtype` maps and `usecols` so large files load faster and use less memory | P2, P3, P6, P8, P10 |
| **Large-file processing** | Read multi-hundred-thousand-row files in pieces with `chunksize`, and looped over many yearly files using `pathlib` and list comprehensions | P3, P4 |
| **Data cleaning** | Found and counted missing values (1.18M nulls in a 1M-row dataset), dropped nulls on chosen columns, parsed dates with `pd.to_datetime`, built new features | P6, P8, P9 |
| **Aggregation & analysis** | `groupby`, multi-key grouping, `value_counts`, `unique`, `idxmax`, sorting, Boolean filtering on several conditions | P2–P9 |
| **Data visualization** | Bar charts, multi-series line charts (using `unstack`) and box plots, with readable titles, axes and month labels | P2, P8 |
| **Functions & documentation** | Turned one-off analysis into reusable functions with parameters, default arguments, type hints and docstrings | P5–P9, P11 |
| **Performance** | Timed a pandas filter against a vectorized NumPy version on 5.8M rows. NumPy was **about 2.4x faster** (0.045s vs 0.019s) with the same result | P10 |
| **Object-oriented programming** | Wrote a `Flight` class, then the `ScheduledFlight` and `CancelledFlight` subclasses with their own methods; turned DataFrame rows into objects and grouped results with dictionary comprehensions | P11, P12 |
| **Python fundamentals** | Lists, tuples, dictionaries, `for` and `while` loops, f-strings, user input, conditionals | P1–P5 |

---

## Project highlights

### Climate & weather analytics
**NOAA Global Historical Climatology Network** (Projects 3, 4, 5, 7) · about 266 yearly files, 370K–580K records per year

- Wrote **file-iteration pipelines** that load several years of daily climate data. I fixed the files' missing headers by setting the 8-column schema myself (`id`, `date`, `element_code`, `value`, quality flags and so on).
- Used **chunked reading** (`chunksize=10000`) to count **164,216 snowfall records** across 1880–1883. I got the same answer from both a vectorized filter and a row-by-row loop, which confirmed the result.
- Wrote parameterized functions such as `avg_aggreg_temp`, `avg_aggreg_temp_by_year_and_month` and `year_with_most_qflags` that compute average temperature or precipitation for any date range, month or measurement type, and find which year had the most records with each data-quality flag.
- Built `get_noaa_data(year)`, a documented loader function, and used it to find the **U.S. weather stations with the most snowy days and the most total snowfall** each year.
- Used a `while` loop to scan forward year by year until average precipitation passed a set threshold.

**WHIN IoT Weather Sensors** (Projects 8, 9) · 1M+ hourly readings from stations across Indiana

- Parsed timestamps into year, month and day features, then computed **monthly temperature profiles for each station**.
- Built reusable plotting functions (`line_plot`, `box_plot_for_wind_speeds`, `bar_plot`) that make multi-year line charts, monthly wind-speed box plots and humidity-by-temperature bar charts for any station or input.
- Ran a **data-quality check**: found **1,184,084 missing values**, compared dropping nulls everywhere with dropping them on chosen columns, built a `latitude_longitude` location key, and found the location with the **highest average wind-gust speed**.

### Aviation analytics
**U.S. Bureau of Transportation Statistics, 2014 flights** (Projects 10, 11, 12) · **5,819,811 flights**

- Loaded only the 9–13 columns I needed, with explicit types, to keep memory use low on a very large file.
- Used **NumPy arrays** to clean `NaN` values and compute departure and arrival delay statistics (mean delay 10.4 min; arrival delays from −112 to 2,444 min).
- **Benchmarked pandas against NumPy** on the same question, "average distance of flights delayed more than 60 minutes" (793.1 miles). The vectorized NumPy version ran about **2.4x faster**.
- Designed an **OOP model**: a `Flight` base class with getter methods, and the subclasses `ScheduledFlight` (`is_ontime()`, `is_delayed()`) and `CancelledFlight`. I turned DataFrame rows into objects and computed **average delays and on-time/delayed counts for each airport** using dictionary comprehensions.

### Campaign finance analytics
**Federal Election Commission individual contributions** (Project 6) · 1980, 1984 and 1988 election cycles

- Parsed pipe-delimited FEC files with a 21-column schema, a `dtype` map and date conversion, and packaged all of it in a documented `read_election_year()` function.
- Built functions to find the number of **unique political committees** (3,856 / 3,733 / 3,995), the **top 5 states by donation total** (California grew to $60.2M by 1988), the top employers and occupations of donors, and the largest individual contributors.

### Marketplace & short-term rental data
**Craigslist used vehicles** (Project 2) · 426,880 listings × 26 features
- Explored the data's size and columns; found **312,633 vehicles listed above $6,000** across **51 states and 404 regions**; compared Indiana and Texas; grouped listings under $6,000 by state and plotted them in a bar chart.

**Airports & Inside Airbnb (Antwerp)** (Project 4)
- Looked up records by condition in an airport reference table and in a live Airbnb dataset loaded from a URL.

---

## Repository structure

```
tdm102/
├── README.md
├── Niharika-Rai-project01.ipynb   # Python basics: I/O, lists, loops; first look at a USDA forest dataset
├── Niharika-Rai-project02.ipynb   # DataFrames, filtering, groupby, bar charts: Craigslist vehicles
├── Niharika-Rai-project03.ipynb   # Looping over files, list comprehensions, chunked reading: NOAA
├── Niharika-Rai-project04.ipynb   # Conditional filtering, while loops, groupby to dict: NOAA, airports, Airbnb
├── Niharika-Rai-project05.ipynb   # Writing parameterized functions and docstrings: NOAA temperature and quality flags
├── Niharika-Rai-project06.ipynb   # Reusable loaders and aggregation functions: FEC election donations
├── Niharika-Rai-project07.ipynb   # Type-hinted functions, dictionaries, sorting: NOAA U.S. snowfall
├── Niharika-Rai-project08.ipynb   # Time-series features and Matplotlib charts: WHIN weather (Parquet)
├── Niharika-Rai-project09.ipynb   # Missing-data handling and feature building: WHIN weather (1M rows)
├── Niharika-Rai-project10.ipynb   # NumPy vectorization and timing against pandas: 2014 flights (5.8M rows)
├── Niharika-Rai-project11.ipynb   # Classes and objects: Flight model, delay summaries by airport
└── Niharika-Rai-project12.ipynb   # Inheritance: ScheduledFlight and CancelledFlight subclasses
```

Each notebook follows the same layout: the question, then my code and its output, then a written explanation of my approach and what the results mean.

---

## Running the notebooks

The notebooks were run on the **Anvil** cluster, where the datasets are stored at `/anvil/projects/tdm/data/...`. All saved outputs, including the charts, are kept in the notebooks, so you can read every result on GitHub without running anything.

To run them yourself:

```bash
pip install pandas numpy matplotlib pyarrow jupyter
```

```bash
jupyter notebook
```

Then change the file paths to point to your own copies of the public datasets: [NOAA GHCN-Daily](https://www.ncei.noaa.gov/products/land-based-station/global-historical-climatology-network-daily), [BTS On-Time Performance](https://www.transtats.bts.gov/), [FEC Bulk Data](https://www.fec.gov/data/browse-data/?tab=bulk-data), [Inside Airbnb](http://insideairbnb.com/) and the [Craigslist vehicles dataset](https://www.kaggle.com/datasets/austinreese/craigslist-carstrucks-data).

---

## What this work shows

- **I learn fast and keep at it.** I started the semester with no programming background and finished it writing class hierarchies and benchmarking code against millions of rows.
- **I work with real, messy data.** Missing headers, null values, inconsistent labels (for example, "HOUSEWIFE", "HOUSE WIFE" and "HOMEMAKER" in FEC employer fields) and files too large to load in one go.
- **I write code others can reuse.** I turn repeated analysis steps into parameterized, documented functions.
- **I explain my results.** Every answer comes with a plain-language explanation of the method and what it means, which is how I would report findings to stakeholders.
- **I work with integrity.** Every project credits its sources and TA help and includes Purdue's academic honesty pledge.

---

## About me

I'm a **Business Analytics & Information Management** student at Purdue University, interested in **data analytics, business intelligence and data science** internships and roles. I like using data to answer business questions, from cleaning raw files to building the charts that tell the story.

📫 **Let's connect:** [LinkedIn](https://www.linkedin.com/in/YOUR-PROFILE) · [GitHub](https://github.com/niharika-rai)

---

<sub>Completed as part of TDM 102 (1 credit hour), The Data Mine, Purdue University, Spring 2024. Datasets were provided through The Data Mine on the Anvil supercomputer.</sub>
