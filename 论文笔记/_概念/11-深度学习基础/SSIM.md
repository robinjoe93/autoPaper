---
type: concept
aliases: [SSIM, Structural Similarity Index]
---

# SSIM

## 定义

通过考虑结构信息变化来评估图像相似度的指标，广泛用于图像质量评估。

## 数学形式

$$
SSIM(x,y) = \frac{(2\mu_x\mu_y + c_1)(2\sigma_{xy} + c_2)}{(\mu_x^2 + \mu_y^2 + c_1)(\sigma_x^2 + \sigma_y^2 + c_2)}
$$

## 核心要点

1. 考虑亮度、对比度、结构三个维度
2. 内容不变，不适合比较图像集合
3. SSIM论文中未被采用

## 局限性

- 内容不变性限制其在数据集比较中的应用
- 不适合合成数据质量评估

## 代表工作

- Wang et al. (2004): 原始提出
- [[SDQM]]: 作为未采用方法的讨论

## 相关概念

- [[FID]]
- [[Mauve]]
- [[LPIPS]]