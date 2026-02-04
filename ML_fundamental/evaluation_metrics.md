# Evaluation Metrics: Precision, Recall, and ROC-AUC

## English Description / Topic
Detailed breakdown of classification metrics, how they are calculated, and when to use which.

## Fundamental Knowledge

### 1. Confusion Matrix
- **TP**: True Positive, **TN**: True Negative
- **FP**: False Positive (Type I Error)
- **FN**: False Negative (Type II Error)

### 2. Basic Metrics
- **Precision**: $\frac{TP}{TP + FP}$ (Of all predicted positives, how many were actually positive? Focus on "Quality").
- **Recall (Sensitivity)**: $\frac{TP}{TP + FN}$ (Of all actual positives, how many did we find? Focus on "Quantity/Coverage").
- **F1-Score**: Harmonic mean of Precision and Recall. $2 \cdot \frac{Precision \cdot Recall}{Precision + Recall}$.

### 3. ROC and AUC
- **ROC Curve (Receiver Operating Characteristic)**: A plot of **True Positive Rate (Recall)** vs. **False Positive Rate ($\frac{FP}{FP+TN}$)** at various threshold settings.
- **AUC (Area Under the Curve)**: Represents the probability that the model will rank a randomly chosen positive instance higher than a randomly chosen negative one.
    - $AUC = 0.5$: Random guessing.
    - $AUC = 1.0$: Perfect classifier.

### 4. Precision-Recall Curve (PR Curve)
- More useful than ROC when the classes are **highly imbalanced**. It doesn't use True Negatives, which can overwhelm the ROC curve in rare-event detection.

## Spoken / Interview Answer

### "What is the difference between Precision and Recall?"
"Precision answers 'Out of all instances we flagged as positive, how many were correct?', while Recall answers 'Out of all actual positive instances, how many did we successfully flag?'. There is usually a trade-off: as you try to catch more positives (higher Recall), you often catch more false alarms (lower Precision)."

### "When would you prefer Recall over Precision?"
"I prefer high **Recall** when the cost of a False Negative is very high, like in **cancer diagnosis** or **airport security**—you'd rather have a false alarm than miss a real case. I prefer high **Precision** when the cost of a False Positive is high, like in **spam filtering**—you don't want a legitimate email to end up in the spam folder."

### "How do you interpret AUC?"
"AUC measures the model's ability to **rank** items. An AUC of 0.8 means that if you pick one positive and one negative sample at random, there is an 80% chance the model will assign a higher probability to the positive one. It is threshold-independent, which makes it a great metric for comparing models overall."

