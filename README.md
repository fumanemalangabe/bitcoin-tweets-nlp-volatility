# Bitcoin tweets: topics, sentiment and volatility

What do people tweet about Bitcoin, and does tweet sentiment help forecast next-day Bitcoin volatility?

## Data

A Kaggle Bitcoin tweets dataset of 4,689,305 tweets. A default CSV reader reports 11.2 million rows because of newlines inside quoted tweets; reading with PyArrow and explicit escape handling recovers the true count.

## Method

- **Cleaning:** separate text columns for topic modelling and sentiment, exact-duplicate removal (1.65 million templated tweets), and removal of bot accounts that post from very few sentence openings.
- **Topics:** NMF, LDA and BERTopic on a 199,992-tweet sample stratified by month. The number of topics was chosen from c_v coherence, which peaks at k = 10 (0.8618).
- **Sentiment:** VADER, extended with crypto slang, against CryptoBERT, both checked against 60 tweets labelled independently by two raters (Cohen's kappa 0.686).
- **Volatility:** Garman-Klass daily volatility as the target, a lagged HAR baseline, GARCH(1,1) as a comparison, and a one-day-ahead expanding-window backtest.

## Results

- Five of the ten topics are promotional templates (airdrops, referral schemes), covering 18.1% of the sample.
- CryptoBERT matched human labels on 48.3% of tweets against 40.0% for VADER, and was much better at catching bearish views (recall 0.72 against 0.44).
- Sentiment did not improve volatility forecasts: the joint F-test on the sentiment variables gives p = 0.373, and no model called the direction of volatility better than a coin flip (best 51.7%). Yesterday's return and trading range were the significant predictors.
- Date-anchored checks (Tokyo Olympics, Bitcoin Pizza Day) show the dataset is mis-dated by exactly four years: the tweets are from March 2021 to March 2022.

## Limitations

The four-year offset was found but not corrected in this run, so the volatility results pair 2021 to 2022 tweets with 2025 to 2026 prices. They show that the pipeline works rather than establishing anything about Bitcoin.

## Files

| File | Contents |
|---|---|
| `bitcoin_tweets_nlp.ipynb` | Full notebook with outputs |
| `reports/` | Written report and presentation slides (PDF) |
| `outputs/q1_removed_templates.csv` | Most repeated templated tweets removed in cleaning |
| `outputs/q2_daily_sentiment.csv` | Daily sentiment series |
| `outputs/q2_modelling_table.csv` | Lagged modelling table |
| `outputs/q2_backtest_results.csv` | Backtest error metrics |
| `outputs/validation_labelled.csv` | 60 hand-labelled tweets from both raters |

The per-tweet topic and sentiment files (over 40 MB each) are not included.

## How to run

Open the notebook in Google Colab with a GPU runtime and upload the Kaggle Bitcoin tweets CSV. Update the file path in the first cells.

## Team

EDAB6808 Assignment 4, BCom Honours in Business Analytics, University of the Free State, 2026. Group project by Fumane Malangabe and Boipelo Letwaba.
