# Mental Health Score Analyzer

Predicts a student's mental health score (0–10) from demographic details and social-media/digital-habit data. A scikit-learn regression model is trained on survey data, served through a FastAPI backend, and consumed by a lightweight HTML/CSS/JS frontend.

## How it works

1. **Model training** (`ML_Project.ipynb`) — explores and cleans `Student Social Media And Mental Health Impact.csv`, engineers features, and trains a regression model to predict a mental health score. The trained model is exported with `joblib` as `Mental_Health_Model.pkl`.
2. **API** (`main.py`) — a FastAPI app that loads the pickled model and exposes a `/predict` endpoint. It validates incoming data with Pydantic, groups uncommon countries into `"Other"`, builds a feature row, and returns the predicted score.
3. **Frontend** (`index.html`, `style.css`, `script.js`) — a simple form for entering the input data, which POSTs to the API and displays the predicted score.

## Project structure

```
├── ML_Project.ipynb                                  # Data exploration + model training
├── ML Project.html                                    # Notebook exported as HTML
├── Mental_Health_Model.pkl                             # Trained model (joblib)
├── Student Social Media And Mental Health Impact.csv   # Training dataset
├── main.py                                             # FastAPI backend (/predict endpoint)
├── requirements.txt                                    # Backend dependencies
├── index.html                                          # Frontend form
├── style.css
└── script.js
```

## Input features

| Field | Type | Notes |
|---|---|---|
| `age` | int | 10–100 |
| `gender` | enum | `Male`, `Female` |
| `country` | string | Grouped into top 10 countries + `Other` |
| `academic_level` | enum | `Undergraduate`, `Graduate`, `High School` |
| `most_used_platform` | enum | e.g. `Instagram`, `TikTok`, `WhatsApp`, `YouTube`, etc. |
| `purpose_of_use` | enum | `Networking`, `Education`, `Entertainment`, `News` |
| `avg_daily_usage_hours` | float | 0–24 |
| `daily_unlocks` | int | ≥ 0 |
| `study_hours` | float | 0–24 |
| `physical_activity_hours` | float | 0–24 |
| `sleep_hours_per_night` | float | 0–24 |
| `stress_level` | enum | `Low`, `Medium`, `High`, `Very High` |

## Tech stack

- **Backend:** FastAPI, Pydantic, scikit-learn, pandas, joblib, Uvicorn
- **Frontend:** HTML, CSS, JavaScript (no framework)
- **Modeling:** Jupyter Notebook / scikit-learn

## Getting started

### 1. Clone and install dependencies

```bash
git clone https://github.com/Saksham-yadav710/mental-health-analyzer.git
cd mental-health-analyzer
pip install -r requirements.txt
```

### 2. Run the API

```bash
uvicorn main:app --reload --port 8000
```

The API will be available at `http://localhost:8000`, with the prediction endpoint at `http://localhost:8000/predict`.

### 3. Run the frontend

Serve the static files (e.g. with Python's built-in server) and open the page in your browser:

```bash
python -m http.server 5500
```

Then visit `http://localhost:5500`. By default the frontend calls the deployed API; to point it at your local backend instead, add an `api` query parameter:

```
http://localhost:5500?api=http://localhost:8000
```

## API reference

**POST** `/predict`

Request body:

```json
{
  "age": 21,
  "gender": "Female",
  "country": "India",
  "academic_level": "Undergraduate",
  "most_used_platform": "Instagram",
  "purpose_of_use": "Entertainment",
  "avg_daily_usage_hours": 5.5,
  "daily_unlocks": 60,
  "study_hours": 3,
  "physical_activity_hours": 1,
  "sleep_hours_per_night": 6.5,
  "stress_level": "High"
}
```

Response:

```json
{
  "predicted_mental_health_score": 6.77
}
```

## Retraining the model

Open `ML_Project.ipynb` to see the full data cleaning, feature engineering, and training pipeline. Re-run it against an updated dataset and re-export with `joblib.dump(...)` to refresh `Mental_Health_Model.pkl`, then restart the API.

## License

MIT — feel free to adapt and improve.
