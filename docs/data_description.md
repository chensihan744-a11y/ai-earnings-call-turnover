# Data Description

## 1. Data Sources

This project uses two main data sources: earnings call transcript data and stock trading data.

### 1.1 Earnings Call Transcript Data

The earnings call transcript data were collected from Capital IQ through WRDS. The dataset focuses on earnings call transcripts for U.S. listed firms from 2019 to 2025.

The transcript data include firm identifiers, transcript identifiers, earnings call dates, company names, transcript headlines, and AI-related text measures constructed from transcript content.

### 1.2 Bloomberg Turnover Data

The daily trading data were collected from Bloomberg Terminal. The Bloomberg data include daily trading volume and shares outstanding for the sample firms.

The two Bloomberg fields used are:

- `PX_VOLUME`: daily trading volume
- `EQY_SH_OUT`: shares outstanding

The Bloomberg daily turnover data cover trading dates from 2019 to 2025.

---

## 2. Collection Method

The project uses database exports and Python-based data cleaning.

The earnings call transcript data were collected from WRDS Capital IQ using Python. Transcript metadata and transcript text were cleaned and combined into an earnings-call-level dataset. AI-related discussion measures were then constructed using keyword-based text analysis.

The Bloomberg turnover data were exported from Bloomberg Terminal and cleaned using Python. The raw Bloomberg output was originally in a wide repeated-column format, with each firm appearing in a separate set of columns. This raw format was reshaped into a long firm-day panel.

No HTML scraping was used for the main proprietary transcript or turnover datasets. The S&P 500 constituent list was obtained through HTML table extraction from Wikipedia for sample screening.

---

## 3. Time Period Covered

The project covers earnings call and trading data from 2019 to 2025.

The year 2019 is used as the baseline period because it represents a pre-pandemic and pre-ChatGPT market environment. The 2021–2022 period is used as the pre-ChatGPT transition period. The year 2023 captures the initial GenAI surge after the release of ChatGPT, while 2024–2025 represents a later AI adoption period.

The year 2020 is not included in the main period classification because the pandemic shock created abnormal market conditions that could distort trading activity and make 2020 less suitable as a clean baseline.

The daily turnover dataset covers trading dates from 2019-01-01 to 2025-12-31.

---

## 4. Dataset Files

This repository includes two research-ready datasets.

### 4.1 Main Assignment Dataset

File name:

`earnings_call_ai_dataset.csv`

This is the main assignment dataset. It is a firm–earnings-call-level dataset used to construct transcript-based AI discussion measures. Each row represents one earnings call.

The full transcript text is not included in this public dataset because of file size and data access restrictions. Instead, this file provides the processed variables needed for Assignment 3 and future empirical analysis.

### 4.2 Supporting Market Data Panel

File name:

`turnover_daily_clean_2019_2025.csv`

This is a supporting firm-day-level market data panel. It is used to construct abnormal turnover and event-window trading activity measures.

A final merged event-level dataset has not yet been constructed in this Assignment 3 submission. The next step is to merge the earnings-call-level dataset with the daily turnover panel by ticker and call date.

---

## 5. Number of Observations and Variables

### 5.1 Earnings Call AI Dataset

File: `earnings_call_ai_dataset.csv`

Number of observations: 4,787 earnings-call observations  
Number of variables: 10 variables  
Number of firms: 195 firms  

Each observation represents one earnings call.

### 5.2 Daily Turnover Dataset

File: `turnover_daily_clean_2019_2025.csv`

Number of observations: 329,819 firm-day observations  
Number of variables: 6 variables  
Number of firms: 189 firms  

Each observation represents one firm on one trading day.

The number of firms is smaller in the turnover dataset because some tickers in the original request list had no valid Bloomberg turnover data, shorter trading histories, ticker changes, or incomplete historical availability.

---

## 6. Variable Descriptions

### 6.1 Earnings Call AI Dataset

| Variable Name | Data Type | Description | Example |
|---|---|---|---|
| `ticker` | string | Firm ticker used for matching across transcript and turnover datasets. Ticker formatting may be standardized before merging with Bloomberg data. | `AAPL` or `AAPL US Equity` |
| `companyname` | string | Company name | `Apple Inc.` |
| `transcriptid` | string / numeric | Capital IQ transcript identifier | `123456` |
| `keydevid` | string / numeric | Capital IQ key development event identifier | `789012` |
| `headline` | string | Earnings call headline | `Apple Inc. Q2 2023 Earnings Call` |
| `call_date` | date | Earnings call date | `2023-05-04` |
| `total_sentences` | numeric | Total number of sentences in the earnings call transcript | `320` |
| `ai_sentence_count` | numeric | Number of sentences containing AI-related keywords | `8` |
| `ai_intensity` | numeric | AI-related sentence count divided by total sentence count | `0.025` |
| `hype_period` | string | Period classification used to represent stages of the AI hype cycle | `2023 Initial GenAI Surge` |

