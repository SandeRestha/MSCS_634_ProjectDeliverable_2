# Deliverable 2 – Regression Modeling & Performance Evaluation  
**Course:** Advanced Data Mining (MSCS 634)  
**Student:** Sandesh Shrestha  
**Dataset:** Walmart Weekly Sales (Kaggle)

---

## 1. Dataset Overview

This project uses the **Walmart Sales** dataset from Kaggle, which contains weekly sales records across multiple Walmart stores.  
The dataset includes:

- **Store ID**  
- **Date** (week start)  
- **Weekly_Sales** (target variable)  
- **Holiday_Flag**  
- **Temperature**  
- **Fuel_Price**  
- **Consumer Price Index (CPI)**  
- **Unemployment Rate**

It contains several thousand rows and 8+ original features, with strong temporal and economic components that make it suitable for regression modeling.

---

## 2. Modeling Process

### **2.1 Data Cleaning**
Following Deliverable 1 practices:
- Converted `Date` to datetime  
- Stripped whitespace from text fields  
- Converted numeric fields to numeric types  
- Handled missing values (median for numeric, mode for categorical)  
- Removed duplicates  
- Applied **IQR-based outlier capping** for key numeric columns

---

### **2.2 Feature Engineering**
To improve model performance, several meaningful features were added:

#### **Temporal Features**
- `Year`, `Month`, `WeekOfYear`  
- `Is_Month_Start`, `Is_Month_End`

#### **Seasonality**
- `Season` (Winter, Spring, Summer, Fall)

#### **Economic Interaction Features**
- `CPI_Unemployment_Ratio`  
- `Temp_Fuel_Interaction`

These engineered features capture **seasonal patterns**, **monthly shopping cycles**, and **economic effects** that influence retail behavior.

---

### **2.3 Model Development**
Two regression models were implemented:

1. **Linear Regression** (baseline)
2. **Ridge Regression** (L2 regularization)

Both models used the same preprocessing pipeline:

- **StandardScaler** for numeric features  
- **OneHotEncoder** for categorical features (`Store`, `Season`)  
- Combined using a `ColumnTransformer` and `Pipeline` for reproducibility and consistency

---

## 3. Model Evaluation Results

Both models were assessed using train/test split **and** 5-fold cross-validation.

### **Hold-Out Test Performance**

| Model | R² (Test) | RMSE (Test) |
|-------|-----------|--------------|
| **Linear Regression** | ~0.9475 | ~128,057 |
| **Ridge Regression** | Slightly lower | ~133,528 |

Linear Regression consistently outperformed Ridge Regression, achieving:

- **Higher R²**  
- **Lower RMSE & MSE**  
- **Better generalization**  

### **Cross-Validation Performance**
Linear Regression achieved:

- **Lower mean CV_RMSE**  
- **Higher mean CV_R²**  
- Very low standard deviations (≈ 0.003), indicating stability

This confirms that Linear Regression is the more reliable model for this dataset.

---

## 4. Key Insights & Observations

- **Holiday weeks show significantly higher average sales**, confirming `Holiday_Flag` is an important predictor.  
- **Temporal and seasonal patterns are strong drivers** of Walmart weekly sales, reflected in the effectiveness of engineered features.  
- The relationship between predictors and `Weekly_Sales` is **highly linear**, which is why Linear Regression performs extremely well.  
- **Ridge regularization provides no benefit** here and slightly reduces accuracy, indicating minimal multicollinearity and a stable feature space.  
- **Residual plots show no major patterns**, suggesting model assumptions are reasonably satisfied.

---

## 5. Challenges & How They Were Addressed

### **1. Handling Outliers**
- Weekly sales had extreme spikes during peak seasons.  
- **Solution:** IQR-based capping preserved all rows while reducing the influence of extreme values.

### **2. Feature Construction Decisions**
- Needed to choose which temporal and interaction features would improve model performance.  
- **Solution:** Added seasonal, monthly, and economic interaction features and evaluated their impact.

### **3. Model Over-Regularization**
- Ridge Regression initially underperformed because regularization shrank useful coefficients.  
- **Solution:** Evaluated both models side-by-side using metrics and CV to justify selecting Linear Regression.

### **4. Maintaining Model Assumptions**
- Retail data can violate homoscedasticity.  
- **Solution:** Used residual plots to validate linearity and ensure no strong patterns remained.

---

## 6. Conclusion

This deliverable implemented a complete regression modeling pipeline that successfully predicts weekly Walmart sales.  
Through strong feature engineering, robust evaluation, and clear comparison of two regression models, the analysis shows that:

> **Linear Regression is the best-performing and most reliable model for this dataset.**

---
