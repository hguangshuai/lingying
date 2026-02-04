# Naive Bayes and Transfer Learning

## English Description / Topic
Overview of the **Naive Bayes** classifier and the concept of **Transfer Learning**.

## Fundamental Knowledge

### 1. Naive Bayes
- **Definition**: A probabilistic classifier based on **Bayes' Theorem**.
  $$P(A|B) = \frac{P(B|A)P(A)}{P(B)}$$
- **Why "Naive"?**: It makes the strong (and often unrealistic) assumption that all features are **independent** of each other given the class label.
- **Key Assumption**: **Conditional Independence** of features.
- **Pros**: Fast, works well with small data, and is a strong baseline for text classification (Spam detection).

### 2. Transfer Learning
- **Definition**: A technique where a model developed for one task is reused as the starting point for a model on a second, related task.
- **Process**:
    1. Take a pre-trained model (e.g., ResNet trained on ImageNet).
    2. **Freeze** the early layers (which learn general features like edges).
    3. **Fine-tune** the last few layers (or replace them) on your specific, smaller dataset.
- **Pros**: Requires much less data and less training time than training from scratch.

## Spoken / Interview Answer
"**Naive Bayes** is a simple probabilistic classifier. It's called 'Naive' because it assumes that every feature is completely independent of the others, which is rarely true in real life, but the model still performs surprisingly well on tasks like spam filtering. 
**Transfer Learning** is the practice of taking a model that has already been trained on a massive dataset—like Google's BERT for text or ImageNet for photos—and repurposing it for your own specific problem. It's extremely powerful because it allows you to get state-of-the-art results even if you only have a small amount of data yourself."

