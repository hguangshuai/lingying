# Handling Imbalanced Data

## English Description / Topic
Strategies for training machine learning models when one class significantly outnumbers the other(s).

## Fundamental Knowledge

### 1. Why is it a Problem?
- Standard algorithms aim to maximize overall accuracy, which can lead the model to simply predict the majority class every time (e.g., in fraud detection where 99.9% is non-fraud).

### 2. Data-Level Techniques (Resampling)
- **Oversampling**: Increase the number of minority class samples.
    - **Random Oversampling**: Duplicate minority samples.
    - **SMOTE (Synthetic Minority Over-sampling Technique)**: Create synthetic samples by interpolating between existing minority samples.
- **Undersampling**: Decrease the number of majority class samples.
    - **Random Undersampling**: Randomly remove majority samples.
    - **Tomek Links**: Remove samples that are very close to the class boundary to make it cleaner.

### 3. Algorithm-Level Techniques
- **Cost-Sensitive Learning**: Assign a higher penalty (weight) to misclassifying the minority class in the loss function.
- **Tree-Based Models**: Algorithms like Random Forest or XGBoost often handle imbalance better than linear models.
- **Anomaly Detection**: Treat the minority class as outliers (e.g., Isolation Forest, One-class SVM).

### 4. Evaluation Metrics (Crucial!)
- Never use **Accuracy**. Use **Precision**, **Recall**, **F1-Score**, and **ROC-AUC** or **PR-AUC**.

## Spoken / Interview Answer
"In an imbalanced dataset, the model tends to be biased toward the majority class. To handle this, I first look at the **Metrics**—I stop using Accuracy and focus on **Precision-Recall** or **F1-score**. 
At the **Data level**, I might use **SMOTE** to generate synthetic data for the minority class or **undersample** the majority class if I have enough data. 
At the **Model level**, I often use **class weights** (e.g., `class_weight='balanced'` in scikit-learn) to tell the loss function to care more about the minority class. Finally, for extremely imbalanced cases like fraud detection, I might even treat it as an **Anomaly Detection** problem."

