# 🏡 New York City Airbnb Price Prediction

## 📌 Overview

This project develops a machine learning model to **predict Airbnb listing prices in New York City** using publicly available data from 2019. The goal is to assist local Airbnb hosts in pricing their properties more accurately based on factors such as location, room type, availability, and review history.

Unlike traditional hotels with expert pricing strategies, many individual Airbnb hosts lack access to real-time market insights. By leveraging data science and machine learning, this project aims to bridge that gap and create a robust, interpretable pricing tool.

---

## 📁 Folder Structure

- `data/`: Raw and processed datasets (NYC Airbnb Open Data 2019).
- `figures/`: Visualizations for EDA, correlation maps, SHAP values, etc.
- `models/`: Saved models, metrics, and tuning results.
- `report/`: Final write-ups and analysis reports.
- `src/`: All source code including preprocessing, EDA, modeling, and evaluation scripts.

---

## 📊 Dataset

- **Source**: [Inside Airbnb](http://insideairbnb.com/get-the-data.html), also available on [Kaggle](https://www.kaggle.com/datasets/dgomonov/new-york-city-airbnb-open-data)
- **Size**: 48,895 rows and 16 columns
- **Year**: 2019  
- **Notable Columns**: `price`, `room_type`, `neighbourhood`, `availability_365`, `last_review`, `reviews_per_month`

---

## 🔍 Exploratory Data Analysis (EDA)

- Price is **highly right-skewed** with a mode around $100 and a max of $10,000.
- Extreme outliers and invalid entries (e.g., price = $0) were removed to focus on general hosts.
- Strong location-price patterns observed: **Downtown Manhattan and North Brooklyn** listings tend to be more expensive.
- Negative correlation observed between price and `reviews_per_month` — **higher-priced listings receive fewer reviews**.

---

## 🛠️ Preprocessing & Feature Engineering

- Dropped identifiers (`id`, `host_id`) and unstructured text fields (`name`, `host_name`).
- Converted `last_review` into a numerical feature: days since last review.
- Missing values handled using:
  - **XGBoost's built-in handling**
  - **Pattern submodel training** for other models
- Applied:
  - `OrdinalEncoder` for `room_type`
  - `OneHotEncoder` for neighborhoods
  - `StandardScaler` for numerical variables
  - `MinMaxScaler` for availability (0–365 days)

---

## 🤖 Models & Methodology

- Data split: 60% Train, 20% Validation, 20% Test (no stratification, no k-fold due to size)
- Models trained:
  - **Random Forest**
  - **Ridge Regression**
  - **K-Nearest Neighbors**
  - **XGBoost** (best performer)

### 🔧 Hyperparameter Tuning
- Performed using log-spaced and linear-spaced grids
- Evaluated using **Root Mean Squared Error (RMSE)** for interpretability (USD units)

---

## 🏆 Results

- **XGBoost** outperformed all other models with a test RMSE of **\$56/night**
- Reduced prediction error by **\$26/night** compared to the baseline
- Top features (by SHAP and permutation importance):
  - `room_type`
  - `longitude`
  - `latitude`

### 📉 Feature Impact
- Room type had the **strongest impact**: shared rooms reduced predicted price, while private/entire rooms increased it.
- Minimum night requirement had minimal influence on final predictions.

---

## 📈 Future Improvements

- Apply **log transformation** to price for better linear model performance
- Tune more hyperparameters in XGBoost
- Explore additional algorithms (e.g., **SVM**) and **advanced NLP** for listing descriptions
- Update dataset with **more recent Airbnb data**

---

## 📚 References

1. [NYC Airbnb Open Data on Kaggle](https://www.kaggle.com/datasets/dgomonov/new-york-city-airbnb-open-data)  
2. [Inside Airbnb](http://insideairbnb.com/)  
3. [CMU Capstone Airbnb Pricing Analysis](https://www.stat.cmu.edu/capstoneresearch/spring2021/315files/team5.html)  
4. [Airbnb Availability Prediction on Kaggle](https://www.kaggle.com/code/gcdatkin/nyc-airbnb-availability-prediction)

---

## 📬 Author

**Hanjun Wei**  
