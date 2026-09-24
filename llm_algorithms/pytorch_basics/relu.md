# ReLU：逐元素运算与梯度

[PyTorch 基础索引](README.md) · [LLM 遗忘记录表](../forgetting_log.md)

记录日期：2026-09-24。学习目标：实现对任意形状张量逐元素计算 `max(0, x)`，保持形状，支持梯度。计算过程由 PyTorch 张量操作完成，避免 Python 逐元素循环。

## 第一次写法与问题

最初写成 `max(0, x)`。这是 Python 的内置 `max`，比较的是两个参数，不会把 `x` 中每个元素分别与 0 比较；对于多元素张量，也可能因无法把布尔张量当作单个真假值而报错。

后来提出的 `x * (x > 0)` 则是逐元素运算：`x > 0` 得到布尔掩码，正数位置为 `True`；与 `x` 相乘后，正数保留，其余位置变为零。这一写法可以让浮点张量的梯度传回 `x`。

粘贴在聊天里的 `\*` 是 Markdown 转义，Python 代码里直接写 `*`；行末的中文逗号 `，` 也要删去。

## 两种手写表达

```python
import torch


def relu_mask(x: torch.Tensor) -> torch.Tensor:
    return x * (x > 0)


def relu_clamp(x: torch.Tensor) -> torch.Tensor:
    return torch.clamp(x, min=0)
```

`torch.clamp` 用于把张量的每个值限制在指定范围内：`min=0` 将负数截为零；例如同时设置 `min=0, max=1` 时，大于 1 的值也会截为 1。[PyTorch `clamp` 文档](https://docs.pytorch.org/docs/stable/generated/torch.clamp.html)

若只需要在模型中调用现成 ReLU，可以使用 `torch.nn.functional.relu(x)`，它也逐元素处理张量。[PyTorch `F.relu` 文档](https://docs.pytorch.org/docs/stable/generated/torch.nn.functional.relu.html)

## 前向结果相同，零点梯度有差别

三种写法对 `[-1, 0, 2]` 的前向结果都等价于 `[0, 0, 2]`。不过 ReLU 在 `x=0` 处数学上不可导，自动求导系统必须选一个约定。在本地 PyTorch 2.4.0 上对输出求和再反向传播，得到：

| 写法 | `x=-1` 的梯度 | `x=0` 的梯度 | `x=2` 的梯度 |
| --- | --- | --- | --- |
| `x * (x > 0)` | 0 | 0 | 1 |
| `torch.clamp(x, min=0)` | 0 | 1 | 1 |
| `torch.nn.functional.relu(x)` | 0 | 0 | 1 |

因此你的掩码写法在零点的梯度约定与 `F.relu` 一致；`clamp` 有相同的前向值，也能传梯度，但零点梯度不同。做大张量计算时，`F.relu` 是直接调用现成算子的方式；掩码表达式会产生额外的布尔掩码与乘法。这里没有做性能基准比较。

## 闭卷自测

- 为什么 `max(0, x)` 不能表示张量的逐元素 ReLU？
- `torch.clamp(x, min=0, max=1)` 对 `[-2, 0.5, 3]` 分别输出什么？
- 当输入恰好为 0，掩码写法与 `clamp(min=0)` 在本次验证中的梯度分别是多少？

复习结果和下次日期记录在[LLM 遗忘表](../forgetting_log.md)。
