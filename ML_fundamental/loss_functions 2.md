# 常用损失函数

## 问题：常用的损失函数有哪些？它们的公式、输出范围、好处和用途是什么？

### 一、详细中文解释

损失函数（Loss Function）衡量模型预测与真实值之间的差异，是机器学习模型优化的目标。不同的损失函数有不同的特性，适用于不同的任务和场景。

#### 1. 回归损失函数

##### 1.1 MSE（Mean Squared Error，均方误差）

**数学公式：**

$$L_{\text{MSE}} = \frac{1}{n} \sum_{i=1}^{n} (y_i - \hat{y}_i)^2$$

**符号解释：**
- $L_{\text{MSE}}$：均方误差（Mean Squared Error）
- $n$：样本数量（Number of samples）
- $y_i $：第 $ i $ 个样本的真实值（True value of the $ i$-th sample）
- $\hat{y}_i $：第 $ i $ 个样本的预测值（Predicted value of the $ i$-th sample）
- $(y_i - \hat{y}_i)^2$：预测误差的平方（Squared prediction error）
- $\sum_{i=1}^{n}$：对所有样本求和（Sum over all samples）

**输出范围：**

$$[0, +\infty)$$

**符号解释：**
- 最小值为 $0 $（Minimum value is $ 0$），当所有预测完全正确时（When all predictions are completely correct）
- 最大值无上界（Maximum value is unbounded），误差越大，损失越大（Larger error leads to larger loss）

**梯度：**

$$\frac{\partial L_{\text{MSE}}}{\partial \hat{y}_i} = -2(y_i - \hat{y}_i)$$

**符号解释：**
- $\frac{\partial L_{\text{MSE}}}{\partial \hat{y}_i}$：损失函数对预测值的梯度（Gradient of loss function with respect to prediction）
- 梯度与误差成正比（Gradient is proportional to error）
- 大误差产生大梯度（Large errors produce large gradients）

![MSE Loss Function](../../images/loss_functions/mse_loss.png)

**好处和用途：**
- **数学性质好**：处处可导，凸函数（Differentiable everywhere, convex function）
- **对大误差敏感**：平方项使得大误差被放大（Squared term amplifies large errors）
- **用途**：回归问题（Regression problems），假设误差服从高斯分布（Assumes errors follow Gaussian distribution）
- **缺点**：对异常值敏感（Sensitive to outliers）

##### 1.2 MAE（Mean Absolute Error，平均绝对误差）

**数学公式：**

$$L_{\text{MAE}} = \frac{1}{n} \sum_{i=1}^{n} |y_i - \hat{y}_i|$$

**符号解释：**
- $L_{\text{MAE}}$：平均绝对误差（Mean Absolute Error）
- $|y_i - \hat{y}_i|$：预测误差的绝对值（Absolute value of prediction error）

**输出范围：**

$$[0, +\infty)$$

**符号解释：**
- 最小值为 $0 $（Minimum value is $ 0$）
- 最大值无上界（Maximum value is unbounded）

**梯度：**

$$\frac{\partial L_{\text{MAE}}}{\partial \hat{y}_i} = \begin{cases} -1 & \text{if } \hat{y}_i < y_i \\ +1 & \text{if } \hat{y}_i > y_i \\ \text{undefined} & \text{if } \hat{y}_i = y_i \end{cases}$$

**符号解释：**
- 梯度在误差为0处不可导（Gradient is not differentiable at zero error）
- 梯度大小恒为1（Gradient magnitude is constant 1），不受误差大小影响（Not affected by error magnitude）

![MAE Loss Function](../../images/loss_functions/mae_loss.png)

**好处和用途：**
- **对异常值鲁棒**：绝对值使得异常值的影响较小（Absolute value makes outliers less influential）
- **用途**：回归问题，特别是存在异常值时（Regression problems, especially when outliers exist）
- **缺点**：在0处不可导（Not differentiable at zero）

##### 1.3 Huber Loss

**数学公式：**

$$L_{\text{Huber}} = \begin{cases} \frac{1}{2}(y_i - \hat{y}_i)^2 & \text{if } |y_i - \hat{y}_i| \leq \delta \\ \delta|y_i - \hat{y}_i| - \frac{1}{2}\delta^2 & \text{if } |y_i - \hat{y}_i| > \delta \end{cases}$$

