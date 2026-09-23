# PyTorch 细节

[返回笔记索引](README.md) · [笔记遗忘记录表](forgetting_log.md) · [PyTorch 学习提纲](../llm_algorithms/pytorch_basics/README.md)

这里积累跨算法通用的操作经验；特定算法的 shape 推导保留在算法笔记中。

## 待整理方向

- 维度：`shape`、`dim`、负数维度索引。
- 布局：`reshape`、`view`、`transpose`、`permute`、`contiguous`。
- 广播与扩展：`unsqueeze`、`expand`、`repeat`、`repeat_interleave`。
- 注意力计算：`matmul`、`einsum`、`softmax`、`masked_fill`。
- 梯度与执行模式：`detach`、`no_grad`、`eval` 各自控制什么。

## 我的记录

| 操作 | 输入 → 输出形状 / 最小例子 | 易错点 | 来源算法或文档 |
| --- | --- | --- | --- |
| — | — | — | — |

空行仅作占位。API 行为涉及版本差异时，补充对应官方文档和版本；需要复习的通用细节记入[笔记遗忘表](forgetting_log.md)。
