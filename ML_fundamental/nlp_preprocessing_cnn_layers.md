# NLP Preprocessing and Simple CNN Architecture

## English Description / Topic
Essential steps for **NLP preprocessing** before embeddings and the functional breakdown of a **simple CNN architecture**.

## Fundamental Knowledge

### 1. NLP Preprocessing (Before Embeddings)
Before feeding text into an embedding layer (like Word2Vec or BERT), we typically perform:
- **Tokenization**: Splitting sentences into words or sub-words.
- **Lowercasing**: Converting all text to lowercase to reduce vocabulary size.
- **Removing Punctuation & Special Characters**.
- **Stopword Removal**: Removing common words (the, is, at) that don't add much meaning (optional depending on task).
- **Stemming / Lemmatization**: Reducing words to their root form (e.g., "running" -> "run").
- **Handling Out-of-Vocabulary (OOV)**: Replacing rare words with an `<UNK>` token.

### 2. Simple CNN Architecture Breakdown
**Architecture**: [Conv Layer] -> [Max Pooling] -> [Fully Connected]

- **Convolutional Layer**: 
    - **Function**: Feature Extraction. 
    - It uses filters to detect local patterns (like edges in images or n-grams in text).
- **Max Pooling Layer**: 
    - **Function**: Dimensionality Reduction & Invariance. 
    - It takes the maximum value in a window, reducing the spatial size while keeping the most important features. It makes the model more robust to small shifts.
- **Fully Connected (FC) Layer**: 
    - **Function**: Classification. 
    - It takes the high-level features extracted by previous layers and maps them to the final class labels (e.g., Cat vs. Dog).

**What is this NN for?**
This is a classic **Image Classifier** (or a simple **Text Classifier** if 1D convolutions are used). It's designed to recognize patterns and categorize inputs into discrete classes.

## Spoken / Interview Answer
"For **NLP preprocessing**, the goal is to clean and standardize the text. I usually start with tokenization and lowercasing. Depending on the task, I might remove stopwords or apply lemmatization to reduce the vocabulary size before mapping words to their embedding vectors.
As for the **NN architecture**: the **Convolutional layer** acts as the 'eyes'—it looks for specific patterns. The **Max Pooling layer** simplifies the information, keeping only the strongest signals. Finally, the **Fully Connected layer** acts as the 'brain' that looks at all these patterns to make the final decision on what the input is."


