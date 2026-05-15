# AI Discussion in Earnings Calls and Trading Activity

**Author:** Sihan Chen

## Research Question

Does AI-related discussion in earnings calls affect stock trading activity, and does this relationship vary across different stages of the AI hype cycle?

## Project Overview

This project examines whether firms' AI-related discussion in earnings calls is associated with investor trading activity in the U.S. equity market. The study combines earnings call transcript data with daily stock turnover data. AI discussion is measured using keyword-based text analysis, while market trading activity is measured using daily turnover.

The project focuses on earnings calls from 2019 to 2025. The 2019 period is used as a pre-ChatGPT baseline, while later periods capture different stages of AI-related market attention.

## Dataset Description

This repository contains two research-ready datasets.

### Main Assignment Dataset: Earnings Call AI Dataset

File path: data/earnings_call_ai_dataset.csv

The main assignment dataset is earnings_call_ai_dataset.csv, which is the firm–earnings-call-level dataset used to construct transcript-based AI discussion measures. Each observation represents one earnings call. The dataset includes firm identifiers, earnings call information, and AI-related text variables.

Main variables include:

- ticker: firm ticker
- companyname: company name
- transcriptid: transcript identifier
- keydevid: Capital IQ key development event identifier
- call_date: earnings call date
- headline: transcript headline
- total_sentences: total number of sentences in the transcript
- ai_sentence_count: number of sentences containing AI-related keywords
- ai_intensity: AI-related sentence count divided by total sentence count

The original transcript data were collected from Capital IQ through WRDS. The full transcript text file is not uploaded because of file size and data access restrictions. Instead, this repository provides a processed earnings-call-level dataset with AI discussion measures.

### Supporting Market Data Panel: Daily Turnover Dataset

File path: data/turnover_daily_clean_2019_2025.csv

The daily turnover dataset is provided as the supporting market-data panel for constructing abnormal turnover and event-window trading activity measures. This dataset contains firm-day-level stock turnover data from 2019 to 2025. The raw data were exported from Bloomberg Terminal and cleaned using Python.

Main variables include:

- ticker: Bloomberg ticker
- trading_date: trading date
- PX_VOLUME: daily trading volume
- EQY_SH_OUT: shares outstanding
- turnover: daily trading volume divided by shares outstanding
- turnover_pct: turnover expressed in percentage terms

Daily turnover is calculated as:

turnover = PX_VOLUME / (EQY_SH_OUT × 1,000,000)

The multiplier is used because Bloomberg's EQY_SH_OUT field is reported in millions of shares.

A final merged event-level dataset has not yet been constructed in this Assignment 3 submission. The next step is to merge the earnings-call-level AI dataset with the daily turnover panel by ticker and event date.

## Data Collection Method

The project uses database exports and Python-based data cleaning.

- Earnings call transcripts were collected from WRDS Capital IQ.
- Daily trading volume and shares outstanding were collected from Bloomberg Terminal.
- Python was used to clean, reshape, merge, and construct analysis variables.
- No HTML scraping was used.

## Code Files

The code folder contains two Jupyter notebooks.

### 1. 01_transcript_collection_2021_2025.ipynb

This notebook documents the initial collection of earnings call transcript data for the 2021–2025 sample period. It collects earnings call metadata and transcript text from WRDS Capital IQ.

### 2. 02_data_extension_cleaning_turnover_2019_2025.ipynb

This notebook extends the dataset by adding the 2019 baseline period, combines transcript datasets, constructs AI-related discussion measures, prepares Bloomberg ticker requests, cleans Bloomberg daily volume and shares outstanding data, and calculates daily turnover.

## How to Run the Notebooks

The notebooks can be opened and run in Jupyter Notebook, JupyterLab, or VS Code.

Required Python packages include:

- pandas
- numpy
- wrds
- openpyxl

The notebooks also use built-in Python libraries such as re and pathlib.

To reproduce the data preparation process:

1. Open the notebooks in the code folder.
2. Run 01_transcript_collection_2021_2025.ipynb to review the transcript collection process.
3. Run 02_data_extension_cleaning_turnover_2019_2025.ipynb to review the data extension, AI variable construction, and Bloomberg turnover cleaning process.

Important access notes:

- The WRDS Capital IQ transcript data require institutional WRDS credentials.
- The Bloomberg turnover data require access to Bloomberg Terminal or Bloomberg Excel Add-in.
- The full raw transcript text file is not uploaded because of file size and data access restrictions.
- The repository provides cleaned research-ready CSV files for Assignment 3 documentation and future empirical analysis.

## Current Research Readiness

The collected data are ready for the next stage of empirical analysis. The earnings-call-level dataset provides the key independent variable, AI intensity, while the daily turnover panel provides the market response measure.

The next step is to merge the earnings-call-level AI dataset with the daily turnover panel by ticker and call date. After the merge, event-window turnover measures will be constructed.

The primary event window is [0,+1] trading days around the earnings call date. The normal turnover benchmark will be calculated over the pre-event window [-60,-11]. Alternative windows such as [-1,+1] and [0,+3] may be used for robustness checks.

These variables will be used to test whether AI-related discussion in earnings calls is associated with higher trading activity and whether this relationship changes across different stages of the AI hype cycle.

## Notes on Data Availability

The full transcript text file is not uploaded to this public repository because it is large and may be subject to WRDS Capital IQ data access restrictions. The repository instead provides a processed earnings-call-level dataset that contains the variables needed for Assignment 3 and future empirical analysis.

The Bloomberg turnover dataset is provided as a cleaned firm-day panel. It was constructed from Bloomberg-exported daily trading volume and shares outstanding data.
