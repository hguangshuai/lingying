# Batch Normalization vs. Layer Normalization

## English Description / Topic
Detailed comparison between **Batch Normalization (BN)** and **Layer Normalization (LN)**, focusing on their mathematical differences, application scenarios, and behavior during training vs. inference.

## Fundamental Knowledge

### 1. Calculation Dimensions
- **Batch Normalization (BN)**: Normalizes across the **batch** dimension. For a specific feature, it calculates the mean and variance using all samples in the current mini-batch.
    - Input Shape: $(N, C, H, W)$
    - Normalizes over: $(N, H, W)$ for each channel $C$.
- **Layer Normalization (LN)**: Normalizes across the **feature (layer)** dimension. For a single sample, it calculates the mean and variance using all features in that sample.
    - Input Shape: $(N, C, H, W)$ or $(N, L, D)$
    - Normalizes over: $(C, H, W)$ for each sample $N$.

### 2. Comparison Table
| Feature | Batch Normalization (BN) | Layer Normalization (LN) |
| :--- | :--- | :--- |
| **Normalization Axis** | Across the mini-batch (N). | Across the features (C/D) of one sample. |
| **Batch Size Dependency** | High. Fails if batch size is 1 or too small. | None. Works same for any batch size. |
| **Training vs. Inference** | Different. Uses batch stats in training, running stats in inference. | Same. Uses the same calculation for both. |
| **Typical Use Case** | CNNs (Computer Vision). | RNNs, Transformers (NLP). |
| **Key Advantage** | Accelerates training, provides slight regularization. | Stable for variable-length sequences. |

### 3. Why BN for CNNs and LN for NLP?
- **CNNs (BN)**: In images, different channels represent different filters. We want to normalize the same filter across different images to ensure the features have a consistent distribution.
- **NLP (LN)**: In sequences, the length can vary. BN would require calculating statistics across different time steps and different sequences, which is unstable. LN normalizes each word embedding (or the whole hidden state) within a single sample, making it independent of other samples and sequence length.

## Spoken / Interview Answer
"The fundamental difference between Batch and Layer Normalization is the **axis** they normalize across. 
**Batch Norm** calculates the mean and variance across the entire mini-batch for each feature. It works great for **CNNs**, but it's sensitive to batch size and requires keeping track of running statistics for inference. 
**Layer Norm**, on the other hand, calculates the mean and variance across all features for each individual sample. This makes it independent of the batch size and ideal for **NLP models like Transformers and RNNs**, where sequence lengths vary and we often process samples independently. 
In short: BN looks at the same feature across many samples; LN looks at all features within one sample."

