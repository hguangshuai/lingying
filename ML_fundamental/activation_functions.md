# 常用激活函数

## 问题：常用的激活函数有哪些？它们的公式、输出范围、好处和用途是什么？

### 一、详细中文解释

激活函数（Activation Function）是神经网络中的非线性变换（Nonlinear Transformation），引入非线性使得神经网络能够学习复杂的模式。激活函数将神经元的输入映射到输出，不同的激活函数有不同的特性，适用于不同的场景。

#### 1. 传统激活函数

##### 1.1 Sigmoid函数

**数学公式：**

$$\sigma(x) = \frac{1}{1 + e^{-x}} = \frac{e^x}{e^x + 1}$$

**符号解释：**
- $\sigma(x)$：Sigmoid函数（Sigmoid function），也叫逻辑函数（Logistic function）
- $x$：输入值（Input value），可以是任意实数
- $e $：自然常数（Natural constant），约等于 $ 2.71828$
- $e^{-x}$：$ e $ 的 $-x $ 次方（$ e $ raised to the power of $-x$）
- $\frac{1}{1 + e^{-x}}$：Sigmoid函数的定义式（Definition of Sigmoid function）

**输出范围：**

$$(0, 1)$$

**符号解释：**
- 输出在开区间 $(0, 1)$ 内（Output is in the open interval $(0, 1)$）
- 当 $x \to -\infty $ 时，$\sigma(x) \to 0 $（When $ x $ tends to negative infinity, $\sigma(x)$ tends to 0）
- 当 $x \to +\infty $ 时，$\sigma(x) \to 1 $（When $ x $ tends to positive infinity, $\sigma(x)$ tends to 1）
- 当 $x = 0 $ 时，$\sigma(0) = 0.5 $（When $ x = 0 $, $\sigma(0) = 0.5$）

**梯度（导数）：**

$$\sigma'(x) = \sigma(x)(1 - \sigma(x))$$

**符号解释：**
- $\sigma'(x)$：Sigmoid函数的导数（Derivative of Sigmoid function）
- 梯度在 $x = 0 $ 处最大，为 $ 0.25 $（Gradient is maximum at $ x = 0 $, equals $ 0.25$）
- 当 $|x|$ 很大时，梯度接近0，导致梯度消失（When $|x|$ is large, gradient approaches 0, causing gradient vanishing）

![Sigmoid Activation Function](../../images/activation_functions/sigmoid.png)

**好处和用途：**
- **输出范围**：$(0, 1)$，适合概率输出（Suitable for probability output）
- **平滑性**：处处可导，梯度连续（Smooth, differentiable everywhere, continuous gradient）
- **用途**：二分类问题的输出层（Output layer for binary classification），多分类问题的softmax输入（Input to softmax for multi-class classification）
- **缺点**：梯度消失问题（Gradient vanishing problem），计算较慢（Slower computation）

##### 1.2 Tanh函数（双曲正切）

**数学公式：**

$$\tanh(x) = \frac{e^x - e^{-x}}{e^x + e^{-x}} = \frac{\sinh(x)}{\cosh(x)} = 2\sigma(2x) - 1$$

**符号解释：**
- $\tanh(x)$：双曲正切函数（Hyperbolic tangent function）
- $\sinh(x) = \frac{e^x - e^{-x}}{2}$：双曲正弦函数（Hyperbolic sine function）
- $\cosh(x) = \frac{e^x + e^{-x}}{2}$：双曲余弦函数（Hyperbolic cosine function）
- $2\sigma(2x) - 1$：与Sigmoid的关系（Relationship with Sigmoid）

**输出范围：**

$$(-1, 1)$$

**符号解释：**
- 输出在开区间 $(-1, 1)$ 内（Output is in the open interval $(-1, 1)$）
- 当 $x \to -\infty $ 时，$\tanh(x) \to -1 $（When $ x $ tends to negative infinity, $\tanh(x)$ tends to $-1$）
- 当 $x \to +\infty $ 时，$\tanh(x) \to 1 $（When $ x $ tends to positive infinity, $\tanh(x)$ tends to $ 1$）
- 当 $x = 0 $ 时，$\tanh(0) = 0 $（When $ x = 0 $, $\tanh(0) = 0$），零中心化（Zero-centered）