**符号解释：**
- $L_{\text{Huber}}$：Huber损失（Huber Loss）
- $\delta $：阈值参数（Threshold parameter），通常取 $ 1.0 $（Typically $ 1.0$）
- 当误差小于等于 $\delta $ 时，使用MSE（When error $\leq \delta$, use MSE）
- 当误差大于 $\delta $ 时，使用MAE（When error $> \delta$, use MAE）

**输出范围：**

$$[0, +\infty)$$

![Huber Loss Function](../../images/loss_functions/huber_loss.png)

**好处和用途：**
- **结合MSE和MAE**：小误差时像MSE（平滑），大误差时像MAE（鲁棒）（Small errors like MSE (smooth), large errors like MAE (robust)）
- **对异常值鲁棒**：大误差时使用线性惩罚（Linear penalty for large errors）
- **用途**：回归问题，特别是需要平衡平滑性和鲁棒性时（Regression problems, especially when balancing smoothness and robustness）

##### 1.4 Quantile Loss（分位数损失）

**数学公式：**

$$L_{\text{Quantile}} = \sum_{i=1}^{n} \begin{cases} \tau(y_i - \hat{y}_i) & \text{if } y_i \geq \hat{y}_i \\ (1-\tau)(\hat{y}_i - y_i) & \text{if } y_i < \hat{y}_i \end{cases}$$

**符号解释：**
- $L_{\text{Quantile}}$：分位数损失（Quantile Loss）
- $\tau $：分位数参数（Quantile parameter），$ 0 < \tau < 1$（Between 0 and 1）
- 当预测值小于真实值时，使用权重 $\tau $（When prediction < true value, use weight $\tau$）
- 当预测值大于真实值时，使用权重 $1-\tau $（When prediction > true value, use weight $ 1-\tau$）

**输出范围：**

$$[0, +\infty)$$

![Quantile Loss Function](../../images/loss_functions/quantile_loss.png)

**好处和用途：**
- **分位数回归**：可以预测不同分位数（Can predict different quantiles）
- **非对称惩罚**：对不同方向的误差给予不同权重（Asymmetric penalty for errors in different directions）
- **用途**：需要预测分位数的场景，如风险预测（Scenarios requiring quantile prediction, such as risk prediction）

#### 2. 分类损失函数

##### 2.1 Binary Cross-Entropy（二分类交叉熵）

**数学公式：**

$$L_{\text{BCE}} = -\frac{1}{n} \sum_{i=1}^{n} [y_i \log(\hat{y}_i) + (1-y_i) \log(1-\hat{y}_i)]$$

**符号解释：**
- $L_{\text{BCE}}$：二分类交叉熵（Binary Cross-Entropy）
- $y_i \in \{0, 1\}$：真实标签（True label），0或1
- $\hat{y}_i \in (0, 1)$：预测概率（Predicted probability），在0和1之间
- $\log$：自然对数（Natural logarithm）
- $y_i \log(\hat{y}_i)$：当真实标签为1时的损失项（Loss term when true label is 1）
- $(1-y_i) \log(1-\hat{y}_i)$：当真实标签为0时的损失项（Loss term when true label is 0）

**输出范围：**

$$[0, +\infty)$$

**符号解释：**
- 最小值为 $0 $（Minimum value is $ 0$），当预测完全正确时（When predictions are completely correct）
- 当预测完全错误时，损失趋向无穷（When predictions are completely wrong, loss tends to infinity）

**梯度：**

$$\frac{\partial L_{\text{BCE}}}{\partial \hat{y}_i} = -\frac{y_i}{\hat{y}_i} + \frac{1-y_i}{1-\hat{y}_i} = \frac{\hat{y}_i - y_i}{\hat{y}_i(1-\hat{y}_i)}$$

**符号解释：**
- 梯度与预测误差成正比（Gradient is proportional to prediction error）
- 当预测接近0或1时，梯度可能很大（When prediction is close to 0 or 1, gradient may be large）

![Binary Cross-Entropy Loss Function](../../images/loss_functions/binary_cross_entropy_loss.png)

**好处和用途：**
- **概率解释**：适用于概率输出（Suitable for probability output）
- **用途**：二分类问题（Binary classification problems），通常配合Sigmoid激活函数（Usually with Sigmoid activation）

