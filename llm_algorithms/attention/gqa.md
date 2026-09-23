# GQA · 分组查询注意力

[注意力索引](README.md) · [遗忘记录](../forgetting_log.md)

学习状态：待记录。

## 核心思路

将 query 头分组，每组共享一个 key 头和一个 value 头。设 query 头数为 `H_q`，K/V 头数为 `H_kv`，等大小分组时要求 `H_q % H_kv == 0`，每组有 `H_q / H_kv` 个 query 头。

当 `H_kv = H_q` 时对应 MHA；当 `H_kv = 1` 时对应 MQA；介于两者之间是常用的 GQA 配置。参见 [GQA 原论文](https://arxiv.org/abs/2305.13245)。

## PyTorch 形状路线

本页约定 `D = H_q × d`。以 `D=32, H_q=8, H_kv=2` 为例，`d=4`，每个 KV 头服务 4 个 query 头。

| 步骤 | 通用形状 / 操作 | 示例 |
| --- | --- | --- |
| Q 投影 | `nn.Linear(D, H_q * d)` | 输出维度 32 |
| K / V 投影 | 各为 `nn.Linear(D, H_kv * d)` | 输出维度各为 8 |
| Q 分头 | `[B, H_q, T, d]` | `[B, 8, T, 4]` |
| K / V 分头 | 各为 `[B, H_kv, T, d]` | `[B, 2, T, 4]` |
| 按组匹配 K/V | 逻辑上每个 KV 头对应一组 query 头 | KV0 → Q0–Q3；KV1 → Q4–Q7 |
| 每头输出 | `[B, H_q, T, d]` | `[B, 8, T, 4]` |
| 合头与输出投影 | `[B, T, D]` | `[B, T, 32]` |

## 实现思路与容易忘的点

- 为了读懂分组，可用 `repeat_interleave(H_q // H_kv, dim=1)` 理解 K/V 头的对应关系。直接使用 `repeat` 的排列顺序不同，要核对组内匹配。
- K/V 的每头维度仍为 `d = D / H_q`，不能误写成 `D / H_kv`。
- 输出保留 `H_q` 个头，合头后维度为 `H_q × d = D`。
- PyTorch SDPA 提供 `enable_gqa` 入口，具体版本与后端支持以[官方文档](https://docs.pytorch.org/docs/stable/generated/torch.nn.functional.scaled_dot_product_attention.html)为准；阅读实现时核对头数约束与布局。

## KV Cache 怎么估算

在相同的层数、`B、T、d、dtype` 下，紧凑缓存只保存 `H_kv` 组 K/V。单层元素数为：

$$
2 \times B \times H_{kv} \times T \times d
$$

相对 MHA 的缓存容量比例是 `H_kv / H_q`。上面的 8 个 Q 头、2 个 KV 头对应 1/4。若演示实现把 K/V 显式复制到 `H_q` 个头，会增加临时张量开销，因此形状演示不能直接作为高效推理实现或速度结论。

## 闭卷自测

- `D=1024, H_q=16, H_kv=4` 时，`d`、K/V 投影输出维度和每组 query 数各是多少？
- 为什么 GQA 共享的是 K/V，而不是把 Q 头数也缩减为 `H_kv`？
- GQA 是否让注意力分数的 query 头维度从 `H_q` 变成了 `H_kv`？
- 缓存元素数少了，是否意味着整个模型显存和运行时间同比下降？

## 我的推导 / 代码片段 / 复盘

待填写。需要重复复习的问题放入[遗忘表](../forgetting_log.md)。