**梯度（导数）：**

$$\tanh'(x) = 1 - \tanh^2(x)$$

**符号解释：**
- $\tanh'(x)$：Tanh函数的导数（Derivative of Tanh function）
- 梯度在 $x = 0 $ 处最大，为 $ 1 $（Gradient is maximum at $ x = 0 $, equals $ 1$）
- 梯度范围比Sigmoid更大（Gradient range is larger than Sigmoid）

![Tanh Activation Function](../../images/activation_functions/tanh.png)

**好处和用途：**
- **零中心化**：输出以0为中心，有助于训练（Zero-centered output helps training）
- **梯度更大**：相比Sigmoid，梯度更大，训练更快（Larger gradient than Sigmoid, faster training）
- **用途**：RNN的隐藏层（Hidden layers in RNN），需要零中心输出的场景（Scenarios requiring zero-centered output）
- **缺点**：仍有梯度消失问题（Still has gradient vanishing problem）

##### 1.3 ReLU函数（Rectified Linear Unit）

**数学公式：**

$$\text{ReLU}(x) = \max(0, x) = \begin{cases} x & \text{if } x > 0 \\ 0 & \text{if } x \leq 0 \end{cases}$$

**符号解释：**
- $\text{ReLU}(x)$：ReLU函数（Rectified Linear Unit）
- $\max(0, x)$：$ 0 $ 和 $ x $ 的最大值（Maximum of $ 0 $ and $ x$）
- 当 $x > 0 $ 时，输出 $ x $（When $ x > 0 $, output is $ x$）
- 当 $x \leq 0 $ 时，输出 $ 0 $（When $ x \leq 0 $, output is $ 0$）

**输出范围：**

$$[0, +\infty)$$

**符号解释：**
- 输出在区间 $[0, +\infty)$ 内（Output is in the interval $[0, +\infty)$）
- 最小值为 $0 $（Minimum value is $ 0$）
- 最大值无上界（Maximum value is unbounded）

**梯度（导数）：**

$$\text{ReLU}'(x) = \begin{cases} 1 & \text{if } x > 0 \\ 0 & \text{if } x \leq 0 \\ \text{undefined} & \text{if } x = 0 \end{cases}$$

**符号解释：**
- $\text{ReLU}'(x)$：ReLU函数的导数（Derivative of ReLU function）
- 当 $x > 0 $ 时，梯度为 $ 1 $（When $ x > 0 $, gradient is $ 1$）
- 当 $x \leq 0 $ 时，梯度为 $ 0 $（When $ x \leq 0 $, gradient is $ 0$），导致"死亡ReLU"问题（Causes "dying ReLU" problem）

![ReLU Activation Function](../../images/activation_functions/relu.png)

**好处和用途：**
- **计算简单**：计算速度快（Fast computation）
- **缓解梯度消失**：正区间梯度为1，缓解梯度消失（Gradient is 1 in positive region, alleviates gradient vanishing）
- **稀疏激活**：负值输出0，产生稀疏性（Negative values output 0, produces sparsity）
- **用途**：深度神经网络的隐藏层（Hidden layers in deep neural networks），CNN（Convolutional Neural Networks）
- **缺点**：死亡ReLU问题（Dying ReLU problem），负值无梯度（No gradient for negative values）

##### 1.4 Leaky ReLU

**数学公式：**

$$\text{LeakyReLU}(x) = \max(\alpha x, x) = \begin{cases} x & \text{if } x > 0 \\ \alpha x & \text{if } x \leq 0 \end{cases}$$

**符号解释：**
- $\text{LeakyReLU}(x)$：Leaky ReLU函数（Leaky Rectified Linear Unit）
- $\alpha $：泄漏系数（Leakage coefficient），通常取 $ 0.01 $（Typically $ 0.01$）
- 当 $x > 0 $ 时，输出 $ x $（When $ x > 0 $, output is $ x$）
- 当 $x \leq 0 $ 时，输出 $\alpha x $（When $ x \leq 0 $, output is $\alpha x$），允许小的负值（Allows small negative values）

**输出范围：**

