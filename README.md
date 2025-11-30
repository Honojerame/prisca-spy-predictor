# PRISCA - SPY Next-Day Opening Price Predictor

**Predictive Real-time Investment Strategy & Capital Allocation**

A machine learning-powered web application that predicts the next-day opening price of SPY (S&P 500 ETF) using XGBoost, sentiment analysis, and technical indicators.

---

## 🎯 Project Overview

PRISCA is an end-to-end ML system that:
- Predicts SPY opening prices with **95.6% R² accuracy**
- Analyzes **53,330+ financial news headlines** using VADER and FinBERT sentiment
- Engineers **43 technical features** from price data
- Serves real-time predictions via **FastAPI backend**
- Displays insights through **interactive web dashboard**

---

## 📊 Model Performance

| Metric | Value |
|--------|-------|
| **MAE** | $3.71 |
| **RMSE** | $5.23 |
| **MAPE** | 1.40% |
| **R²** | 0.956 |

**Improvement over baseline**: 19.2% reduction in MAE

---

## 🏗️ Project Structure

```
PRISCA/
├── backend/
│   ├── app.py                 # FastAPI server
│   └── requirements.txt       # Python dependencies
├── frontend/
│   └── index.html            # Web dashboard
├── notebooks/
│   ├── Salesforce_1b_data_exploration and preparation.ipynb
│   └── XGB_Regressor.ipynb   # Model training
├── models/
│   ├── prisca_xgb_model.pkl  # Trained model
│   ├── feature_list.json     # Feature names
│   └── model_metadata.json   # Model info
├── data/
│   └── final_modeling_dataset.csv  # Training data
├── enhanced_visualizations.py      # Plotly charts
├── PRISCA_ARCHITECTURE.md          # System design
├── QUICKSTART.md                   # Setup guide
└── MODEL_TRAINING_SUMMARY.md       # Training details
```

---

## 🚀 Quick Start

### 1. Install Dependencies

```powershell
cd backend
pip install -r requirements.txt
```

### 2. Start Backend Server

```powershell
cd backend
python app.py
```

Server runs at: `http://localhost:8000`

### 3. Open Frontend Dashboard

```powershell
cd frontend
# Open index.html in browser or use:
python -m http.server 3000
```

Dashboard available at: `http://localhost:3000`

### 4. Test API

```powershell
# Health check
curl http://localhost:8000/health

# Get prediction
curl -X POST http://localhost:8000/predict \
  -H "Content-Type: application/json" \
  -d "{}"
```

---

## 📡 API Endpoints

### `GET /health`
Health check and model status

### `POST /predict`
Generate next-day prediction

**Request:**
```json
{
  "date": "2025-11-30",  // optional
  "include_explanation": false  // optional
}
```

**Response:**
```json
{
  "prediction": 305.67,
  "prediction_date": "2025-12-01",
  "confidence_interval": {
    "lower": 297.93,
    "upper": 313.41
  },
  "current_price": 304.25,
  "predicted_change": 1.42,
  "predicted_change_pct": 0.47,
  "model_version": "XGBoost (Tuned)",
  "timestamp": "2025-11-30T10:30:00"
}
```

### `GET /model/info`
Model metadata and performance metrics

### `GET /features`
List of 43 features used by model

---

## 🧠 Model Features

### Price Data (5 features)
- Open, High, Low, Close, Volume

### Technical Indicators (12 features)
- Moving Averages (MA_5, MA_20)
- Volatility (5/10/20 day)
- ATR, Momentum

### Lagged Features (11 features)
- Close lags (1/2/3 days)
- Returns (1/2/3/5/10 day)

### Sentiment Scores (7 features)
- VADER: compound, positive, negative, neutral
- FinBERT: positive, negative, neutral

### Calendar Features (6 features)
- Day of week, month, day of month, week of month
- Month end, quarter end flags

---

## 🔬 Model Training

The model was trained using:
- **Algorithm**: XGBoost Regressor
- **Dataset**: 619 samples (2018-2020)
- **Training**: 496 samples (80%)
- **Holdout**: 123 samples (20%)
- **Hyperparameter Tuning**: GridSearchCV (243 combinations)

**Best Hyperparameters:**
- n_estimators: 300
- max_depth: 4
- learning_rate: 0.05
- subsample: 0.8
- colsample_bytree: 0.8

See `MODEL_TRAINING_SUMMARY.md` for complete details.

---

## 📈 Key Features by Importance

1. **Close_SPY** - Previous closing price
2. **Low_SPY** - Previous low price
3. **Open_SPY** - Previous opening price
4. **High_SPY** - Previous high price
5. **Close_lag1** - 1-day lagged close
6. **MA_20** - 20-day moving average
7. **Volatility_20** - 20-day volatility

---

## 🎨 Visualizations

The project includes 7 interactive Plotly visualizations:
- Candlestick chart with volume
- Sentiment impact chart
- Volatility heatmap
- Feature importance chart
- Prediction confidence gauge
- Correlation network
- Prediction summary card

See `enhanced_visualizations.py`

---

## 🛠️ Technology Stack

**Backend:**
- FastAPI (API server)
- XGBoost (ML model)
- SHAP (explainability)
- yFinance (market data)
- VADER + FinBERT (sentiment)

**Frontend:**
- HTML5 + Tailwind CSS
- Plotly.js (charts)
- Vanilla JavaScript

**ML/Data:**
- Pandas, NumPy, Scikit-learn
- Jupyter Notebooks
- Python 3.12

---

## 📝 Development Roadmap

### ✅ Phase 1: Complete
- [x] Data collection (SPY prices + news)
- [x] Feature engineering
- [x] Sentiment analysis (VADER + FinBERT)
- [x] Model training and tuning
- [x] Model export

### ✅ Phase 2: Complete
- [x] FastAPI backend
- [x] Basic frontend dashboard
- [x] Documentation

### 🔄 Phase 3: In Progress
- [ ] Real-time news sentiment integration
- [ ] Historical chart visualization
- [ ] Feature importance display
- [ ] Deployment to cloud

### 📋 Phase 4: Planned
- [ ] User authentication
- [ ] Prediction history tracking
- [ ] Email/SMS alerts
- [ ] Multi-asset support (QQQ, IWM, DIA)

---

## ⚠️ Limitations & Disclaimers

1. **Historical Data**: Trained on 2018-2020 data
2. **Single Asset**: Only predicts SPY
3. **Educational Purpose**: Not financial advice
4. **Mock Sentiment**: Uses neutral sentiment in demo (replace with real news API)
5. **Market Hours**: Predictions assume normal trading conditions

---

## 🤝 Contributing

This is an educational project. Suggestions and improvements welcome!

---

## 📄 License

This project is for educational purposes. 

---

## 🙏 Acknowledgments

- **Data Sources**: yFinance, Kaggle (Financial News)
- **Models**: XGBoost, VADER, FinBERT (ProsusAI)
- **Frameworks**: FastAPI, Plotly, Tailwind CSS

---

## 📧 Contact

For questions about this project, please refer to the documentation files:
- `QUICKSTART.md` - Setup instructions
- `PRISCA_ARCHITECTURE.md` - System architecture
- `MODEL_TRAINING_SUMMARY.md` - Model details

---

**Built with ❤️ for Cornell AI Studio Project - Fall 2025**
