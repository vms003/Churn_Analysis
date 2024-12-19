# Customer Churn Prediction Project

## 🚀 Overview
This project aims to predict customer churn and analyze the factors driving it. By leveraging **SQL**, **Python**,**PowerBI** and **Machine Learning**, the project provides actionable insights and a user-friendly dashboard to enhance customer retention strategies. 

---

## 🛠️ Project Components

### 1. **SQL: Data Exploration and Transformation**
#### 🔍 Data Exploration Queries
```sql
-- Count Gender Distribution
SELECT Gender, COUNT(Gender) AS TotalCount,
       COUNT(Gender) * 1.0 / (SELECT COUNT(*) FROM stg_Churn) AS Percentage
FROM stg_Churn
GROUP BY Gender;

-- Contract Analysis
SELECT Contract, COUNT(Contract) AS TotalCount,
       COUNT(Contract) * 1.0 / (SELECT COUNT(*) FROM stg_Churn) AS Percentage
FROM stg_Churn
GROUP BY Contract;

-- Customer Revenue Contribution
SELECT Customer_Status, COUNT(Customer_Status) AS TotalCount, 
       SUM(Total_Revenue) AS TotalRev,
       SUM(Total_Revenue) / (SELECT SUM(Total_Revenue) FROM stg_Churn) * 100 AS RevPercentage
FROM stg_Churn
GROUP BY Customer_Status;
```

#### 🧹 Null Handling and Transformation
```sql
SELECT 
    Customer_ID,
    Gender,
    Age,
    Married,
    State,
    Number_of_Referrals,
    Tenure_in_Months,
    ISNULL(Value_Deal, 'None') AS Value_Deal,
    Phone_Service,
    ISNULL(Churn_Reason, 'Others') AS Churn_Reason
INTO prod_Churn
FROM stg_Churn;
```

#### 📄 Views for Power BI Integration
```sql
CREATE VIEW vw_ChurnData AS
SELECT * FROM prod_Churn WHERE Customer_Status IN ('Churned', 'Stayed');

CREATE VIEW vw_JoinData AS
SELECT * FROM prod_Churn WHERE Customer_Status = 'Joined';
```

---

### 2. **Power BI: Data Transformation and Visualization**
#### 🔄 Power Query Transformations
- **Custom Columns:**
  - `Churn Status = if [Customer_Status] = "Churned" then 1 else 0`
  - `Monthly Charge Range = if [Monthly_Charge] < 20 then "< 20" else if [Monthly_Charge] < 50 then "20-50" else if [Monthly_Charge] < 100 then "50-100" else "> 100"`

- **Mapping Tables:**
  - **Age Group:**
    ```
    Age Group = if [Age] < 20 then "< 20" else if [Age] < 36 then "20 - 35" else if [Age] < 51 then "36 - 50" else "> 50"
    AgeGrpSorting = if [Age Group] = "< 20" then 1 else if [Age Group] = "20 - 35" then 2 else ...
    ```
  - **Tenure Group:**
    ```
    Tenure Group = if [Tenure_in_Months] < 6 then "< 6 Months" else ...
    ```

#### 📊 Key Measures
```DAX
-- Summary Page Metrics
Total Customers = COUNT(prod_Churn[Customer_ID])
New Joiners = CALCULATE(COUNT(prod_Churn[Customer_ID]), prod_Churn[Customer_Status] = "Joined")
Total Churn = SUM(prod_Churn[Churn Status])
Churn Rate = [Total Churn] / [Total Customers]

-- Prediction Page Metrics
Count Predicted Churners = COUNT(Predictions[Customer_ID])
Title Predicted Churners = "COUNT OF PREDICTED CHURNERS : " & COUNT(Predictions[Customer_ID])
```

---

### 3. **Python: Data Preprocessing and Model Building**
#### 🧪 Data Exploration
- **Validation:** Checking for distinct values and nulls.
- **Visualization:** Understanding churn patterns.

#### 🔧 Data Preprocessing Pipeline
- Handling missing values.
- Encoding categorical variables.
- Scaling numerical features.

#### 🤖 Churn Prediction Model
- **Algorithm:** Random Forest Classifier
- **Metrics:** Accuracy, Precision, Recall, F1 Score

#### 📂 Export Results
- Predictions are saved as a CSV file and integrated into Power BI.

---

## 🖥️ How to Use
1. **SQL:**
   - Execute the SQL queries in sequence for data exploration, cleaning, and transformation.
2. **Power BI:**
   - Import the SQL views and transformed tables.
   - Apply Power Query transformations.
   - Build visuals using the provided DAX measures.
3. **Python:**
   - Run the Python script for preprocessing and model training.
   - Export predictions for Power BI integration.

---

## 📈 Output
### Descriptive Analytics
- Insights on churn trends based on demographics, services, and contract types.

### Predictive Analytics
- Identified customers at risk of churning.

### Recommendations
- Strategies to retain customers based on churn drivers.

---

## 📊 Visuals
### Summary Dashboard
- **Metrics:**
  - Total Customers
  - New Joiners
  - Total Churn
  - Churn Rate

### Prediction Dashboard
- **Key Insights:**
  - Count of Predicted Churners
  - Churn Factors Breakdown

---

## 📌 Conclusion
This project integrates **SQL**, **Python**, **Power BI** and **Machine Learning** to deliver a comprehensive solution for churn analysis and prediction. It offers actionable insights and a visually appealing dashboard for business decision-makers.

---

## 📂 Repository Structure
```
📁 Project
├── churn_modeling.ipynb
├── customer_churn_dashboard.pbix
├── data.xlsx
├── predictions.csv
├── rf_model.pkl
└── 📜 README.md
```

---

## 📧 Contact
- **Author:** Viraj Suthar
- **Email:** virajsuthar2003@gmail.com
- **LinkedIn:** [Viraj Suthar](https://www.linkedin.com/in/viraj-suthar-517445333/)

---

> **Note:** This project is continually evolving, and enhancements will be added to improve functionality and insights. Stay tuned!