$$(-\infty, +\infty)$$

**符号解释：**
- 输出可以是任意实数（Output can be any real number）
- 负值不会完全为0（Negative values are not completely zero）

**梯度（导数）：**

$$\text{LeakyReLU}'(x) = \begin{cases} 1 & \text{if } x > 0 \\ \alpha & \text{if } x \leq 0 \end{cases}$$

**符号解释：**
- 当 $x > 0 $ 时，梯度为 $ 1 $（When $ x > 0 $, gradient is $ 1$）
- 当 $x \leq 0 $ 时，梯度为 $\alpha $（When $ x \leq 0 $, gradient is $\alpha$），通常很小但不为0（Usually small but not zero）

![Leaky ReLU Activation Function](../../images/activation_functions/leaky_relu.png)

**好处和用途：**
- **解决死亡ReLU**：负值有小的梯度，避免神经元死亡（Negative values have small gradient, avoid neuron death）
- **保持稀疏性**：负值很小，仍保持一定稀疏性（Negative values are small, still maintains some sparsity）
- **用途**：替代ReLU，特别是在GAN中（Alternative to ReLU, especially in GANs）

##### 1.5 ELU（Exponential Linear Unit）

**数学公式：**

$$\text{ELU}(x) = \begin{cases} x & \text{if } x > 0 \\ \alpha(e^x - 1) & \text{if } x \leq 0 \end{cases}$$

**符号解释：**
- $\text{ELU}(x)$：ELU函数（Exponential Linear Unit）
- $\alpha $：超参数（Hyperparameter），通常取 $ 1 $（Typically $ 1$）
- 当 $x > 0 $ 时，输出 $ x $（When $ x > 0 $, output is $ x$）
- 当 $x \leq 0 $ 时，输出 $\alpha(e^x - 1)$（When $ x \leq 0 $, output is $\alpha(e^x - 1)$），平滑的负值（Smooth negative values）

**输出范围：**

$$(-\alpha, +\infty)$$

**符号解释：**
- 最小值为 $-\alpha $（Minimum value is $-\alpha$）
- 最大值无上界（Maximum value is unbounded）

**梯度（导数）：**

$$\text{ELU}'(x) = \begin{cases} 1 & \text{if } x > 0 \\ \text{ELU}(x) + \alpha & \text{if } x \leq 0 \end{cases}$$

**符号解释：**
- 当 $x > 0 $ 时，梯度为 $ 1 $（When $ x > 0 $, gradient is $ 1$）
- 当 $x \leq 0 $ 时，梯度为 $\text{ELU}(x) + \alpha $（When $ x \leq 0 $, gradient is $\text{ELU}(x) + \alpha$），平滑且非零（Smooth and non-zero）

![ELU Activation Function](../../images/activation_functions/elu.png)

**好处和用途：**
- **平滑性**：负值区域平滑，梯度连续（Smooth in negative region, continuous gradient）
- **零均值**：负值区域使得输出接近零均值（Negative region makes output close to zero mean）
- **用途**：需要平滑激活的场景（Scenarios requiring smooth activation）

##### 1.6 Softmax函数

**数学公式：**

$$\text{Softmax}(\mathbf{x})_i = \frac{e^{x_i}}{\sum_{j=1}^{K} e^{x_j}}$$

**符号解释：**
- $\text{Softmax}(\mathbf{x})_i $：Softmax函数对输入向量 $\mathbf{x}$ 的第 $ i $ 个元素的输出（Output of Softmax function for the $ i $-th element of input vector $\mathbf{x}$）
- $\mathbf{x} = [x_1, x_2, \ldots, x_K]^{\top}$：输入向量（Input vector），$ K $ 维向量（$ K$-dimensional vector）
- $e^{x_i}$：$ e $ 的 $ x_i $ 次方（$ e $ raised to the power of $ x_i$）
- $\sum_{j=1}^{K} e^{x_j}$：对所有 $ K $ 个元素的指数求和（Sum of exponentials of all $ K$ elements），归一化因子（Normalization factor）

**输出范围：**

$$(0, 1)$$

