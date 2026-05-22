# Project Report Outline: Student Performance Prediction System

**Course Project Report**  
**Project Title:** Student Performance Prediction System using Machine Learning  
**Author:** Student / Researcher Name  
**Technology:** Python, Scikit-Learn, Pandas, Seaborn, Streamlit  

---

## 📄 Executive Summary (Abstract)
The Student Performance Prediction System is a data-driven web tool designed to evaluate student demographics and initial exam scores, classify overall performance, and provide personalized, actionable study recommendations. Using the Kaggle `StudentsPerformance.csv` dataset, we trained a **RandomForestClassifier** to classify students into **High**, **Average**, or **Low** performance tiers based on demographic details (Gender, Ethnicity, Lunch type, Parental Education, Test Prep status) and score distributions. The final machine learning model achieved an accuracy of **97.00%**, which was successfully integrated into an interactive web dashboard using **Streamlit**.

---

## 1. Introduction
* **Objective:** Bridge the gap between student raw scores and meaningful scholastic interventions.
* **Context:** Educational institutions generate large amounts of raw student score data. However, analyzing and acting on this data to prevent student failure is often complex.
* **Proposed Solution:** A lightweight, end-to-end Python pipeline that pre-processes academic scores, computes performance classes, trains a Random Forest model, and embeds it in an easy-to-use GUI for parents and teachers.

---

## 2. System Architecture & Work Flow
The system operates as a classic machine learning pipeline divided into two halves:

```text
[ Raw CSV Dataset ]
        │
        ▼ (main.py)
[ Data Preprocessing ] ────► Check for Missing & Duplicate Values
        │
        ▼
[ Feature Engineering ] ───► Compute average_score & categorise performance (High, Average, Low)
        │
        ├──────────────────► Save Visual EDA Charts to /screenshots
        ▼
[ Label Encoding ] ────────► Convert categories to numeric codes (LabelEncoder)
        │
        ▼
[ Train/Test Split ] ──────► Split 80/20 train/test sets (random_state=42)
        │
        ▼
[ Model Training ] ────────► Fit RandomForestClassifier (accuracy: 97.00%)
        │
        ▼
[ Pickle Saving ] ─────────► Export model.pkl and label_encoders.pkl to /models
        │
        ▼ (app.py)
[ Streamlit Web App ] ─────► User input dropdowns + score sliders
        │
        ▼
[ Model Inference ] ───────► Load model, encode input, predict performance
        │
        ▼
[ Feedback Generator ] ────► Display predicted performance + print study recommendations
```

---

## 3. Dataset Description
The dataset used is `StudentsPerformance.csv` consisting of **1,000 students** (rows) and **8 features** (columns):
1. **Gender:** Categorical (`female`, `male`).
2. **Race/Ethnicity:** Categorical (`group A`, `group B`, `group C`, `group D`, `group E`).
3. **Parental Level of Education:** Categorical (`high school`, `some high school`, `some college`, `associate's degree`, `bachelor's degree`, `master's degree`).
4. **Lunch Type:** Categorical (`standard`, `free/reduced` - indicates socioeconomic status).
5. **Test Preparation Course:** Categorical (`none`, `completed`).
6. **Math Score:** Numeric integer ($0$ to $100$).
7. **Reading Score:** Numeric integer ($0$ to $100$).
8. **Writing Score:** Numeric integer ($0$ to $100$).

---

## 4. Data Preprocessing & Feature Engineering
* **Data Integrity Checks:** 
  * Total duplicates removed: **0**
  * Total missing/null values detected: **0**
* **Average Score Calculation:**
  $$\text{average\_score} = \frac{\text{math score} + \text{reading score} + \text{writing score}}{3}$$
* **Target Classification Rules:**
  * **High:** Average score $\ge 75$ (Excellent tier)
  * **Average:** Average score $\ge 50$ (Passing/Moderate tier)
  * **Low:** Average score $< 50$ (At-Risk tier)

---

## 5. Exploratory Data Analysis (EDA) Findings
*(Attach corresponding charts from the `screenshots/` directory)*

* **Performance Level Distribution:** 
  * The dataset is predominantly composed of "Average" performers (573 students), followed by "High" performers (324), and a minor group of "Low" performers (103).
* **Correlation Heatmap:** 
  * High mathematical correlation observed between `reading score` and `writing score` ($\approx 0.95$), indicating that strong reading skills heavily translate to strong writing skills. Math score correlates moderately ($\approx 0.80$) with both.
* **Gender vs Average Score:** 
  * Bar plot indicates female students slightly outperform male students in average overall scores across reading and writing.

---

## 6. Model Training & Evaluation
* **Algorithm:** RandomForestClassifier (An ensemble of 100 Decision Trees).
* **Configuration:** `n_estimators=100`, `random_state=42`.
* **Testing Split:** 80% Training Data, 20% Unseen Evaluation Data.
* **Metric Results:**
  * **Final Test Accuracy:** **`97.00%`**
  * **Macro F1-Score:** **`0.97`**
  * **Confusion Matrix Analysis:** 
    * The model correctly predicted **194 out of 200** test cases.
    * Only 6 students were misclassified (mostly marginal edge cases between Average and High performance tiers).

---

## 7. Streamlit Web Dashboard & Recommendation Engine
* **GUI Elements:** Built with dynamic `st.selectbox` fields for demographics and interactive `st.slider` fields for test scores.
* **Prediction Feedback:** 
  * **High** performance displays a green `st.success()` banner.
  * **Average** performance displays a yellow `st.warning()` banner.
  * **Low** performance displays a red `st.error()` banner.
* **Actionable Recommendation Engine:** 
  * Checks score boundaries ($< 50$) to inject specific tutoring directives.
  * If a student didn't complete prep courses, it triggers a prep course completion warning.
  * If the overall score is low, it calls for an increase in dedicated study calendar hours.

---

## 8. Conclusion & Future Scope
* **Conclusion:** The project demonstrates how basic demographic data combined with simple exam records can predict performance levels with high accuracy. This can help schools identify at-risk students before final term results are released.
* **Future Enhancements:**
  * Add a database connection (e.g. SQLite) to store student predictions over time.
  * Incorporate attendance records and extracurricular activity data.
  * Implement hyperparameter tuning (GridSearchCV) to maximize classifier performance on larger datasets.
