# ML Model Design Framework (Case Study)

## English Description / Topic
A structured framework for designing an ML model during a System Design or Model Design interview.

## Fundamental Knowledge

### 1. Problem Clarification & Goal Setting
- **Objective**: What is the business goal? (e.g., increase CTR, reduce churn).
- **Task Type**: Binary classification, multi-class classification, regression, or ranking?
- **Constraints**: Latency (ms), throughput, data volume.

### 2. Data & Features
- **Data Sources**: User profile, item metadata, context (time, device), historical interactions.
- **Feature Engineering**:
    - **Categorical**: One-hot, Embedding (for high cardinality).
    - **Numerical**: Scaling, Bucketing.
    - **Cross-features**: User-item interaction features.
- **Handling Missing Data & Outliers**.

### 3. Model Selection
- **Baseline**: Logistic Regression or Random Forest (simple, interpretable).
- **Advanced**: Gradient Boosted Decision Trees (XGBoost/LightGBM) for tabular data, or Deep Learning (Wide & Deep, DeepFM) for recommendation systems.

### 4. Evaluation
- **Offline**: AUC, F1-Score, Log-Loss, Precision-Recall.
- **Online**: A/B testing (CTR, conversion rate, revenue).
- **Validation Strategy**: Time-based split (crucial for time-series data like recommendations).

## Spoken / Interview Answer
"When designing an ML model, I follow a 4-step framework. 
First, I **clarify the goal**. For example, if we are building a Feed Ranking model, the goal is to maximize user engagement (clicks/likes). 
Second, I focus on **Data and Features**. I would categorize features into User (interests, demographics), Item (content type, popularity), and Context (time of day, location). 
Third, I **choose a model**. I usually start with a baseline like Logistic Regression to establish a performance floor, then move to more complex models like GBDT or a Deep Ranking model if we have sufficient data. 
Finally, I define the **Evaluation strategy**. I'd use offline metrics like ROC-AUC on a time-based validation set, and ultimately evaluate the success via A/B testing on online metrics like Daily Active Users or Click-Through Rate."