**符号解释：**
- 每个输出在开区间 $(0, 1)$ 内（Each output is in the open interval $(0, 1)$）
- 所有输出的和为1（Sum of all outputs equals 1）：$\sum_{i=1}^{K} \text{Softmax}(\mathbf{x})_i = 1$
- 输出可以解释为概率分布（Outputs can be interpreted as probability distribution）

**梯度（导数）：**

$$\frac{\partial \text{Softmax}(\mathbf{x})_i}{\partial x_j} = \text{Softmax}(\mathbf{x})_i (\delta_{ij} - \text{Softmax}(\mathbf{x})_j)$$

**符号解释：**
- $\frac{\partial \text{Softmax}(\mathbf{x})_i}{\partial x_j}$：Softmax输出的第 $ i $ 个元素对输入的第 $ j $ 个元素的偏导数（Partial derivative of the $ i $-th output element with respect to the $ j$-th input element）
- $\delta_{ij}$：Kronecker delta（克罗内克函数），当 $ i = j $ 时为 $ 1 $，否则为 $ 0 $（Kronecker delta, equals $ 1 $ when $ i = j $, otherwise $ 0$）
- 当 $i = j $ 时，梯度为 $\text{Softmax}(\mathbf{x})_i (1 - \text{Softmax}(\mathbf{x})_i)$（When $ i = j $, gradient is $\text{Softmax}(\mathbf{x})_i (1 - \text{Softmax}(\mathbf{x})_i)$）
- 当 $i \neq j $ 时，梯度为 $-\text{Softmax}(\mathbf{x})_i \text{Softmax}(\mathbf{x})_j $（When $ i \neq j $, gradient is $-\text{Softmax}(\mathbf{x})_i \text{Softmax}(\mathbf{x})_j$）

![Softmax Activation Function](../../images/activation_functions/softmax.png)

**好处和用途：**
- **概率输出**：输出是有效的概率分布（Output is valid probability distribution），所有元素非负且和为1（All elements non-negative and sum to 1）
- **多分类**：适用于多分类问题的输出层（Suitable for output layer of multi-class classification）
- **数值稳定性**：实际实现时通常减去最大值（Numerical stability: usually subtract maximum in practice）：$\text{Softmax}(\mathbf{x})_i = \frac{e^{x_i - \max(\mathbf{x})}}{\sum_{j=1}^{K} e^{x_j - \max(\mathbf{x})}}$
- **用途**：多分类神经网络的输出层（Output layer of multi-class neural networks），通常与交叉熵损失函数配合使用（Usually used with cross-entropy loss function）

#### 2. LLM中的新型激活函数

##### 2.1 GELU（Gaussian Error Linear Unit）

**数学公式：**

$$\text{GELU}(x) = x \cdot \Phi(x) = x \cdot \frac{1}{2}\left(1 + \text{erf}\left(\frac{x}{\sqrt{2}}\right)\right)$$

**符号解释：**
- $\text{GELU}(x)$：GELU函数（Gaussian Error Linear Unit）
- $\Phi(x)$：标准正态分布的累积分布函数（Cumulative distribution function of standard normal distribution）
- $\text{erf}(x) = \frac{2}{\sqrt{\pi}} \int_0^x e^{-t^2} dt$：误差函数（Error function）
- $\sqrt{2}$：$\sqrt{2} \approx 1.414$（Square root of 2）

**近似公式（常用）：**

$$\text{GELU}(x) \approx 0.5x\left(1 + \tanh\left(\sqrt{\frac{2}{\pi}}\left(x + 0.044715x^3\right)\right)\right)$$

**符号解释：**
- $\tanh$：双曲正切函数（Hyperbolic tangent function）
- $\sqrt{\frac{2}{\pi}} \approx 0.797885$（Square root of 2/π）
- $0.044715$：近似系数（Approximation coefficient）

**输出范围：**

$$(-\infty, +\infty)$$

**符号解释：**
- 输出可以是任意实数（Output can be any real number）
- 当 $x \to -\infty $ 时，$\text{GELU}(x) \to 0 $（When $ x $ tends to negative infinity, $\text{GELU}(x)$ tends to $ 0$）
- 当 $x \to +\infty $ 时，$\text{GELU}(x) \approx x $（When $ x $ tends to positive infinity, $\text{GELU}(x) \approx x$）

