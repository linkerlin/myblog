---
title: Discrete Diffusion Models for Language Generation 智能记忆学习材料

---

## 学习目标
通过精心设计的选择题和原文对照，帮助学习者掌握核心知识点

## 使用说明
请仔细阅读每个问题，对照原文理解解析

## 题目与解析


---

### 问题 1
**知识点：** Autoregressive models (AR)
**题目：** 什么是自回归模型（AR模型）的主要生成方式？
**选项：**
- A. 并行生成所有标记
- B. 基于变分下界优化
- C. 逐个标记顺序生成
- D. 通过噪声添加和去噪过程

---

**正确答案：** C
**原文依据：** 「Natural language processing (NLP) relies heavily on autoregressive (AR) models, especially when it comes to sequence generation tasks. These models successfully capture the conditional dependencies inherent in language by generating text by predicting each token based on the order of preceding tokens.」（出自：Discrete Diffusion Models for Language Generation.pdf，第21页）
**解析：** 自回归模型通过顺序预测每个标记来生成序列，这使得它们能够捕捉语言中的条件依赖，但也限制了并行计算的能力。


---


### 问题 2
**知识点：** Discrete Denoising Diffusion Probabilistic Models (D3PM)
**题目：** D3PM模型的核心过程包括什么？
**选项：**
- A. 前向噪声添加和后向去噪
- B. 仅顺序预测标记
- C. 嵌入向量转换
- D. 变分自编码器训练

---

**正确答案：** A
**原文依据：** 「The primary objective during training is to minimize the loss function that quantifies the difference between the original data and the model's reconstruction at each denoising step. Through this optimization process, the model progressively improves its generative capability by learning a highfidility reverse diffusion path.」（出自：Discrete Diffusion Models for Language Generation.pdf，第22页）
**解析：** D3PM是一种离散扩散模型，通过前向过程添加噪声，后向过程学习去噪来生成数据，这与连续扩散模型不同，适用于离散领域如语言。


---


### 问题 3
**知识点：** Forward diffusion process
**题目：** 前向扩散过程在扩散模型中的作用是什么？
**选项：**
- A. 直接生成新序列
- B. 优化模型参数
- C. 训练去噪网络
- D. 逐步向数据添加噪声

---

**正确答案：** D
**原文依据：** 「In the forward process, noise is incrementally added to the input data across a series of timesteps, gradually transforming the data into a noiselike distribution.」（出自：Discrete Diffusion Models for Language Generation.pdf，第24页）
**解析：** 前向过程将原始数据逐步转化为噪声分布，这是一个可逆的Markov过程，为后向去噪提供基础。


---


### 问题 4
**知识点：** Backward diffusion process
**题目：** 后向扩散过程的主要目标是什么？
**选项：**
- A. 添加更多噪声
- B. 从噪声中恢复原始数据
- C. 计算嵌入向量
- D. 顺序生成标记

---

**正确答案：** B
**原文依据：** 「We can demonstrate, from a statistical standpoint, that the training objective to diffusion models involves minimizing a loss function derived from the Variational Lower Bound (VLB) [10]. This objective enables the model to learn the denoising process in a step-by-step manner by approximating the reverse transitions from...」（出自：Discrete Diffusion Models for Language Generation.pdf，第23页）
**解析：** 后向过程学习逆转前向噪声添加，通过逐步去噪从噪声分布生成数据样本。


---


### 问题 5
**知识点：** Bits Per Token (BPT)
**题目：** Bits Per Token (BPT)指标用于衡量什么？
**选项：**
- A. 模型的压缩效率
- B. 生成速度
- C. 噪声添加水平
- D. 序列长度

---