##### 2.2 Multi-Class Cross-Entropy（多分类交叉熵）

**数学公式：**

$$L_{\text{CCE}} = -\frac{1}{n} \sum_{i=1}^{n} \sum_{c=1}^{C} y_{ic} \log(\hat{y}_{ic})$$

**符号解释：**
- $L_{\text{CCE}}$：多分类交叉熵（Multi-Class Cross-Entropy），也叫Categorical Cross-Entropy
- $C$：类别数量（Number of classes）
- $y_{ic}$：第 $ i $ 个样本属于类别 $ c $ 的真实标签（True label of the $ i $-th sample for class $ c$），one-hot编码（One-hot encoding）
- $\hat{y}_{ic}$：第 $ i $ 个样本属于类别 $ c $ 的预测概率（Predicted probability of the $ i $-th sample for class $ c$）
- $\sum_{c=1}^{C}$：对所有类别求和（Sum over all classes）

**输出范围：**

$$[0, +\infty)$$

![Multi-Class Cross-Entropy Loss Function](../../images/loss_functions/multi_class_cross_entropy_loss.png)

**好处和用途：**
- **多分类问题**：适用于多分类任务（Suitable for multi-class classification）
- **用途**：多分类问题（Multi-class classification problems），通常配合Softmax激活函数（Usually with Softmax activation）

##### 2.3 Focal Loss

**数学公式：**

$$L_{\text{Focal}} = -\alpha_t (1-p_t)^{\gamma} \log(p_t)$$

**符号解释：**
- $L_{\text{Focal}}$：Focal损失（Focal Loss）
- $\alpha_t$：类别权重（Class weight），平衡类别不平衡（Balances class imbalance）
- $p_t$：预测概率（Predicted probability），对于正确类别（For correct class）
- $\gamma $：聚焦参数（Focusing parameter），通常取 $ 2 $（Typically $ 2$）
- $(1-p_t)^{\gamma}$：调制因子（Modulating factor），降低易分类样本的权重（Reduces weight of easy examples）

**输出范围：**

$$[0, +\infty)$$

![Focal Loss Function](../../images/loss_functions/focal_loss.png)

**好处和用途：**
- **处理类别不平衡**：通过 $\alpha_t $ 平衡类别（Balances classes through $\alpha_t$）
- **聚焦难样本**：通过 $(1-p_t)^{\gamma}$ 聚焦难分类样本（Focuses on hard examples through $(1-p_t)^{\gamma}$）
- **用途**：目标检测（Object detection），特别是类别不平衡的场景（Especially in class-imbalanced scenarios）

##### 2.4 Hinge Loss（合页损失）

**数学公式：**

$$L_{\text{Hinge}} = \max(0, 1 - y_i \hat{y}_i)$$

**符号解释：**
- $L_{\text{Hinge}}$：Hinge损失（Hinge Loss）
- $y_i \in \{-1, +1\}$：真实标签（True label），-1或+1
- $\hat{y}_i$：预测值（Predicted value），可以是任意实数（Can be any real number）
- $y_i \hat{y}_i $：当 $ y_i $ 和 $\hat{y}_i $ 同号且乘积大于1时，损失为0（When $ y_i $ and $\hat{y}_i$ have same sign and product > 1, loss is 0）

**输出范围：**

$$[0, +\infty)$$

![Hinge Loss Function](../../images/loss_functions/hinge_loss.png)

**好处和用途：**
- **最大间隔**：鼓励分类间隔最大化（Encourages maximum classification margin）
- **用途**：支持向量机（SVM，Support Vector Machines），二分类问题（Binary classification problems）

#### 3. 特殊损失函数

##### 3.1 KL散度（Kullback-Leibler Divergence）

**数学公式：**

$$D_{\text{KL}}(P||Q) = \sum_{i} P(i) \log\frac{P(i)}{Q(i)}$$

**符号解释：**
- $D_{\text{KL}}(P||Q)$：KL散度（Kullback-Leibler Divergence），衡量分布 $ P $ 和 $ Q $ 的差异（Measures difference between distributions $ P $ and $ Q$）
- $P(i)$：分布 $ P $ 在 $ i $ 处的概率（Probability of distribution $ P $ at $ i$）
- $Q(i)$：分布 $ Q $ 在 $ i $ 处的概率（Probability of distribution $ Q $ at $ i$）
- $\log\frac{P(i)}{Q(i)}$：对数比率（Log ratio）