![GELU Activation Function](../../images/activation_functions/gelu.png)

**好处和用途：**
- **概率解释**：基于高斯分布的累积分布函数，有概率解释（Based on Gaussian CDF, has probabilistic interpretation）
- **平滑性**：处处可导，梯度平滑（Differentiable everywhere, smooth gradient）
- **用途**：BERT、GPT等Transformer模型（BERT, GPT and other Transformer models），替代ReLU（Alternative to ReLU）

##### 2.2 Swish（Self-Gated Activation）

**数学公式：**

$$\text{Swish}(x) = x \cdot \sigma(x) = \frac{x}{1 + e^{-x}}$$

**符号解释：**
- $\text{Swish}(x)$：Swish函数（Self-gated activation）
- $\sigma(x)$：Sigmoid函数（Sigmoid function）
- $x \cdot \sigma(x)$：输入与Sigmoid的乘积（Product of input and Sigmoid）

**输出范围：**

$$(-\infty, +\infty)$$

**符号解释：**
- 输出可以是任意实数（Output can be any real number）
- 当 $x \to -\infty $ 时，$\text{Swish}(x) \to 0 $（When $ x $ tends to negative infinity, $\text{Swish}(x)$ tends to $ 0$）
- 当 $x \to +\infty $ 时，$\text{Swish}(x) \approx x $（When $ x $ tends to positive infinity, $\text{Swish}(x) \approx x$）

![Swish/SiLU Activation Function](../../images/activation_functions/swish_silu.png)

**好处和用途：**
- **自门控**：输入本身作为门控信号（Input itself acts as gate signal）
- **平滑性**：比ReLU更平滑（Smoother than ReLU）
- **性能**：在某些任务上比ReLU表现更好（Better performance than ReLU on some tasks）
- **用途**：深度学习中替代ReLU（Alternative to ReLU in deep learning）

##### 2.3 SiLU（Sigmoid Linear Unit）

**数学公式：**

$$\text{SiLU}(x) = x \cdot \sigma(x) = \frac{x}{1 + e^{-x}}$$

**符号解释：**
- $\text{SiLU}(x)$：SiLU函数（Sigmoid Linear Unit）
- 与Swish函数相同（Same as Swish function）
- $x \cdot \sigma(x)$：输入与Sigmoid的乘积（Product of input and Sigmoid）

**输出范围：**

$$(-\infty, +\infty)$$

**好处和用途：**
- **与Swish相同**：SiLU和Swish是同一个函数（SiLU and Swish are the same function）
- **用途**：在Transformer和某些CNN中使用（Used in Transformers and some CNNs）

##### 2.4 GLU（Gated Linear Unit）

**数学公式：**

$$\text{GLU}(x) = (W_1 x + b_1) \odot \sigma(W_2 x + b_2)$$

**符号解释：**
- $\text{GLU}(x)$：GLU函数（Gated Linear Unit）
- $W_1, W_2$：权重矩阵（Weight matrices）
- $b_1, b_2$：偏置向量（Bias vectors）
- $\odot$：逐元素乘法（Element-wise multiplication），也叫Hadamard积（Hadamard product）
- $\sigma(W_2 x + b_2)$：门控信号（Gate signal），Sigmoid输出
- $(W_1 x + b_1)$：线性变换（Linear transformation）

**输出范围：**

$$(-\infty, +\infty)$$

**符号解释：**
- 输出可以是任意实数（Output can be any real number）
- 门控机制控制信息流（Gate mechanism controls information flow）

![GLU Activation Function](../../images/activation_functions/glu.png)

**好处和用途：**
- **门控机制**：通过Sigmoid门控控制信息流（Controls information flow through Sigmoid gate）
- **表达能力**：比普通线性变换更有表达力（More expressive than ordinary linear transformation）
- **用途**：GPT、PaLM等LLM的FFN层（FFN layers in GPT, PaLM and other LLMs），替代标准前馈网络（Alternative to standard feedforward networks）

##### 2.5 GeGLU（Gated Exponential Linear Unit）

**数学公式：**

