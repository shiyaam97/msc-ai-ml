# 🚗 Sri Lankan Car Price Predictor & Explainability (XAI)
### MSc in Artificial Intelligence — Machine Learning Assignment

An end-to-end Machine Learning and Explainable AI (XAI) system predicting used vehicle market valuations in Sri Lanka using **Explainable Boosting Machine (EBM)**, an algorithm not covered in foundational lectures. The system is served via a lightweight **Python Flask API** and features a responsive **HTML5/CSS/JavaScript front-end** that visualizes prediction results alongside local feature attributions.

---

## 📊 Model Evaluation Summary

Evaluated on an 80/20 train/test split from Sri Lankan vehicle listings (Ikman.lk & Riyasewana via Kaggle):

| Metric | Score | Analysis |
| :--- | :--- | :--- |
| **Model** | **Explainable Boosting Machine (EBM)** | Microsoft Research Generalized Additive Model ($GA^2M$) |
| **$R^2$ Score** | **0.7334** | Explains 73.34% of variance in market valuations |
| **MAE** | **LKR 788,279** | Mean Absolute Error across test listings |
| **RMSE** | **LKR 2,302,109** | Penalizes severe outlier errors on luxury vehicles |

---

## 📁 Repository Structure
```text
msc-ai-car-price-predictor/
├── ml/
│   ├── train.py                  # End-to-end data processing, EBM training & SHAP/XAI plots
│   ├── holdout_test_row.csv      # Reserved unseen vehicle for inference verification
│   └── requirements.txt          # Training dependencies
├── backend/
│   ├── app.py                    # Flask REST API handling /predict & XAI generation
│   ├── ebm_model.pkl             # Trained EBM model artifact
│   ├── encoder.pkl               # Fitted OrdinalEncoder artifact
│   └── requirements.txt          # API dependencies
├── frontend/
│   ├── index.html                # Responsive web form & prediction view
│   ├── style.css                 # Clean CSS styling
│   └── app.js                    # Fetch API integration to Flask
├── .gitignore
└── README.md
```

---

## 🚀 Quick Start Guide

### 1. Install Dependencies
```bash
pip install -r backend/requirements.txt -r ml/requirements.txt
```

### 2. Train Model & Export Visualizations
```bash
cd ml
python train.py
cd ..
```
*Outputs evaluation metrics, saves `ml/shap_summary.png`, `ml/shap_waterfall.png`, and exports serialized artifacts to `backend/`.*

### 3. Launch Flask Backend
```bash
cd backend
python app.py
```
*Runs on `http://127.0.0.1:5000`.*

### 4. Open the Web Frontend
Open `frontend/index.html` in Google Chrome, Edge, or Firefox. Enter specifications or populate with values from `ml/holdout_test_row.csv` to verify real-time price estimation and interactive XAI explanations!

---

## ☁️ GitHub CI/CD Deployment

The GitHub Actions workflow in `.github/workflows/ci.yml` installs the API dependencies, checks Python syntax, and verifies that the API loads the model. The Render Blueprint in `render.yaml` deploys the frontend and API as one web service and uses `/health` for health checks.

1. Push the project to a GitHub repository. Include `backend/ebm_model.pkl` and `backend/encoder.pkl`; the `.gitignore` allows these model artifacts to be committed.
2. In Render, choose **New +** then **Blueprint**, connect the GitHub repository, and deploy the detected `render.yaml` service.
3. After the first deploy, visit `https://<your-render-service>.onrender.com/health`. A healthy response includes `"model_loaded": true`.

Each push to `main` runs CI on GitHub. Render can automatically deploy the same branch after a successful change. The first build may take several minutes while Python dependencies are installed.
