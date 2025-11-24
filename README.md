# 🛵 Food Delivery Route Efficiency Prediction Model

This project aims to develop a **Linear Regression** model to determine the key factors affecting delivery time (**`delivery_time_min`**) and predict this duration, using data from a food delivery company.

---

## 🎯 Project Goal

The primary goal is to optimize logistics operations and enhance customer satisfaction by accurately predicting the delivery time based on variables such as order distance, traffic, weather conditions, and the delivery vehicle used.

---

## 🔍 Dataset and Features

The dataset used includes 200 delivery records.

| Feature Name | Description | Type |
| :--- | :--- | :--- |
| `distance_km` | Straight-line distance between the restaurant and customer (km). | Numerical |
| `route_length_km` | The actual route distance covered by the delivery person (km). | Numerical |
| **`delivery_time_min`** | Time taken for the order to be delivered (Minutes) - **Target Variable (Y)** | Numerical |
| `traffic_level` | Level of congestion (Low, Medium, High). | Categorical |
| `delivery_mode` | Delivery vehicle used (Car, Bike, Scooter, Bicycle). | Categorical |
| `weather` | Weather condition during delivery (Clear, Rainy, Cloudy, Windy). | Categorical |
| `order_time` | Exact date and time when the order was placed. | Time Series |
| `restaurant_zone` | Zone where the restaurant is located. | Categorical |
| `customer_zone` | Zone where the customer is located. | Categorical |

---

## ⚙️ Feature Engineering Steps

The following preprocessing and engineering steps were applied to the raw data to maximize the model's performance:

### 1. Categorical Data Encoding

* **Ordinal Encoding:** The **`traffic_level`** column was numerically encoded due to its natural order:
    * `Low`: **0**
    * `Medium`: **1**
    * `High`: **2**
* **One-Hot Encoding:** The **`delivery_mode`** and **`weather`** nominal categorical columns were converted into binary (dummy) variables that the model can use (e.g., `delivery_mode_Car`, `weather_Rainy`).

### 2. Time Series Feature Extraction

The `order_time` column was parsed and new numerical features that could affect delivery time were extracted, as the original format could not be used directly:

* `order_month`
* `order_day`
* `order_hour` (A critical feature indicating if the delivery occurred during peak hours)
* `order_minute`

### 3. Feature Selection

Columns deemed irrelevant or redundant after engineering were dropped from the dataset:

* `order_id`
* `order_time` (Replaced by the extracted hour/day/minute features)
* `customer_zone` and `restaurant_zone` (Removed at this stage to reduce model complexity.)

---

## 🧪 Model Training and Results

The prepared dataset was split into 75% training and 25% testing sets, and a **Linear Regression** model was trained.

### Scaling

Before training the model, numerical features were standardized using the **`StandardScaler`**.

$$
Z = \frac{(X - \mu)}{\sigma}
$$

### Performance Metrics

Model's test data performance was evaluated using the following metrics:

| Metric | Value | Description |
| :--- | :--- | :--- |
| **MSE** (Mean Squared Error) | 72.76 | The average of the squared errors. |
| **MAE** (Mean Absolute Error) | **6.41** | The average magnitude of errors (Minutes). |
| **RMSE** (Root Mean Squared Error) | **8.53** | The square root of MSE, sensitive to outliers (Minutes). |
| **R² Score** (Coefficient of Determination) | **0.892** | Indicates the proportion of the variance in the target variable explained by the model. |
| **Adjusted R² Score** | **0.844** | R² adjusted for the number of