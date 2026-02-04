# Convolutional Neural Networks (CNN) vs. MLP

## English Description / Topic
Basic concepts of **CNNs** and how they differ from traditional **Multi-Layer Perceptrons (MLP)**.

## Fundamental Knowledge

### 1. What is a CNN?
- A type of Deep Neural Network designed for processing grid-like data (e.g., images).
- **Core Layers**:
    - **Convolutional Layer**: Uses filters (kernels) to extract local features (spatial hierarchy).
    - **Pooling Layer**: Reduces spatial dimensions (downsampling) to reduce computation and prevent overfitting.
    - **Fully Connected (FC) Layer**: Standard MLP layers at the end for classification.

### 2. CNN vs. MLP: Key Differences
- **Local Connectivity**:
    - **MLP**: Each neuron in one layer is connected to every neuron in the next (Fully Connected).
    - **CNN**: Each neuron is only connected to a small local region of the input (Receptive Field).
- **Weight Sharing**:
    - **MLP**: Every connection has its own weight.
    - **CNN**: The same filter (weight) is applied across the entire image. This significantly reduces the total number of parameters.
- **Invariance**:
    - CNNs are better at handling **Translation Invariance** (recognizing a cat regardless of where it is in the image) due to the convolution and pooling operations.

### 3. Advantages of CNN
- **Parametric Efficiency**: Because of weight sharing, CNNs have much fewer parameters than MLPs for the same input size, making them easier to train.
- **Hierarchical Feature Extraction**: Lower layers learn edges, middle layers learn shapes, and higher layers learn complex objects.

## Spoken / Interview Answer

### "How is a CNN different from a standard MLP?"
"The main difference lies in how they handle spatial information. In an **MLP**, we flatten the input into a 1D vector, which loses all spatial relationships. In a **CNN**, we preserve the 2D/3D structure of the image. 
CNNs use **Weight Sharing** and **Local Connectivity**, meaning the same filter is applied across different parts of the image. This makes CNNs much more efficient (fewer parameters) and gives them **Translation Invariance**, which is crucial for computer vision."

### "Why not just use an MLP for image classification?"
"If we use an MLP for a 1000x1000 pixel image, the first hidden layer with 1000 neurons would have 1 billion weights. This is computationally impossible and prone to massive overfitting. A CNN would only need a few thousand parameters for the same task because the same filters are reused across the image."

