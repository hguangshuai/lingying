# Neural Network Regularization and Dropout

## English Description / Topic
Detailed mechanics of **Dropout** and other common regularization techniques in Deep Learning.

## Fundamental Knowledge

### 1. Dropout Details
- **Mechanism**: During training, at each iteration, each neuron (except for the output layer) has a probability $p$ of being temporarily "dropped out" (set to zero).
- **Training Phase**: The network is trained on a "thinned" version of itself. This prevents **co-adaptation**, where neurons rely too heavily on the presence of specific other neurons.
- **Inference/Test Phase**: All neurons are active. However, the weights must be scaled down by $1-p$ (or the training weights scaled up by $1/(1-p)$—a technique called **Inverted Dropout**) to ensure the expected output remains consistent with what was seen during training.

### 2. Other NN Regularization Techniques
- **L2 Regularization (Weight Decay)**: Penalizes large weights by adding $\frac{\lambda}{2} \sum w^2$ to the loss. This keeps the weights small and the model simpler.
- **Early Stopping**: Monitoring validation loss and stopping training when it starts to increase.
- **Batch Normalization**: Although primarily used for faster convergence, it has a slight regularizing effect because it adds some noise (mean/variance of the batch) to the activations.
- **Data Augmentation**: Artificially increasing the size of the training set by applying transformations (rotation, noise) to the input.

## Spoken / Interview Answer
"**Dropout** is one of the most effective ways to prevent overfitting in deep neural networks. During training, we randomly 'turn off' a percentage of neurons. This forces the network to learn redundant representations because it can't rely on any single neuron to do all the work. It's like training a sports team by making them play with random players missing—eventually, everyone becomes more versatile. 
During **inference**, we use all the neurons, but we scale the weights so the total signal strength matches what the model experienced during training. In modern frameworks like PyTorch or TensorFlow, we use **Inverted Dropout**, which does the scaling during the training phase instead."

