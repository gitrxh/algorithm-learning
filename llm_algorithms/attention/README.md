# 注意力机制

[返回 LLM 索引](../README.md) · [LLM 遗忘记录表](../forgetting_log.md)

建议按 [SDPA](sdpa.md) → [MHA](mha.md) → [MQA](mqa.md) → [GQA](gqa.md) 阅读。先理解单个头如何计算，再比较不同方法如何组织 Q/K/V。

## MHA、MQA、GQA 对照

以下以 `H_q = 8` 为例，每个头维度均为 `d`。

| 方法 | query 头数 | K/V 头数 | 共享方式 | 相对 MHA 的紧凑 KV Cache 容量 |
| --- | --- | --- | --- | --- |
| MHA | 8 | 8 | 每个 query 头有对应的 K/V 头 | 1 |
| GQA 示例 | 8 | 2 | 每 4 个 query 头共享一个 K/V 头 | 1/4 |
| MQA | 8 | 1 | 所有 query 头共享一个 K/V 头 | 1/8 |

比例只比较相同层数、batch、序列长度、每头维度及 dtype 下的 **K/V 缓存**，不代表总显存或运行速度同比下降。更一般地，GQA 的每组 query 头数为 `H_q / H_kv`。参见 [GQA 原论文](https://arxiv.org/abs/2305.13245)。

## 学完后应该能回答

- 从 `[B, T, D]` 到注意力分数 `[B, H_q, T, T]`，每一步如何变化？
- softmax 应沿哪个维度？因果 mask 屏蔽哪一侧？
- MHA、MQA、GQA 中，哪些投影矩阵的尺寸发生变化？
- GQA 减少了什么存储？为什么不应把演示用的 K/V 展开理解为高效缓存实现？
- `torch.nn.MultiheadAttention` 与 SDPA 的布尔 mask 语义有什么区别？

遗忘时，把具体问题记入[LLM 遗忘记录表](../forgetting_log.md)，并链接到相应笔记。
