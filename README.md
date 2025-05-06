# ⚡ Electricity Consumption Forecasting with Time Series Machine Learning

This project forecasts daily electricity usage for 370 Portuguese clients using advanced time series models, clustering techniques, and feature engineering. It was completed as part of a graduate-level forecasting course at Columbia University.

---

## 📦 Project Overview

This end-to-end pipeline focuses on:
- Preprocessing high-frequency consumption data
- Integrating weather features
- Applying clustering via Dynamic Time Warping (DTW)
- Training multi-series forecasting models using:
  - SARIMAX
  - LSTM (PyTorch Lightning)
  - Histogram-based Gradient Boosting (skforecast)

The final model achieved a **30.20% mean MAPE** across clients, with a **median MAPE of 4.19%**, demonstrating high accuracy and strong generalization.

---

## 📂 Files

- `Final_code.ipynb`: Full notebook containing data loading, feature engineering, modeling, and evaluation.
- `lisbon.csv`: Lisbon weather data (external features).
- `Forecasting Project Deliverable 2 Report.pdf`: Technical write-up with background, methodology, and analysis.
- `Forecasting Project Slides Deliverable 2.pdf`: Final presentation slides.

---

## 🧠 Problem & Business Value

Electricity demand fluctuates constantly. Without accurate forecasting:
- Providers may over- or under-produce
- Grid stability may suffer
- Operational and environmental costs increase

This project helps:
- Reduce overproduction and outages
- Support smarter grid planning and pricing
- Enable better integration of renewable sources

---

## 🔍 Data Description

### Main Dataset (UCI Electricity Load Diagrams)
- 370 clients in Portugal
- 15-minute electricity usage from 2012 to 2014
- Converted to daily kWh

### External Dataset (Lisbon Weather)
- Daily weather data: min/max temp, sun hours, humidity, etc.
- Merged by date to enhance model inputs

---

## 🧹 Data Preprocessing

- Removed 2011 data with excessive missing values
- Adjusted for daylight saving time
- Aggregated to daily consumption
- Created per-client time series dictionary (`series_dict`)
- Generated exogenous features (`exog_dict`) with:
  - Temperature
  - Day-of-week/year (sine/cosine encoding)
  - Cluster label (from DTW)

---

## 🤝 Clustering with DTW

Used **Dynamic Time Warping** to segment clients into 5 distinct clusters based on usage pattern similarity. Cluster labels were used as features to improve forecasting performance.

| Cluster | Characteristics | Size |
|---------|------------------|------|
| 0       | Steady usage     | 280  |
| 1       | Highly fluctuating | 7  |
| 2       | Gradual upward trend | 53 |
| 3       | Decline after spike | 2 |
| 4       | Periodic patterns | 28  |

---

## 🔧 Models & Configuration

### SARIMAX
- Separate model per cluster (selected clients)
- Captures weekly seasonality and uses temperature as an external regressor
- Average MAPE: **160.55%**, struggled on noisy clients

### LSTM (PyTorch Lightning)
- Global model trained across all clients
- Used 30-day lookback, 7-day prediction
- MAPE: **135.94%**
- Captured stable trends but struggled with irregular patterns

### Histogram-based Gradient Boosting (Best)
- Recursive forecasting with sliding windows
- Input: 30 past days → predict next 7
- Robust to outliers and irregular usage
- Mean MAPE: **30.20%**, Median MAPE: **4.19%**

---

## 📈 Results Summary

| Model                       | Mean MAPE | Median MAPE |
|----------------------------|-----------|--------------|
| SARIMAX                    | 160.55%   | —            |
| LSTM                       | 135.94%   | —            |
| **HistGradientBoosting** ✅ | **30.20%** | **4.19%**     |

- **Gradient boosting outperformed** other models in both accuracy and robustness.
- Performance was consistent across clusters, with longer forecast horizons introducing expected error increases.

---

## 🚧 Challenges & Future Work

- **Cluster imbalance**: Cluster 0 contained ~75% of clients.
- **Limited features**: Original data lacked metadata about clients.
- Future improvements:
  - Use richer clustering techniques (e.g., KShape, Autoencoder)
  - Add more contextual features
  - Explore hybrid models (e.g., LSTM + boosting ensemble)

---

## 👥 Team

- Ziying Song  
- Yueer Liu  
- Zimeng (Sandy) Li  
- Yihan Yang

---

## 📚 References

- UCI Electricity Load Dataset: https://archive.ics.uci.edu/dataset/321/electricityloaddiagrams20112014  
- Lisbon Weather: https://www.kaggle.com/datasets/luisvivas/spain-portugal-weather  
- Multi-Series Forecasting with skforecast: https://cienciadedatos.net/documentos/py44-multi-series-forecasting-skforecast

---

## 📄 License

This project is released for academic purposes.  
Contact `zs2698@columbia.edu` for inquiries.

