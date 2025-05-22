# 分词器的数学原理与算法推导

## 1. 信息论基础

### 1.1 熵与信息量

在分词算法中,我们经常使用信息熵来评估分词的质量。对于一个词表 $V$,其信息熵定义为:

$$H(V) = -\sum_{i=1}^{|V|} p(w_i) \log p(w_i)$$

其中:
- $p(w_i)$ 是词 $w_i$ 的出现概率
- $|V|$ 是词表大小

### 1.2 条件熵

对于上下文相关的分词,我们考虑条件熵:

$$H(Y|X) = -\sum_{x \in X} p(x) \sum_{y \in Y} p(y|x) \log p(y|x)$$

这帮助我们评估分词的上下文依赖性。

## 2. 统计分词算法

### 2.1 最大匹配算法

#### 2.1.1 前向最大匹配(FMM)
设定最大词长 $L$,对于文本 $T$,算法复杂度为:

$$O(|T| \cdot \min(L, |T|))$$

形式化定义:
```
FMM(T, Dict):
    n = len(T)
    i = 0
    while i < n:
        matched = False
        for j in range(min(L, n-i), 0, -1):
            if T[i:i+j] in Dict:
                output(T[i:i+j])
                i += j
                matched = True
                break
        if not matched:
            output(T[i])
            i += 1
```

### 2.2 基于统计的分词模型

#### 2.2.1 N-gram模型
N-gram模型的概率计算:

$$P(w_i|w_{i-1}) = \frac{count(w_{i-1}, w_i)}{count(w_{i-1})}$$

对于tri-gram:

$$P(w_i|w_{i-2},w_{i-1}) = \frac{count(w_{i-2}, w_{i-1}, w_i)}{count(w_{i-2}, w_{i-1})}$$

## 3. 子词分词算法

### 3.1 BPE(Byte Pair Encoding)

#### 3.1.1 算法形式化描述

初始状态:
- 词表 $V = \{字符集\}$
- 训练语料 $C$

迭代过程:
1. 统计相邻字符对频率:
   $$freq(x,y) = \sum_{w \in C} count((x,y), w)$$

2. 选择最高频字符对 $(x^*, y^*)$:
   $$(x^*, y^*) = \arg\max_{(x,y)} freq(x,y)$$

3. 合并字符对,更新词表:
   $$V = V \cup \{xy\}$$

#### 3.1.2 复杂度分析

时间复杂度:
- 统计频率: $O(|C|)$
- 合并操作: $O(|V|)$
- 总体: $O(n|C| + n|V|)$,其中 n 是迭代次数

### 3.2 Unigram Language Model

#### 3.2.1 概率模型

给定文本序列 $X$,分词序列 $S$,概率模型定义为:

$$P(X) = \sum_S P(S) = \sum_S \prod_{w \in S} p(w)$$

其中 $p(w)$ 是子词 $w$ 的概率。

#### 3.2.2 EM算法优化

E步:计算每个分词序列的后验概率:

$$P(S|X) = \frac{P(S)}{\sum_{S'} P(S')}$$

M步:更新子词概率:

$$p(w) = \frac{\sum_{X} c(w,X)P(S|X)}{\sum_{w'} \sum_{X} c(w',X)P(S|X)}$$

其中 $c(w,X)$ 是子词 $w$ 在序列 $X$ 中的出现次数。

### 3.3 WordPiece

#### 3.3.1 打分函数

对于候选合并对 $(x,y)$,其得分计算为:

$$score(x,y) = \frac{freq(xy)}{freq(x) \cdot freq(y)}$$

这反映了两个子词组合的互信息。

## 4. 深度学习中的分词

### 4.1 Subword Regularization

使用多项分布采样分词序列:

$$P(S|X) = \frac{\exp(-\frac{1}{\tau} \sum_{w \in S} \log p(w))}{\sum_{S'} \exp(-\frac{1}{\tau} \sum_{w \in S'} \log p(w))}$$

其中 $\tau$ 是温度参数,控制分布的平滑程度。

### 4.2 动态编程算法

#### 4.2.1 Viterbi算法

前向概率 $\alpha$ 计算:

$$\alpha_t(j) = \max_{i} \{\alpha_{t-1}(i) + \log p(w_t|i,j)\}$$

后向回溯得到最优路径。

## 5. 评估指标的数学定义

### 5.1 覆盖率

词表覆盖率定义:

$$Coverage = \frac{|Tokens_{covered}|}{|Tokens_{total}|} \times 100\%$$

### 5.2 分词准确率

精确率:
$$P = \frac{|Correct_{tokens}|}{|Predicted_{tokens}|}$$

召回率:
$$R = \frac{|Correct_{tokens}|}{|Reference_{tokens}|}$$

F1分数:
$$F1 = \frac{2PR}{P+R}$$

### 5.3 压缩率

$$Compression = \frac{|Tokens|}{|Characters|}$$

## 6. 优化算法

### 6.1 词表优化

使用最小描述长度(MDL)准则:

$$MDL(V) = L(V) + L(D|V)$$

其中:
- $L(V)$ 是词表的编码长度
- $L(D|V)$ 是使用词表 $V$ 编码数据 $D$ 的长度

### 6.2 动态规划分词

给定句子 $s$ 和词表 $V$,最优分词序列的递推公式:

$$dp[i] = \min_{j<i} \{dp[j] + cost(s[j:i])\}$$

其中 $cost$ 函数可以是:
- 词频对数
- 子词长度
- 语言模型概率

## 参考文献

1. Sennrich, R., Haddow, B., & Birch, A. (2016). Neural Machine Translation of Rare Words with Subword Units
2. Kudo, T. (2018). Subword Regularization: Improving Neural Network Translation Models with Multiple Subword Candidates
3. Schuster, M., & Nakajima, K. (2012). Japanese and Korean Voice Search