**输出范围：**

$$[0, +\infty)$$

**符号解释：**
- 最小值为 $0 $（Minimum value is $ 0 $），当 $ P = Q $ 时（When $ P = Q$）
- 非对称（Asymmetric）：$D_{\text{KL}}(P||Q) \neq D_{\text{KL}}(Q||P)$

![KL Divergence Loss Function](../../images/loss_functions/kl_divergence_loss.png)

**好处和用途：**
- **分布距离**：衡量两个概率分布的差异（Measures difference between two probability distributions）
- **用途**：变分自编码器（VAE，Variational Autoencoders），知识蒸馏（Knowledge Distillation），模型正则化（Model regularization）

##### 3.2 Contrastive Loss（对比损失）

**数学公式：**

$$L_{\text{Contrastive}} = \frac{1}{2n} \sum_{i=1}^{n} [y_i d_i^2 + (1-y_i) \max(0, m-d_i)^2]$$

**符号解释：**
- $L_{\text{Contrastive}}$：对比损失（Contrastive Loss）
- $y_i \in \{0, 1\}$：样本对是否相似（Whether sample pair is similar），1表示相似，0表示不相似
- $d_i$：样本对之间的距离（Distance between sample pair），通常是欧氏距离（Usually Euclidean distance）
- $m$：边界参数（Margin parameter），不相似样本对的最小距离（Minimum distance for dissimilar pairs）

**输出范围：**

$$[0, +\infty)$$

![Contrastive Loss Function](../../images/loss_functions/contrastive_loss.png)

**好处和用途：**
- **学习表示**：学习使得相似样本接近、不相似样本远离的表示（Learns representations that bring similar samples close and dissimilar samples far）
- **用途**：度量学习（Metric Learning），人脸识别（Face Recognition），相似度学习（Similarity Learning）

---

### 二、口语化答案

#### 中文版本

损失函数衡量模型预测与真实值的差异，是模型优化的目标。

回归损失包括MSE、MAE、Huber Loss、Quantile Loss。MSE是均方误差，输出范围0到正无穷，对大误差敏感，对异常值敏感，适用于回归问题。MAE是平均绝对误差，输出范围0到正无穷，对异常值鲁棒，但在0处不可导。Huber Loss结合MSE和MAE，小误差时像MSE，大误差时像MAE，对异常值鲁棒。Quantile Loss是分位数损失，可以预测不同分位数，非对称惩罚，适用于需要预测分位数的场景。

分类损失包括Binary Cross-Entropy、Multi-Class Cross-Entropy、Focal Loss、Hinge Loss。Binary Cross-Entropy是二分类交叉熵，输出范围0到正无穷，适用于概率输出，通常配合Sigmoid。Multi-Class Cross-Entropy是多分类交叉熵，适用于多分类任务，通常配合Softmax。Focal Loss通过调制因子聚焦难样本，处理类别不平衡，适用于目标检测。Hinge Loss鼓励最大间隔，适用于SVM。

特殊损失包括KL散度和Contrastive Loss。KL散度衡量两个概率分布的差异，非对称，适用于VAE、知识蒸馏。Contrastive Loss学习使得相似样本接近、不相似样本远离的表示，适用于度量学习、人脸识别。

#### English Version

Loss functions measure the difference between model predictions and true values, serving as optimization objectives.

Regression losses include MSE, MAE, Huber Loss, Quantile Loss. MSE is mean squared error, output range 0 to positive infinity, sensitive to large errors and outliers, suitable for regression. MAE is mean absolute error, output range 0 to positive infinity, robust to outliers but not differentiable at zero. Huber Loss combines MSE and MAE, like MSE for small errors, like MAE for large errors, robust to outliers. Quantile Loss can predict different quantiles, asymmetric penalty, suitable for scenarios requiring quantile prediction.

Classification losses include Binary Cross-Entropy, Multi-Class Cross-Entropy, Focal Loss, Hinge Loss. Binary Cross-Entropy is for binary classification, output range 0 to positive infinity, suitable for probability output, usually with Sigmoid. Multi-Class Cross-Entropy is for multi-class tasks, usually with Softmax. Focal Loss focuses on hard examples through modulating factor, handles class imbalance, suitable for object detection. Hinge Loss encourages maximum margin, suitable for SVM.

