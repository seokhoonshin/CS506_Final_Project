# 🚗 Uber & Lyft Ride Price Prediction - Midterm Report  

## 📌 Project Description  
Ride-sharing services like **Uber and Lyft** have become integral to urban transportation. This project aims to analyze ride-sharing data from **Boston, MA** to identify key factors influencing ride prices and demand.  

Using a dataset containing ride details such as **time, location, temperature, and service type**, we will develop a **predictive model** that estimates ride fares based on these features. Additionally, this project seeks to uncover insights into pricing strategies and demand patterns, providing a deeper understanding of how external variables impact ride-sharing services.  

---

## 🎯 Goals  

### 🔹 Primary Goal  
- Successfully **predict ride prices** for Uber and Lyft in Boston based on **time of day, location, distance, temperature, and service type**.  

### 🔹 Secondary Goals  
- Identify patterns in **pricing strategies and demand fluctuations**.  
- Provide valuable insights for **users and ride-sharing companies** to optimize pricing and travel decisions.  

---

## 📊 Data Collection  

### 📌 Dataset Source  
- **Uber and Lyft Dataset - Boston, MA** ([Kaggle](https://www.kaggle.com/datasets/brllrb/uber-and-lyft-dataset-boston-ma))  

### 📌 Dataset Overview  
This dataset contains **six hundred thousand rows** of ride-sharing data collected from Uber and Lyft in **Boston, MA**, with **57 features** capturing key aspects of ride pricing, demand, and external influences:  

- **📅 Temporal Patterns:** `timestamp`, `datetime`, `timezone` (effect of time on ride demand and pricing)  
- **📍 Location-Based Demand:** `source`, `destination` (popular pickup/drop-off locations)  
- **💰 Pricing & Surge Effects:** `price`, `distance`, `surge_multiplier` (fare variations and surge pricing)  
- **🚖 Ride Type Comparison:** `cab_type` (Uber vs. Lyft fare and demand comparison)  
- **🌦 Weather Influence:** `windSpeed`, `temperature` (impact of weather on pricing and ride frequency)  
- **📌 Additional Attributes:** Various ride-specific details.  

---

## 🔎 EDA Process

### 📌 Exploration Data Analysis
🔹 We initially attempted to examine the dataset for missing values, data types, and basic statistical summaries.

<img width="258" alt="Missing value" src="https://github.com/user-attachments/assets/9e534eb8-b69a-446d-a182-a530bae1c81b" />

  - We found out that there were 55095 mssing values from the 'price' column.

🔹 We then moved on to examine the correlations between columns to identify and remove irrelevant, redundant, or highly correlated features by creating a heatmap.

<img width="686" alt="Heatmap" src="https://github.com/user-attachments/assets/7fa5df1d-a092-4fda-b66b-ee4891a3773a" />

  - We will keep features like month, temperature, visibility, and precipProbability, and remove other features that show strong multicollinearity (correlation > 0.75) with them based on the correlation analysis above.

  - We found out that there were 55095 mssing values from the 'price' column. We than moved on to examine the correlations between columns to identify and remove irrelevant, redundant, or highly correlated features by making an heatmap. We will additionally drop id and product_id because they are simply unique identifiers that do not contribute to modeling or prediction. The long_summary feature is removed as it is an unstructured text field that overlaps in meaning with short_summary and is not necessary for numerical analysis. The icon feature is also redundant since it duplicates weather summary information. We remove timezone because it remains constant throughout the dataset and does not provide predictive value. The datetime feature is dropped as we have already extracted hour and month features from it, making it redundant. Finally, ozone is dropped as it has little direct influence on rideshare demand or pricing and may introduce unnecessary noise into the model.

*All of these drops will be performed during the data preprocessing stage.

---

### 📌 Visualization Analysis
🔹 We then moved our next step to viaulize more graphs showing the relationship between various features for more anaylsis or understanding of the background of the given data.

✅ **Pre-Visualization**: Pre-Visualiztions for understanding landscape.
<img width="675" alt="ride count by hour" src="https://github.com/user-attachments/assets/ac922477-0eab-4cb3-8866-d24429f7c678" />

  - Ride Count by Hour: The number of rides remains fairly consistent throughout the day, with slightly lower counts during early morning hours (around 4-7 AM), and peaks around midnight and midday.

<img width="679" alt="ride count by destination location" src="https://github.com/user-attachments/assets/6ff1d8b7-54a1-4fa5-804b-64966d353df7" />

  - Financial District has the highest number of rides, indicating it’s the most popular drop-off area. Other destinations, such as Back Bay, Theatre District, and Boston University, also show consistently high and similar ride counts. This suggests that major business and university hubs are frequent destinations.

<img width="581" alt="ride count by cab type" src="https://github.com/user-attachments/assets/456d4f5c-7346-4031-ae7d-e3ee3a9482e5" />

  - There are more Uber rides compared to Lyft overall.

✅ **Boxplot/Historgram**: Compare Uber and Lyft pricing structures.  

<img width="684" alt="price distribution by cab type" src="https://github.com/user-attachments/assets/5853a910-1aba-44c5-beea-198c20bb69cb" /> <img width="684" alt="price distribution by cab type2" src="https://github.com/user-attachments/assets/405dbb49-c128-4418-b260-c752bec4148c" />

  - The median and maximum price for Lyft are both higher than those of Uber. Looking at the distribution, Uber is used more frequently at lower price ranges, indicating that users tend to choose Uber for cheaper rides. However, Uber also has more outliers on the higher end of the price spectrum. We think that this is likely due to the availability of premium or luxury vehicle options in Uber’s service offerings.

✅ **Weather Impact Analysis**: Display bar graphs between weather and ride counts to examine relationship them.

<img width="462" alt="ride count by weather" src="https://github.com/user-attachments/assets/6a8babb5-0f13-4424-afd1-69e226cbacd2" /> <img width="462" alt="ride count by weather2" src="https://github.com/user-attachments/assets/6efd29ea-2ec4-4f68-9a65-9384384ad444" />

   - Precipitation Probability vs Ride Count The majority of rides occur when precipitation probability is low (0-10%). As precipitation probability increases, ride count significantly drops. A small spike is observed at 90-100% precipitation probability, possibly due to increased ride demand during heavy rain.
   - Humidity vs Ride Count Ride count generally increases with humidity, peaking around 72-78% humidity. A sharp decline follows after 78% humidity, possibly indicating a threshold where extreme humidity reduces ride demand.
   - Wind Speed vs Ride Count Ride count is highest when wind speed is around 3-9 mph. Rides decline as wind speed increases beyond 12 mph, likely due to worsening travel conditions.
   - Visibility vs Ride Count The majority of rides occur when visibility is high (9-10 miles). Low visibility (under 3 miles) significantly reduces ride count, suggesting poor driving conditions discourage ride requests.

---

## 📌 Data Preprocessing
  🔹 As mentioned in the EDA process, we will drop unnecessary and redundant features, remove missing values, exclude outliers, and encode Categorical features before modeling.
  - Drop unnecessary, redundant features:
      - We will additionally drop the *_bin columns, as they were only used for exploratory visualizations and are not suitable for modeling. These interval-based features could cause errors and do not provide additional predictive power, so they were removed.
  
  - Exclude Outliers:
      - The process of removing the outliers were done using the Interquartile Range (IQR) Method.
      - We calculated the first quartile (Q1, 25th percentile) and third quartile (Q3, 75th percentile) to determine the middle 50% of the data. The IQR measures data spread, and idetifies values below Q1 - 1.5 * IQR or above Q3 + 1.5 * IQR as outliers. We then then filtered to keep only values within this range from the dataset.
    

  -  Encoding Categorical features:
      -  We created two versions of the dataset for different modeling needs. The Label Encoded version (df_clean_label) is used for tree-based models like Random Forest and XGBoost, which can handle integer-labeled categories effectively. The One-Hot Encoded version (df_clean_encoded) is used for models such as Neural Networks, Linear Regression, and KNN, which require categorical variables to be represented as independent binary features to avoid implying any ordinal relationship.

     - <img width="291" alt="encoding" src="https://github.com/user-attachments/assets/7e06a312-6c1f-43eb-a0bc-6b43340e93b9" /> <img width="291" alt="encoding2" src="https://github.com/user-attachments/assets/ffa44e55-ab63-4433-a8ef-adf9bf81455b" />

---

## ⚙️ Machine Learning Models  

  🔹 We tested multiple models to predict ride fares:
  - ✅ Random Forest:

    - <img width="365" alt="rfg" src="https://github.com/user-attachments/assets/3ffa300f-27da-4978-975b-009eb053d986" /> <img width="193" alt="RF" src="https://github.com/user-attachments/assets/08e7de1f-3c48-4cdb-94e6-15e55ebad5e7" />
    
  - ✅ XGBoost:

    - <img width="365" alt="xgbg" src="https://github.com/user-attachments/assets/d6724cfe-8a7a-408a-9f3e-f9f4aacd49c1" /> <img width="223" alt="xgb" src="https://github.com/user-attachments/assets/2e9d9c6d-3962-45cf-8426-8787145f61a6" />

  - ✅ Linear Regression:

    - <img width="365" alt="lrg" src="https://github.com/user-attachments/assets/d370f6a9-b3ff-43cb-8367-ca3bebd3cd5f" /> <img width="223" alt="LR" src="https://github.com/user-attachments/assets/eca5546d-6916-40eb-b9ff-d160f0bf77f7" />


  - ✅ Neural Network:

    - <img width="367" alt="NNg" src="https://github.com/user-attachments/assets/146c22df-66d7-4020-a338-b5fa9b53449e" /> <img width="196" alt="NN" src="https://github.com/user-attachments/assets/d8524371-e0a1-4106-8af4-b1eccca0a0d3" />

  - ✅ KNN:

    - <img width="367" alt="knng" src="https://github.com/user-attachments/assets/b25a94c4-6d4d-4119-a8b5-1cc45cba2aba" /> <img width="196" alt="KNN" src="https://github.com/user-attachments/assets/bd20078a-7a16-4c2e-9be9-2445b1c381f6" />


### 🚀 **Best Model: XGBoost**  
- XGBoost performed the best with the highest Test R² (0.959) and the lowest Test MAE (1.25), meaning it has the best predictive power and the lowest error in price estimation. 

---

## ✅ Next Steps  

🔹 **Fine-tune XGBoost parameters** for better accuracy.  
🔹 **Optimize model for real-time price prediction**.  

🚀 **Final goal:** Develop a highly accurate, generalizable model for Uber & Lyft ride price estimation.  

---

## 📢 Contributors  
🚀 **Joshua Nahm** - [GitHub Profile](https://github.com/JoshuaNahm)  
🚀 **Seokhoon Shin** - [GitHub Profile](https://github.com/seokhoonshin)  