**正确答案：** A
**原文依据：** 「In the evaluation of language models, Bits Per Token (BPT) is a fundamental metric that quantifies the average number of bits required to encode each token[ $\mathrm{}$ is given sequence.」（出自：Discrete Diffusion Models for Language Generation.pdf，第18页）
**解析：** BPT衡量模型对每个标记的编码位数，越低表示模型预测效率越高，压缩能力越强。


---


### 问题 6
**知识点：** Perplexity (PPL)
**题目：** 困惑度（PPL）指标反映了模型的什么能力？
**选项：**
- A. 并行计算速度
- B. 噪声去噪效果
- C. 预测未见数据的概率
- D. 嵌入向量维度

---

**正确答案：** C
**原文依据：** 「Perplexity (PPL) Section 2.5, an indicator of the model's ability to predict unseen data.」（出自：Discrete Diffusion Models for Language Generation.pdf，第17页）
**解析：** PPL是模型对测试数据困惑程度的度量，低PPL表示模型更好地预测序列概率。


---


### 问题 7
**知识点：** Negative Log-Likelihood (NLL)
**题目：** 负对数似然（NLL）作为损失函数的作用是什么？
**选项：**
- A. 衡量生成速度
- B. 计算噪声分布
- C. 评估模型预测分布
- D. 量化预测概率的对数负值

---

**正确答案：** D
**原文依据：** 「Negative Log-Likelihood (NLL) (Section 2.6), a loss-based evaluation metric derived from the log-likelihood of the predicted distribution.」（出自：Discrete Diffusion Models for Language Generation.pdf，第17页）
**解析：** NLL量化模型对正确标记的概率赋值，低值表示模型更自信地预测正确分布。


---


### 问题 8
**知识点：** WikiText-103 dataset
**题目：** WikiText-103数据集的特点是什么？
**选项：**
- A. 仅包含字符级任务
- B. 包含完整维基百科文章，保留大小写和标点
- C. 小于Penn Treebank
- D. 用于图像生成

---

**正确答案：** B
**原文依据：** 「Composed of full Wikipedia articles, WikiText-103 is particularly suitable for evaluating models capable of capturing long-range dependencies in text 5.」（出自：Discrete Diffusion Models for Language Generation.pdf，第32页）
**解析：** 该数据集较大，保留了真实语言特征，适合评估长距离依赖建模。


---


### 问题 9
**知识点：** Tokenization
**题目：** 分词（Tokenization）在数据预处理中的作用是什么？
**选项：**
- A. 将句子分解为子词单元
- B. 添加噪声
- C. 计算变分下界
- D. 顺序生成序列

---

**正确答案：** A
**原文依据：** 「Subword Tokenization: The sentence is divided into smaller units (subwords) rather than whole words, which helps handle rare words and improves model generalization.」（出自：Discrete Diffusion Models for Language Generation.pdf，第34页）
**解析：** 分词将文本转换为模型可处理的标记，帮助处理稀有词并提升泛化。


---


### 问题 10
**知识点：** Embedding vectors
**题目：** 嵌入向量在模型输入中的功能是什么？
**选项：**
- A. 噪声添加
- B. 将标记ID转换为多维向量表示
- C. 计算困惑度
- D. 并行去噪

---

**正确答案：** B
**原文依据：** 「Embedding Layer: This is typically a lookup table (e.g., a matrix of embeddings vector for a specific token ID.」（出自：Discrete Diffusion Models for Language Generation.pdf，第35页）
**解析：** 嵌入层将离散标记映射到连续向量空间，便于神经网络处理。


---


### 问题 11
**知识点：** Transformer architecture
**题目：** Transformer架构的核心优势是什么？
**选项：**
- A. 顺序处理序列
- B. 通过注意力机制捕捉长距离依赖
- C. 仅用于扩散模型
- D. 添加噪声过程

---

**正确答案：** C
**原文依据：** 「This section introduces the transformer architecture [15], illustrated in Figure 7.1, which serves as the foundational structure for all three autoregressive models discussed later in this work.」（出自：Discrete Diffusion Models for Language Generation.pdf，第59页）
**解析：** Transformer使用自注意力机制，克服RNN的梯度消失问题，支持并行计算。


---


### 问题 12
**知识点：** Exposure bias in AR models
**题目：** 自回归模型中的暴露偏差（exposure bias）是什么？
**选项：**
- A. 噪声过多
- B. 训练时使用真实前缀，推理时使用生成前缀导致不匹配
- C. 嵌入维度过高
- D. 去噪步骤不足

---

**正确答案：** D
**原文依据：** 「While autoregressive models have set the state-of-the-art in natural language generation, however, they suffer from well-known limitations, including exposure bias and restricted support for parallel computation during inference.」（出自：Discrete Diffusion Models for Language Generation.pdf，第14页）
**解析：** 暴露偏差指训练和推理上下文不一致，可能导致错误累积。


---


### 问题 13
**知识点：** Parallel generation in diffusion models
**题目：** 扩散模型在生成中的并行优势是什么？
**选项：**
- A. 顺序预测每个标记
- B. 仅用于训练阶段
- C. 噪声添加顺序
- D. 同时生成所有标记

---

**正确答案：** B
**原文依据：** 「Unlike AR models, diffusion models generate entire sequences in parallel during inference, offering potential advantages in sampling flexibility, controllability, and robustness against exposure bias.」（出自：Discrete Diffusion Models for Language Generation.pdf，第13页）
**解析：** 扩散模型允许并行去噪，提高推理效率，克服AR的顺序限制。


---


### 问题 14
**知识点：** Masking in D3PM
**题目：** D3PM中的掩码（masking）机制用于什么？
**选项：**
- A. 模拟噪声，通过掩码状态转换
- B. 顺序生成
- C. 计算嵌入
- D. 评估困惑度

---

**正确答案：** A
**原文依据：** 「As an illustrative example, assume $f_1=0.3$. The corresponding $Q$ matrix becomes: ... the token 'Saman' (One-hot vec: $[1,0,0,0,0]$ ) and Vec $Q_1$ remains itself with a probability of 0.7, while it transi- tions to the mask state with a probability of 0.3 .」（出自：Discrete Diffusion Models for Language Generation.pdf，第25页）
**解析：** 掩码用于模拟离散噪声，帮助模型学习从掩码状态恢复原始标记。


---


### 问题 15
**知识点：** Transition matrix Q
**题目：** 转移矩阵Q在离散扩散中的作用是什么？
**选项：**
- A. 定义嵌入维度
- B. 顺序预测概率
- C. 定义从一个状态到另一个状态的概率
- D. 计算生成速度

---

**正确答案：** D
**原文依据：** 「The transition matrix at each step, of size $(m+1) \times(m+1)$, is given by: $Q=(1-f) I...$」（出自：Discrete Diffusion Models for Language Generation.pdf，第25页）
**解析：** Q矩阵控制每个时间步的状态转移概率，包括保持原状或转向掩码。


---


### 问题 16
**知识点：** Variational Lower Bound (VLB)
**题目：** 变分下界（VLB）在扩散模型训练中的作用是什么？
**选项：**
- A. 直接生成样本
- B. 噪声添加
- C. 优化去噪过程的损失函数
- D. 嵌入转换

---

**正确答案：** C
**原文依据：** 「We can demonstrate, from a statistical standpoint, that the training objective to diffusion models involves minimizing a loss function derived from the Variational Lower Bound (VLB) [10].」（出自：Discrete Diffusion Models for Language Generation.pdf，第23页）
**解析：** VLB提供一个可优化的下界，用于近似最大似然估计，帮助训练去噪模型。


---


### 问题 17
**知识点：** GPT-2 as baseline
**题目：** GPT-2在实验中作为基准的作用是什么？
**选项：**
- A. 扩散模型变体
- B. 验证自回归模型性能
- C. 数据集预处理
- D. 计算噪声矩阵

---

**正确答案：** D
**原文依据：** 「To enhance the reliability and confidence of our experimental results, we implement an additional validation step by training a GPT-2 model under the same dataset and training conditions.」（出自：Discrete Diffusion Models for Language Generation.pdf，第35页）
**解析：** GPT-2作为强基线，用于交叉验证AR模型的表现，确保结果鲁棒性。


---


### 问题 18
**知识点：** Training loss
**题目：** 训练损失在模型评估中的意义是什么？
**选项：**
- A. 仅用于生成速度
- B. 反映模型收敛和学习效果
- C. 添加噪声水平
- D. 嵌入维度选择

---

**正确答案：** B
**原文依据：** 「The training loss graphs... The global step metric represents the cumulative number of optimization steps taken during training...」（出自：Discrete Diffusion Models for Language Generation.pdf，第39页）
**解析：** 损失曲线显示模型在训练过程中的进步，帮助诊断稳定性。


---


### 问题 19
**知识点：** Generation speed
**题目：** 生成速度在AR和D3PM比较中的差异是什么？
**选项：**
- A. AR更快
- B. D3PM支持并行，潜在更快
- C. 两者相同
- D. 取决于嵌入

---

**正确答案：** C
**原文依据：** 「In terms of inference speed, both AR and D3PM models demonstr... D3PM slightly outperforms the AR model in batch generation rate, which implies its potential advantage in parallel generation settings.」（出自：Discrete Diffusion Models for Language Generation.pdf，第46页）
**解析：** D3PM的并行去噪使它在生成速度上具有优势，尤其在推理阶段。


---


### 问题 20
**知识点：** High-Performance Computing (HPC)
**题目：** 高性能计算（HPC）在该研究中的必要性是什么？
**选项：**
- A. 处理大型模型训练的计算需求
- B. 添加噪声
- C. 分词过程
- D. 计算困惑度

---

**正确答案：** A
**原文依据：** 「For the successful execution of model training and evaluation, highperformance computing (HCPC) resources provided by the university were utilized. Due to the computational demands of large-scale language models, my personal workstation was insufficient...」（出自：Discrete Diffusion Models for Language Generation.pdf，第60页）
**解析：** HPC提供GPU和大量内存，支持训练124M参数模型，否则本地设备无法完成。


## 知识点总结
本材料覆盖了离散扩散模型在语言生成中的核心概念，包括自回归模型的顺序生成、D3PM的去噪过程、评估指标如BPT、PPL和NLL、数据集处理、架构基础以及实验比较。这些知识点帮助理解扩散模型相对于传统AR模型的优势与局限，促进对生成AI的深入掌握。

## 参考资料
- Discrete Diffusion Models for Language Generation.pdf