$$\text{GeGLU}(x) = \text{GELU}(W_1 x + b_1) \odot (W_2 x + b_2)$$

**符号解释：**
- $\text{GeGLU}(x)$：GeGLU函数（Gated Exponential Linear Unit）
- $\text{GELU}(W_1 x + b_1)$：GELU激活的线性变换（Linear transformation with GELU activation）
- $(W_2 x + b_2)$：门控信号（Gate signal），线性变换
- $\odot$：逐元素乘法（Element-wise multiplication）

**输出范围：**

$$(-\infty, +\infty)$$

![GeGLU Activation Function](../../images/activation_functions/geglu.png)

**好处和用途：**
- **结合GELU和GLU**：结合了GELU的平滑性和GLU的门控机制（Combines GELU's smoothness and GLU's gating mechanism）
- **用途**：T5、PaLM等LLM的FFN层（FFN layers in T5, PaLM and other LLMs）

##### 2.6 SwiGLU（Swish-Gated Linear Unit）

**数学公式：**

$$\text{SwiGLU}(x) = \text{Swish}(W_1 x + b_1) \odot (W_2 x + b_2)$$

**符号解释：**
- $\text{SwiGLU}(x)$：SwiGLU函数（Swish-Gated Linear Unit）
- $\text{Swish}(W_1 x + b_1)$：Swish激活的线性变换（Linear transformation with Swish activation）
- $(W_2 x + b_2)$：门控信号（Gate signal）
- $\odot$：逐元素乘法（Element-wise multiplication）

**输出范围：**

$$(-\infty, +\infty)$$

![SwiGLU Activation Function](../../images/activation_functions/swiglu.png)

**好处和用途：**
- **结合Swish和GLU**：结合了Swish的平滑性和GLU的门控机制（Combines Swish's smoothness and GLU's gating mechanism）
- **用途**：PaLM、LLaMA等LLM的FFN层（FFN layers in PaLM, LLaMA and other LLMs），通常比标准FFN表现更好（Usually performs better than standard FFN）

---

### 二、口语化答案

#### 中文版本

激活函数是神经网络中的非线性变换，引入非线性使网络能学习复杂模式。

传统激活函数包括Sigmoid、Tanh、ReLU、Leaky ReLU、ELU。Sigmoid输出范围是0到1，适合概率输出，但容易梯度消失。Tanh输出范围是-1到1，零中心化，梯度比Sigmoid大，但仍有梯度消失问题。ReLU输出范围是0到正无穷，计算简单，缓解梯度消失，但负值无梯度导致死亡ReLU问题。Leaky ReLU允许小的负值，解决死亡ReLU问题。ELU负值区域平滑，输出接近零均值。

LLM中的新型激活函数包括GELU、Swish、SiLU、GLU、GeGLU、SwiGLU。GELU基于高斯分布的累积分布函数，有概率解释，在BERT、GPT等Transformer中使用。Swish是自门控激活，输入本身作为门控信号，比ReLU更平滑。SiLU和Swish是同一个函数。GLU是门控线性单元，通过Sigmoid门控控制信息流，在GPT、PaLM等LLM的FFN层使用。GeGLU结合GELU和GLU，在T5、PaLM中使用。SwiGLU结合Swish和GLU，在PaLM、LLaMA中使用，通常比标准FFN表现更好。

#### English Version

Activation functions are nonlinear transformations in neural networks, introducing nonlinearity to enable networks to learn complex patterns.

Traditional activation functions include Sigmoid, Tanh, ReLU, Leaky ReLU, ELU, Softmax. Sigmoid output range is 0 to 1, suitable for probability output, but prone to gradient vanishing. Tanh output range is -1 to 1, zero-centered, larger gradient than Sigmoid, but still has gradient vanishing problem. ReLU output range is 0 to positive infinity, simple computation, alleviates gradient vanishing, but no gradient for negative values causes dying ReLU problem. Leaky ReLU allows small negative values, solves dying ReLU problem. ELU has smooth negative region, output close to zero mean. Softmax outputs probability distribution, all elements in (0,1) and sum to 1, suitable for multi-class classification output layer.