### 6.2 Daily Turnover Dataset

| Variable Name | Data Type | Description | Example |
|---|---|---|---|
| `ticker` | string | Firm ticker used for transcript-level identification. Bloomberg files may use the `US Equity` suffix before standardization. | `AAPL` |
| `trading_date` | date | Trading date | `2022-06-07` |
| `PX_VOLUME` | numeric | Daily trading volume | `1236306` |
| `EQY_SH_OUT` | numeric | Shares outstanding reported in millions of shares | `298.70806` |
| `turnover` | numeric | Daily turnover calculated as trading volume divided by shares outstanding | `0.00413884` |
| `turnover_pct` | numeric | Daily turnover expressed in percentage terms | `0.413884` |

Daily turnover is calculated as:

`turnover = PX_VOLUME / (EQY_SH_OUT × 1,000,000)`

The multiplier is used because Bloomberg's `EQY_SH_OUT` field is reported in millions of shares.

---

## 7. AI Keyword Measurement

AI-related discussion is measured using keyword-based text analysis. The keyword list includes terms related to artificial intelligence, generative AI, machine learning, large language models, and major AI-related products or organizations.

Examples of keywords include:

- artificial intelligence
- generative AI
- GenAI
- machine learning
- large language model
- large language models
- LLM
- LLMs
- foundation model
- foundation models
- ChatGPT
- GPT
- OpenAI
- Copilot
- Gemini
- Claude
- Anthropic

The main AI discussion variable is:

`ai_intensity = ai_sentence_count / total_sentences`

This variable measures the share of transcript sentences that contain AI-related discussion.

For example, if a transcript contains 320 total sentences and 8 AI-related sentences, then:

`ai_intensity = 8 / 320 = 0.025`

This means that 2.5% of the transcript sentences contain AI-related discussion.

---

## 8. Hype Period Classification

The dataset includes a `hype_period` variable to classify earnings calls into different periods of the AI hype cycle.

The period classification is based on the earnings call date. The four period categories are:

- `2019 Pre-Pandemic Baseline`
- `2021–2022 Pre-ChatGPT Transition`
- `2023 Initial GenAI Surge`
- `2024–2025 AI Adoption Period`

The 2019 period is used as the baseline period because it represents a pre-pandemic and pre-ChatGPT market environment. The 2021–2022 period captures the transition period before the release of ChatGPT. The 2023 period captures the initial surge of market attention to generative AI. The 2024–2025 period captures a later AI adoption stage, when AI discussion became more common and more integrated into business strategy.

---

## 9. Data Quality Issues

### 9.1 Transcript Data

The full transcript text file is not uploaded to this public repository because of file size and data access restrictions. Instead, the repository provides a processed earnings-call-level dataset with AI discussion measures.

The transcript data may contain variation in transcript availability, formatting, and coverage across firms and years. Some firms may have fewer available transcripts than others.

The AI discussion measure is currently based on keyword matching. This approach is transparent and reproducible, but it may not fully capture context, tone, or whether AI discussion is strategic, operational, or risk-related.

### 9.2 Bloomberg Turnover Data

The raw Bloomberg output required reshaping because each ticker was exported in a repeated wide-column format. The raw file was converted into a long firm-day panel.

Some tickers have shorter date coverage because of IPO timing, spin-offs, ticker changes, or limited Bloomberg availability.

The original ticker request list contained more firms than the final turnover panel. The cleaned turnover dataset contains 189 firms because several tickers had no valid Bloomberg turnover data or incomplete historical availability.

The cleaned turnover dataset originally contained a very small number of missing turnover observations. These observations were removed before saving the final dataset.

### 9.3 Ticker Matching

Ticker formats may differ across transcript and Bloomberg files. Bloomberg tickers usually include the `US Equity` suffix, while transcript tickers may use the common stock ticker only. Ticker formatting will be standardized before merging the earnings-call-level dataset with the Bloomberg turnover panel.

### 9.4 Final Merge Status

The earnings-call-level AI dataset and the daily turnover panel have not yet been merged in this Assignment 3 submission. The planned next step is to merge them by ticker and call date, then construct event-window turnover measures.

The primary event window will be `[0,+1]` trading days around the earnings call date. The normal turnover benchmark will be calculated over the pre-event window `[-60,-11]`. Alternative windows such as `[-1,+1]` and `[0,+3]` may be used for robustness checks.

---

## 10. Research Readiness

The collected datasets are ready for the next stage of empirical analysis.

The earnings-call-level dataset provides the key independent variable, AI intensity. The daily turnover dataset provides the market response measure. Together, these datasets can be used to test whether AI-related discussion in earnings calls is associated with abnormal trading activity and whether this relationship changes across different stages of the AI hype cycle.

The next analytical step is to construct an event-level dataset by matching each earnings call to stock turnover around the call date. After this merge, the project can estimate whether AI intensity is associated with abnormal turnover and whether this relationship differs across hype-cycle periods.
