# MHA · 多头注意力

[注意力索引](README.md) · [遗忘记录](../forgetting_log.md) · [下一篇：MQA](mqa.md)

学习状态：待记录。

## 核心思路

多个注意力头分别计算权重和加权结果，再拼接并经过输出投影。标准等维自注意力设 `H_q = H_kv = H`、`d = D / H`；要求 `D` 能被 `H` 整除。

$$
\mathrm{head}_i = \operatorname{Attention}(XW_i^Q, XW_i^K, XW_i^V)
$$

$$
\operatorname{MHA}(X) = \operatorname{Concat}(\mathrm{head}_1,\ldots,\mathrm{head}_H)W^O
$$

公式及模块接口参见 [PyTorch MultiheadAttention 文档](https://docs.pytorch.org/docs/stable/generated/torch.nn.MultiheadAttention.html)。

## PyTorch 形状路线

| 步骤 | 形状 | 操作思路 |
| --- | --- | --- |
| 输入 X | `[B, T, D]` | 明确 batch 在第一维 |
| Q / K / V 投影 | 各为 `[B, T, D]` | 三个 `nn.Linear(D, D)`；也可一次投影后拆分 |
| 拆头 | `[B, T, H, d]` → `[B, H, T, d]` | `reshape` 后交换序列与头维度 |
| 分数与权重 | `[B, H, T, T]` | 缩放点积、mask、softmax |
| 每头输出 | `[B, H, T, d]` | 权重乘 V |
| 合并头 | `[B, T, H, d]` → `[B, T, D]` | 先交换维度，再合并最后两维 |
| 输出投影 | `[B, T, D]` | `nn.Linear(D, D)` |

具体例子：`B=2, T=5, D=32, H=4`，则 `d=8`，分头后 Q 为 `[2,4,5,8]`，注意力分数为 `[2,4,5,5]`。

## 易错点

- `reshape` 负责拆合维度，`transpose` 负责交换维度；只改变形状不能替代交换维度。
- 合并多头前先恢复 `[B,T,H,d]` 的顺序；使用 `view` 时需关注张量是否连续。
- `nn.MultiheadAttention` 使用 `[B,T,D]` 输入时应核对 `batch_first=True`；其默认布局为 `[T,B,D]`。
- 该模块的布尔 `attn_mask` / `key_padding_mask` 中，`True` 表示屏蔽，与 SDPA 的布尔 `attn_mask` 相反。参见[模块文档](https://docs.pytorch.org/docs/stable/generated/torch.nn.MultiheadAttention.html)。

## 闭卷自测

- 为什么不能把 `[B,H,T,d]` 直接 reshape 成 `[B,T,D]`？
- 在自注意力中，Q/K/V 来自同一输入，为什么通常仍使用不同投影？
- 在 `D` 固定时增加头数，每头维度怎样变化？

## 我的推导 / 代码片段 / 复盘

待填写。需要重复复习的问题放入[遗忘表](../forgetting_log.md)。
