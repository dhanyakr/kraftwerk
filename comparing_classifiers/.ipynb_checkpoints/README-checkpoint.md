### **Final Report: Comparing Classifiers: Bank Marketing Dataset Analysis and Modeling**

---

#### **Objective:**
To analyze customer data and predict whether a client will subscribe to a term deposit (`yes` or `no`) using machine learning models. The goal was to evaluate and optimize the performance of four models: K-Nearest Neighbors (KNN), Decision Trees, Logistic Regression, and Support Vector Machines (SVM).

---

### **1. Data Overview**
The dataset is related to the marketing of bank products over the telephone. The dataset comes from the [UCI Machine Learning Repository](https://archive.ics.uci.edu/ml/datasets/bank+marketing). The data is from a Portuguese banking institution and is a collection of the results of multiple marketing campaigns.

> [!NOTE]  
> The dataset includes both full and sampled data files. Due to processing power limitations, I used the sampled data file; however, I believe the code in the notebooks should also be capable of handling the full data file.

**Dataset Size:** 4,119 entries, 21 columns. 

**Features:**
- **Categorical** (e.g., `job`, `marital`, `education`, `month`, etc.)
- **Numerical** (e.g., `age`, `duration`, `campaign`, etc.)
- **Target Variable:** `y` (binary: `yes`, `no`).

**Imbalance in Target Variable:**
- `no`: 89.05%
- `yes`: 10.95%

---

### **2. Preprocessing**

- **Categorical Features**:
  - ['job', 'marital', 'education', 'default', 'housing', 'loan', 'contact', 'month', 'day_of_week', 'poutcome', 'y']
- **Numerical Features**: 
  - ['age', 'duration', 'campaign', 'pdays', 'previous', 'emp.var.rate', 'cons.price.idx', 'cons.conf.idx', 'euribor3m', 'nr.employed']


- **Train-Test Split**: 70% training, 30% testing.
---

### **3. Models and Metrics**

#### **Models Evaluated**:
1. K-Nearest Neighbors (KNN)
2. Decision Trees
3. Logistic Regression
4. Support Vector Machines (SVM)

#### **Evaluation Metrics**:
- **Accuracy:** Overall correctness.
- **Precision:** Relevance of predictions for each class.
- **Recall:** Sensitivity to true positives.
- **F1-Score:** Balance between precision and recall.
- **AUC-ROC:** Ability to distinguish between classes.

---

### **4. Results**

	Accuracy	Precision (no)	Precision (yes)	Recall (no)	Recall (yes)	F1-Score (no)	F1-Score (yes)	AUC-ROC
Model								
KNN({'n_neighbors': 9, 'weights': 'distance'})	0.901294	0.912384	0.632653	0.983651	0.229630	0.946678	0.336957	0.831480
Decision Tree({'max_depth': 10, 'min_samples_split': 10})	0.900485	0.933511	0.555556	0.956403	0.444444	0.944818	0.493827	0.812053
Logistic Regression({'C': 1, 'solver': 'liblinear'})	0.909385	0.927398	0.645570	0.974569	0.377778	0.950399	0.476636	0.923760
SVM({'C': 0.1, 'kernel': 'linear'})	0.907767	0.921435	0.661538	0.980018	0.318519	0.949824	0.430000	0.921849
KNN	0.891586	0.911489	0.508197	0.972752	0.229630	0.941125	0.316327	0.805540
Decision Tree	0.884304	0.939450	0.472603	0.930064	0.511111	0.934733	0.491103	0.720587
Logistic Regression	0.909385	0.927398	0.645570	0.974569	0.377778	0.950399	0.476636	0.924096
SVM	0.906149	0.916314	0.679245	0.984559	0.266667	0.949212	0.382979	0.907431


#### Confusion Matrices

  

---

### **5. Key Insights**

1. **Logistic Regression (Optimized)**:
   - Best overall model with an AUC-ROC of **0.924** and high accuracy (90.9%).
   - Balances recall for both `yes` and `no`.

2. **Decision Tree (Optimized)**:
   - High recall for `yes` (44.4%), making it suitable for capturing minority class instances.

3. **SVM (Optimized)**:
   - High precision for `yes` (66.2%), indicating fewer false positives.

4. **KNN (Optimized)**:
   - Moderate performance but struggles with recall for `yes` (23%).

---

### **6. Next Steps**

1. **Business Application**:
   - Use **Logistic Regression** as the primary model for predicting term deposit subscriptions.
   - Deploy **Decision Tree** in scenarios where recall for `yes` (minority class) is critical, such as in targeted marketing campaigns.

2. **Class Imbalance Mitigation**:
   - Implement techniques like **SMOTE (Synthetic Minority Oversampling Technique)** to improve recall for `yes` and ensure better class balance.

3. **Feature Engineering**:
   - Explore additional features or create interactions between existing ones to enhance model performance.
   
4. **Model Monitoring**:
   - Continuously monitor the model’s performance in production.
   - Use recent data for retraining the model periodically to account for changing customer behaviors.

5. **Ensemble Models**:
   - Experiment with ensemble techniques like **Random Forests** or **Gradient Boosting Machines** to improve prediction accuracy and recall further.

---
### Link to the Notebook
* [Notebook](./performance_comparison_classifiers.ipynb)
---
