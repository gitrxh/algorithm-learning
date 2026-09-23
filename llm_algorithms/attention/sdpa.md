# SDPA · 缩放点积注意力

[注意力索引](README.md) · [遗忘记录](../forgetting_log.md) · [下一篇：MHA](mha.md)

学习状态：待记录。下面是学习提纲，个人推导与复盘在文末补充。

## 核心公式

$$
\operatorname{Attention}(Q,K,V)
= \operatorname{softmax}\left(\frac{QK^\top}{\sqrt{d}} + M\right)V
$$

`M` 为加性 mask：允许的位置取 0，屏蔽的位置取负无穷。softmax 沿 key 的序列维度进行，使每个 query 对可见 key 的权重归一化。

## 形状推导

先看头数相同、每头 Q/K/V 维度均为 `d` 的情形：

| 步骤 | 张量形状 | PyTorch 思路 |
| --- | --- | --- |
| Q | `[B, H, T_q, d]` | 由输入投影并拆头 |
| K / V | `[B, H, T_kv, d]` | 由输入投影并拆头 |
| 分数 | `[B, H, T_q, T_kv]` | `Q @ K.transpose(-2, -1)`，再除以 `sqrt(d)` |
| 权重 | `[B, H, T_q, T_kv]` | 加 mask 后 `softmax(dim=-1)` |
| 输出 | `[B, H, T_q, d]` | 权重乘 V |

## PyTorch 阅读要点

- 对照接口：`torch.nn.functional.scaled_dot_product_attention`。
- SDPA 的布尔 `attn_mask` 中，`True` 表示允许参与注意力；不要直接套用 `nn.MultiheadAttention` 的屏蔽语义。
- 在评估阶段使用 SDPA 时，需要传 `dropout_p=0.0` 才关闭其 dropout。
- 无缓存、等长自注意力的因果 mask 为下三角可见；有 KV Cache 且 `T_q != T_kv` 时，应按 query 的实际位置核对可见范围。
- 手写 softmax 时若整行都被置为负无穷，会产生未定义的归一化；先检查该 query 是否至少有一个有效 key。

接口与 mask 约定见 [PyTorch SDPA 文档](https://docs.pytorch.org/docs/stable/generated/torch.nn.functional.scaled_dot_product_attention.html)。

## 闭卷自测

- 为什么缩放因子使用每头维度 `d`，而不是模型维度 `D`？
- 若 `T_q = 1`、`T_kv = 10` 且 query 是最新 token，它应该看到哪些 key？
- softmax 沿 query 维度计算，会改变什么含义？

## 我的推导 / 代码片段 / 复盘

待填写。需要重复复习的问题放入[遗忘表](../forgetting_log.md)。
