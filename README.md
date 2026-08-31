# SpaceX Falcon 9 First Stage Landing Prediction

Capstone project for the **IBM Data Science Professional Certificate** (Applied Data Science Capstone).

The goal is to predict whether the first stage of a SpaceX Falcon 9 rocket will land successfully, using publicly available launch data.

---

## Background

SpaceX advertises Falcon 9 launches at roughly **\$62 million**, while competing providers charge upward of **\$165 million** per launch. The difference comes largely from SpaceX reusing the first stage rather than discarding it.

If the first stage lands successfully, the launch is substantially cheaper. Predicting that outcome therefore lets a competitor estimate the true cost of a given launch and bid against SpaceX with better information.

This project takes the role of a data scientist at a hypothetical competitor, working through the full pipeline: data collection, wrangling, exploratory analysis, interactive visualisation, and classification modelling.

### Questions addressed

- Which factors determine whether the first stage lands successfully?
- How do launch site, payload mass, orbit type, and booster version interact with landing outcome?
- Has the landing success rate improved over time?
- Which classification model best predicts landing outcome, and how well does it perform?

---

## Repository structure

```
Applied_Data_Science_Capstone_IBM/
├── notebooks/     analysis notebooks and the Dash app
├── datasets/      CSV inputs and outputs
├── README.md
├── LICENSE
└── .gitignore
```

Notebooks live in `notebooks/` and all CSVs in `datasets/`, so paths inside the notebooks are relative: `../datasets/<file>.csv`.

| # | Notebook / file | Stage |
|---|---|---|
| 1 | `jupyter_labs_spacex_data_collection_api.ipynb` | Data collection via the SpaceX REST API |
| 2 | `jupyter_labs_webscraping.ipynb` | Data collection via web scraping (Wikipedia) |
| 3 | `labs_jupyter_spacex_Data_wrangling.ipynb` | Data wrangling and label creation |
| 4 | `jupyter_labs_eda_sql_coursera_sqllite.ipynb` | Exploratory data analysis with SQL |
| 5 | `jupyter_labs_eda_dataviz_v2.ipynb` | Exploratory data analysis with visualisation |
| 6 | `lab_jupyter_launch_site_location.ipynb` | Interactive map with Folium |
| 7 | `notebooks/spacex_dash_app.py` | Interactive dashboard with Plotly Dash |
| 8 | `SpaceX_Machine_Learning_Prediction_Part_5.ipynb` | Predictive analysis (classification) |

### Data files

| File | Description |
|---|---|
| `datasets/dataset_part_1.csv` | Falcon 9 launch records, 90 rows by 17 columns, 2010-06-04 to 2020-11-05 |
| `datasets/dataset_part_2.csv` | Cleaned data with the `Class` landing label |
| `datasets/dataset_part_3.csv` | Feature-engineered data, one-hot encoded |
| `datasets/spacex_web_scraped.csv` | Falcon 9 launch records scraped from Wikipedia |
| `datasets/spacex_launch_dash.csv` | Input data for the Plotly Dash app |

### Deliverables

- `Data Science Capstone Project Report.pdf` — final presentation

---

## Methodology

### 1. Data collection

Two independent sources were combined:

- **SpaceX REST API** (`api.spacexdata.com/v4`). Launch records were requested as JSON, normalised into a dataframe, and enriched by resolving the booster, launchpad, payload, and core IDs against their respective endpoints. Records were filtered to Falcon 9 only, and missing payload masses were imputed with the column mean.

  The community-run r/SpaceX-API project was archived in June 2026 and its origin now returns a TLS 525 error. Booster version is therefore resolved offline from the three static v4 rocket IDs, and the remaining fields fall back to the equivalent snapshot hosted by the course. The notebook carries the full diagnosis.
- **Web scraping.** Falcon 9 launch records were scraped from the relevant Wikipedia page with `requests` and `BeautifulSoup`, parsing the HTML launch tables into a structured dataframe.

### 2. Data wrangling

Exploratory counts were computed per launch site, orbit, and landing outcome. The various textual landing outcomes were then collapsed into a **binary label `Class`**, where `1` marks a successful first-stage landing and `0` marks any failure or non-attempt. This label is the target variable for the classification stage.

### 3. Exploratory data analysis

- **SQL.** The dataset was loaded into a SQL database and queried for launch site names, payload totals and averages by booster version, first successful ground-pad landing date, boosters matching combined landing and payload conditions, mission outcome counts, maximum-payload boosters, and ranked landing outcomes over a date range.
- **Visualisation.** Scatter, bar, and line plots built with `matplotlib` and `seaborn` covering flight number against launch site, payload against launch site, success rate by orbit type, flight number against orbit type, payload against orbit type, and the yearly success trend. Categorical features were then one-hot encoded and cast to `float64` for modelling.

### 4. Interactive visual analytics

- **Folium.** Launch sites were marked on a global map, launch outcomes were displayed as colour-coded clustered markers, and distances from a selected site to nearby coastline, railway, highway, and city were measured and drawn.
- **Plotly Dash.** A dashboard with a launch-site dropdown, a success-count pie chart, a payload range slider, and a payload against launch-outcome scatter plot coloured by booster version.

### 5. Predictive analysis

The feature matrix was standardised with `StandardScaler` and split into training and test sets. Four classifiers were tuned with `GridSearchCV` over 10-fold cross-validation:

- Logistic Regression
- Support Vector Machine
- Decision Tree
- K-Nearest Neighbours

Each model was scored on cross-validation accuracy and on held-out test accuracy, and the best performer was inspected via its confusion matrix.

---

## Results

> **TODO:** fill in from your own notebook output. Do not copy numbers from anywhere else, they vary between runs.

| Model | Best CV accuracy | Test accuracy |
|---|---|---|
| Logistic Regression | | |
| SVM | | |
| Decision Tree | | |
| KNN | | |

> **TODO:** name the best-performing model and summarise what its confusion matrix shows, in particular how it handles false positives.

### Key findings

> **TODO:** replace with your own conclusions. Typical themes to check against your plots:
>
> - how success rate changes with flight number (experience effect)
> - which orbit types show the highest success rates
> - how payload mass relates to outcome, and whether that differs by site
> - the trajectory of the yearly success rate

---

## Tech stack

`Python 3` · `pandas` · `numpy` · `requests` · `BeautifulSoup` · `matplotlib` · `seaborn` · `folium` · `plotly` · `dash` · `scikit-learn` · `SQLAlchemy` / `sqlite3` · `Jupyter`

---

## Running the project

```bash
git clone https://github.com/SergejGri/Applied_Data_Science_Capstone_IBM.git
cd Applied_Data_Science_Capstone_IBM

python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate

pip install -r requirements.txt
jupyter lab
```

Run the notebooks in the order listed under [Repository structure](#repository-structure), as later stages depend on the CSV files written by earlier ones.

To launch the dashboard:

```bash
python notebooks/spacex_dash_app.py
```

Then open `http://127.0.0.1:8050` in a browser.

---

## Author

> **TODO:** your name, and a link to your GitHub or LinkedIn profile if you want it there.

## Acknowledgements

Course materials and lab templates by [IBM Skills Network](https://www.ibm.com/training/skills-network) via Coursera. Launch data from the [SpaceX REST API](https://github.com/r-spacex/SpaceX-API) and Wikipedia.