Special losses include KL Divergence and Contrastive Loss. KL Divergence measures difference between two probability distributions, asymmetric, suitable for VAE, knowledge distillation. Contrastive Loss learns representations that bring similar samples close and dissimilar samples far, suitable for metric learning, face recognition.

---

### 三、损失函数对比表格

| 损失函数 | 公式 | 输出范围 | 梯度特性 | 主要优点 | 主要缺点 | 典型用途 | 配合激活函数 |
|---------|------|---------|---------|---------|---------|---------|------------|
| **MSE** | $\frac{1}{n}\sum(y_i-\hat{y}_i)^2 $ | $[0, +\infty)$ | 与误差成正比 | 数学性质好，凸函数 | 对异常值敏感 | 回归问题 | 无（直接输出） |
| **MAE** | $\frac{1}{n}\sum\|y_i-\hat{y}_i\|$ | $[0, +\infty)$ | 恒为±1 | 对异常值鲁棒 | 在0处不可导 | 回归（有异常值） | 无（直接输出） |
| **Huber Loss** | MSE if $\|e\|\leq\delta $, else MAE | $[0, +\infty)$ | 小误差平滑，大误差线性 | 结合MSE和MAE优点 | 需要调参$\delta$ | 回归（平衡平滑和鲁棒） | 无（直接输出） |
| **Quantile Loss** | $\tau\|e\|$ if $ e\geq0 $, else $(1-\tau)\|e\|$ | $[0, +\infty)$ | 非对称梯度 | 可预测分位数 | 需要选择$\tau$ | 分位数回归 | 无（直接输出） |
| **Binary Cross-Entropy** | $-\frac{1}{n}\sum[y_i\log\hat{y}_i+(1-y_i)\log(1-\hat{y}_i)]$ | $[0, +\infty)$ | $\frac{\hat{y}_i-y_i}{\hat{y}_i(1-\hat{y}_i)}$ | 概率解释，适合二分类 | 预测接近0/1时梯度大 | 二分类问题 | **Sigmoid** |
| **Multi-Class Cross-Entropy** | $-\frac{1}{n}\sum_i\sum_c y_{ic}\log\hat{y}_{ic}$ | $[0, +\infty)$ | 与预测误差相关 | 多分类，概率解释 | 类别多时计算量大 | 多分类问题 | **Softmax** |
| **Focal Loss** | $-\alpha_t(1-p_t)^{\gamma}\log(p_t)$ | $[0, +\infty)$ | 聚焦难样本 | 处理类别不平衡，聚焦难样本 | 需要调参$\alpha,\gamma$ | 目标检测 | Sigmoid/Softmax |
| **Hinge Loss** | $\max(0, 1-y_i\hat{y}_i)$ | $[0, +\infty)$ | 在边界处不连续 | 最大间隔，稀疏性 | 不可导在边界 | SVM | 无（直接输出） |
| **KL Divergence** | $\sum_i P(i)\log\frac{P(i)}{Q(i)}$ | $[0, +\infty)$ | 非对称 | 衡量分布差异 | 非对称，非距离 | VAE、知识蒸馏 | Softmax |
| **Contrastive Loss** | $y_id_i^2+(1-y_i)\max(0,m-d_i)^2 $ | $[0, +\infty)$ | 与距离相关 | 学习表示，相似度学习 | 需要选择$ m$ | 度量学习、人脸识别 | 无（直接输出） |

**记忆要点：**
- **回归损失**：MSE常用但敏感，MAE鲁棒但不可导，Huber平衡两者，Quantile用于分位数
- **分类损失**：BCE配Sigmoid用于二分类，CCE配Softmax用于多分类，Focal处理不平衡，Hinge用于SVM
- **特殊损失**：KL用于分布距离，Contrastive用于表示学习
- **选择原则**：回归用MSE/MAE，二分类用BCE，多分类用CCE，不平衡用Focal，SVM用Hinge

---

## 📚 导航

← [上一题: 常用激活函数](activation_functions.md)

→ [下一题: 正则化基础知识](../regularization/0_basic_concepts.md)