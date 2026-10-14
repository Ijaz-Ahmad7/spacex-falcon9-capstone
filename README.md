# SpaceX Falcon 9 Landing Prediction — IBM Data Science Capstone

This project is the final capstone for the **IBM Applied Data Science** specialization on Coursera. It applies the full data science pipeline to predict whether the **Falcon 9 first stage will land successfully**, using launch data from the SpaceX API and Wikipedia.

## Project Background

SpaceX advertises Falcon 9 rocket launches at **$62 million**, while other providers cost upward of **$165 million**. Much of the savings come from **reusing the first stage**. Therefore, if we can predict whether the first stage will land, we can estimate the cost of a launch. This information is valuable for companies bidding against SpaceX for rocket launches.

## Business Problem

Predict whether the Falcon 9 first stage will land successfully. Target variable `Class`:

- `1` = Successful landing
- `0` = Unsuccessful landing

## Data Collection

### 1. SpaceX REST API

- Endpoint: `https://api.spacexdata.com/v4/launches/past` (via IBM's static snapshot)
- Extracted 107 launches with fields: flight number, date, booster version, payload mass, orbit, launch site, outcome, etc.

### 2. Web Scraping (Wikipedia)

- Scraped the Falcon 9 launch records from Wikipedia's *List of Falcon 9 and Falcon Heavy launches*
- Used `requests` + `BeautifulSoup` + `pd.read_html()`
- Worked around a Wikipedia HTTP 403 by setting a browser User-Agent header

### 3. Data Wrangling

- Merged the API and scraped datasets
- Handled missing values and standardized types
- Created the `Class` column (1 = successful landing, 0 = failure)
- Added `Longitude` and `Latitude` for map visualizations
- Final dataset: **90 rows × 18 columns**

## Exploratory Data Analysis (EDA)

Six visualizations produced with Matplotlib and Seaborn:

1. **Flight Number vs Launch Site** — CCAFS SLC 40 is the primary pad
2. **Payload Mass vs Launch Site** — heavier payloads (>8,000 kg) tend to succeed
3. **Landing Success Rate by Orbit** — GTO has the lowest success rate (51.9%)
4. **Flight Number vs Orbit Type** — early flights failed, later flights succeeded
5. **Payload Mass vs Orbit Type** — payload mass alone does not predict success
6. **Yearly Success Trend** — from 0% (2010–2013) to ~85% (2019–2020)

## SQL Analysis

Seven SQL queries run against the dataset using SQLite:

1. Total payload mass by booster block
2. Average payload mass by launch site
3. Number of launches per launch site
4. Total payload mass by launch site
5. Successful vs failed landings
6. Number of launches per year
7. Max payload mass by orbit

## Interactive Visual Analytics

- **Folium Map:** Launch site locations (CCAFS SLC 40, KSC LC 39A, VAFB SLC 4E) with markers and success/failure circles. Saved as `spacex_launch_sites.html`.
- **Plotly Dash Dashboard:** Pie chart of landing outcomes + interactive scatter plot with dropdown filter by launch site.

## Machine Learning Prediction

Four classification models were trained and tuned with `GridSearchCV` (10-fold cross-validation):

| Model | Test Accuracy |
|---|---|
| **Logistic Regression** | **83.3%** |
| SVM | 83.3% |
| KNN | 83.3% |
| Decision Tree | 77.8% |

**Best model:** Logistic Regression — chosen for top accuracy, interpretability, and robustness on this small dataset.

Confusion matrix (Logistic Regression):

```
              Predicted
              Fail   Success
Actual Fail  [  3       3   ]
       Succ  [  0      12   ]
```



## Repository Contents

| File | Description |
|---|---|
| `spacex-falcon9-capstone.ipynb` | Main Jupyter notebook with all code |
| `Data Science Capstone Project Report.pdf` | Full project report |
| `SpaceX_Falcon9_Capstone_Slides.pdf` | Presentation slides |
| `dataset_part_1.csv` | SpaceX API data (107 rows) |
| `dataset_part_2.csv` | Merged + wrangled data (90 rows) |
| `spacex_launch_sites.html` | Interactive Folium map |
| `README.md` | This file |

## Tools & Libraries

- **Python 3** — pandas, numpy
- **Visualization** — matplotlib, seaborn, folium, plotly, dash
- **ML** — scikit-learn
- **Database** — sqlite3
- **Web** — requests, BeautifulSoup4

## How to Run

1. Open `spacex-falcon9-capstone.ipynb` in Google Colab or Jupyter
2. Run all cells top to bottom
3. Outputs (CSVs, charts, map) will be generated in the working directory

## Author

**Ijaz Ahmad**
GitHub: [@Ijaz-Ahmad7](https://github.com/Ijaz-Ahmad7)

## Acknowledgments

- IBM Applied Data Science Capstone (Coursera)
- SpaceX API
- Wikipedia — *List of Falcon 9 and Falcon Heavy launches*
