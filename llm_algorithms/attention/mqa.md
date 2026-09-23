# MQA · 多查询注意力

[注意力索引](README.md) · [遗忘记录](../forgetting_log.md) · [下一篇：GQA](gqa.md)

学习状态：待记录。

## 与 MHA 的区别

保留 `H_q` 个 query 头，但只保留一个 key 头和一个 value 头，供所有 query 头共享。可以把它看作 [GQA](gqa.md) 在 `H_kv=1` 时的特例。MQA 的定位和与 GQA 的关系可参考 [GQA 论文](https://arxiv.org/abs/2305.13245)。

## PyTorch 形状路线

本页约定 `D = H_q × d`。

| 张量 / 步骤 | 形状或投影 |
| --- | --- |
| Q 投影 | `nn.Linear(D, H_q * d)` |
| K / V 投影 | 各为 `nn.Linear(D, d)` |
| Q 分头 | `[B, H_q, T, d]` |
| K / V 分头 | 各为 `[B, 1, T, d]` |
| 每个 query 头的输出 | `[B, H_q, T, d]` |
| 合头并输出投影 | `[B, T, D]` |

学习时可用广播理解单个 K/V 头如何服务多个 query 头；共享 K/V 不意味着各头得到相同的权重，因为 query 仍可不同。

## 闭卷自测

- MQA 中 K/V 投影输出维度为什么是 `d`，不是 `D`？
- 相同 `B、T、d、dtype` 下，单层 K/V 缓存元素数为何从 `2 × B × H_q × T × d` 变为 `2 × B × T × d`？
- K/V 相同，Q 不同，注意力权重会完全相同吗？

## 我的推导 / 代码片段 / 复盘

待填写。需要重复复习的问题放入[遗忘表](../forgetting_log.md)。