New activation functions in LLMs include GELU, Swish, SiLU, GLU, GeGLU, SwiGLU. GELU is based on Gaussian CDF, has probabilistic interpretation, used in BERT, GPT and other Transformers. Swish is self-gated activation, input itself acts as gate signal, smoother than ReLU. SiLU and Swish are the same function. GLU is gated linear unit, controls information flow through Sigmoid gate, used in FFN layers of GPT, PaLM and other LLMs. GeGLU combines GELU and GLU, used in T5, PaLM. SwiGLU combines Swish and GLU, used in PaLM, LLaMA, usually performs better than standard FFN.

---

### 三、激活函数对比表格

| 激活函数 | 公式 | 输出范围 | 梯度特性 | 主要优点 | 主要缺点 | 典型用途 |
|---------|------|---------|---------|---------|---------|---------|
| **Sigmoid** | $\sigma(x) = \frac{1}{1+e^{-x}}$ | $(0, 1)$ | 最大梯度0.25，易梯度消失 | 输出为概率，平滑可导 | 梯度消失，计算慢 | 二分类输出层 |
| **Tanh** | $\tanh(x) = \frac{e^x-e^{-x}}{e^x+e^{-x}}$ | $(-1, 1)$ | 最大梯度1，比Sigmoid大 | 零中心化，梯度更大 | 仍有梯度消失 | RNN隐藏层 |
| **ReLU** | $\text{ReLU}(x) = \max(0, x)$ | $[0, +\infty)$ | 正区间梯度为1 | 计算快，缓解梯度消失，稀疏激活 | 死亡ReLU，负值无梯度 | 深度网络隐藏层，CNN |
| **Leaky ReLU** | $\max(\alpha x, x), \alpha=0.01 $ | $(-\infty, +\infty)$ | 正区间1，负区间$\alpha $ | 解决死亡ReLU，保持稀疏性 | 需要调参$\alpha$ | 替代ReLU，GAN |
| **ELU** | $x $ if $ x>0 $, else $\alpha(e^x-1)$ | $(-\alpha, +\infty)$ | 正区间1，负区间平滑 | 平滑性，零均值 | 计算较慢 | 需要平滑激活的场景 |
| **Softmax** | $\frac{e^{x_i}}{\sum_j e^{x_j}}$ | $(0, 1)$，和为1 | 梯度与输出相关 | 概率分布，多分类 | 数值稳定性需注意 | 多分类输出层 |
| **GELU** | $x \cdot \Phi(x)$ | $(-\infty, +\infty)$ | 平滑梯度 | 概率解释，平滑 | 计算较复杂 | Transformer（BERT, GPT） |
| **Swish/SiLU** | $x \cdot \sigma(x)$ | $(-\infty, +\infty)$ | 平滑梯度 | 自门控，比ReLU平滑 | 计算比ReLU慢 | 深度学习替代ReLU |
| **GLU** | $(W_1x+b_1) \odot \sigma(W_2x+b_2)$ | $(-\infty, +\infty)$ | 门控梯度 | 门控机制，表达力强 | 参数更多 | LLM的FFN层（GPT, PaLM） |
| **GeGLU** | $\text{GELU}(W_1x+b_1) \odot (W_2x+b_2)$ | $(-\infty, +\infty)$ | 平滑门控梯度 | 结合GELU和GLU | 参数多，计算复杂 | LLM的FFN层（T5, PaLM） |
| **SwiGLU** | $\text{Swish}(W_1x+b_1) \odot (W_2x+b_2)$ | $(-\infty, +\infty)$ | 平滑门控梯度 | 结合Swish和GLU，性能好 | 参数多，计算复杂 | LLM的FFN层（PaLM, LLaMA） |

**记忆要点：**
- **传统激活**：Sigmoid/Tanh用于RNN，ReLU用于CNN/DNN，Softmax用于多分类输出
- **LLM激活**：GELU用于Transformer，GLU变体用于FFN层，SwiGLU性能最好
- **选择原则**：隐藏层用ReLU/GELU，多分类输出用Softmax，LLM的FFN用GLU变体

---

## 📚 导航

← [上一题: 模型比较评估](model_comparison_evaluation.md)

→ [下一题: 常用损失函数](loss_functions.md)