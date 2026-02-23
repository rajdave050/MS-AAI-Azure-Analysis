# Azure Functions Performance Analysis  
### End-to-End Statistical Modeling of Serverless Load–Latency Behavior

---

## 📌 Project Overview

This project presents a complete end-to-end statistical analysis of production-level serverless execution data from the Microsoft Azure Functions Trace 2019 dataset.

The primary objective is to evaluate whether invocation load (traffic volume) statistically explains variability in execution latency and to assess how accurately latency can be predicted using load-based statistical models.

The analysis includes:

- Data Cleaning and Structural Validation  
- Exploratory Data Analysis (EDA)  
- Regression Modeling  
- Random Forest Modeling  
- Model Evaluation and Statistical Interpretation  
- Identification and mitigation of feature leakage  
- Business insights and operational implications  

---

## 📊 Research Question

> To what extent does invocation load (Count) statistically explain variability in execution latency (Average Duration), and how accurately can latency be predicted using load-based statistical models?

---

## 🗂 Dataset Information

- **Source:** Microsoft Azure Functions Trace 2019  
- **Subset Used:** `function_durations_percentiles`  
- **Observations:** ~100,000 production execution records  
- **Variables of Interest:**  
  - Count (Invocation Load)  
  - Average (Execution Duration in milliseconds)  
  - Minimum / Maximum  
  - Percentile metrics  
  - Day indicator  

All analysis was conducted on cleaned data after removal of invalid negative latency values.

---

## 🧹 Data Preparation

Preprocessing steps included:

- Structural validation of dataset dimensions and variable types  
- Missing value audit (none detected)  
- Removal of invalid negative execution duration values  
- Retention of legitimate extreme outliers to preserve production realism  
- Log(1 + x) transformation applied due to extreme right-skewness  
- Train–test split (80/20 hold-out method)  

The preprocessing strategy preserved real-world workload variability while ensuring statistical validity.

---

## 📈 Exploratory Data Analysis (EDA)

Key findings:

- Heavy-tailed distribution in both invocation load and execution latency  
- Skewness (Count ≈ 106, Average ≈ 9) indicating extreme right asymmetry  
- Near-zero Pearson correlation between load and latency (≈ -0.005)  
- Evidence of heteroscedasticity at higher load levels  
- Log transformation significantly improved interpretability and variance stability  

These findings motivated the modeling approach adopted in the next stage.

---

## 🤖 Modeling Approach

Two primary models were evaluated:

### 1️⃣ Linear Regression (Baseline Model)
- Log–Log specification  
- RMSE ≈ 2.42  
- R² ≈ 0.053  

### 2️⃣ Random Forest Regressor (Challenger Model)
- 50 decision trees  
- Maximum depth = 10  
- RMSE ≈ 2.36  
- R² ≈ 0.098  

An exploratory multivariate regression initially produced an inflated R² (> 0.93). However, this was identified as **feature leakage**, as it included latency-derived predictors (Minimum and Maximum). That model was intentionally discarded to preserve predictive integrity and real-world applicability.

Final models strictly used invocation load as the explanatory predictor.

---

## 📉 Key Results

- Invocation load explains less than 10% of latency variability.  
- Random Forest slightly outperforms linear regression but still demonstrates limited explanatory power.  
- Azure’s serverless architecture appears to effectively decouple traffic scale from execution latency.  
- Latency variability is likely influenced by infrastructure-level factors such as cold starts, resource allocation, and runtime characteristics.  

---

## 🏢 Business Implications

- Monitoring traffic volume alone is insufficient for predicting performance degradation.  
- Infrastructure-level metrics are necessary for reliable forecasting.  
- Azure auto-scaling mechanisms appear effective at preventing proportional latency increases during traffic spikes.  

---

## 🔬 Future Work

Potential extensions for improved predictive modeling include:

- Time-since-last-invocation (Cold Start modeling)  
- Memory allocation and runtime environment variables  
- Plan tier differentiation (Consumption vs Premium)  
- Concurrency and scaling metrics  

---

## 👥 Team Contributions

- **Moinuddin K** — Led exploratory data analysis (EDA), implemented regression and Random Forest models, performed statistical evaluation (RMSE, R²), identified and addressed feature leakage, and interpreted modeling results.

- **Kunal** — Contributed to dataset selection, formulation of research objectives, development of the introduction and business context, and review of preprocessing methodology.

- **Raj Dave** — Conducted data cleaning and structural validation, generated visualizations (histograms, boxplots, scatterplots), supported model validation, and prepared presentation materials.

---

## 💻 How to Run the Project

## 💻 How to Run the Project

1. Clone the repository:
   ```
   git clone https://github.com/moinuddinghouse/MS-AAI-Azure-Analysis.git
   ```

2. Navigate into the project folder:
   ```
   cd MS-AAI-Azure-Analysis
   ```

3. Install required libraries:
   ```
   pip install -r requirements.txt
   ```

4. Launch Jupyter Notebook:
   ```
   jupyter notebook Final-Project-Report-Team-2.ipynb
   ```

5. Run all cells sequentially to reproduce the analysis.


---

## 🤖 AI Assistance Disclosure

Generative AI tools (ChatGPT) were used to assist with language refinement, structural organization, and clarification of statistical explanations. All data preprocessing, modeling decisions, implementation, and interpretation were independently executed and validated by the project team.

---

## 📚 Academic Use Notice

This project was developed for academic purposes within the AAI program.
