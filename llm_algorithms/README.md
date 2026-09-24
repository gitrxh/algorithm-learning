# LLM 算法 · PyTorch

[返回首页](../README.md) · [遗忘记录表](forgetting_log.md) · [LLM 笔记模板](../templates/llm_note.md)

这一部分学习大语言模型中的算法与组件，以 **PyTorch 的张量操作和实现思路**为主。每个主题依次整理：解决的问题 → 公式 → 张量形状 → 核心操作 → 易错点 → 闭卷自测。需要时在 Markdown 内补充代码片段。

## 学习路线

| 顺序 | 模块 | 主题 | 学习目标 |
| --- | --- | --- | --- |
| 1 | [PyTorch 基础](pytorch_basics/README.md) | shape、广播、reshape、transpose、autograd | 能追踪张量维度及梯度路径 |
| 2 | [注意力机制](attention/README.md) | SDPA → MHA → MQA → GQA | 能说明 Q/K/V 的形状和共享关系 |
| 3 | [位置编码](position_encoding/README.md) | 绝对位置编码、RoPE | 能说明位置如何进入注意力计算 |
| 4 | [归一化](normalization/README.md) | LayerNorm、RMSNorm、Pre-Norm | 能写出统计维度及残差连接顺序 |
| 5 | [前馈网络](feed_forward/README.md) | FFN、SwiGLU、MoE | 能说明维度扩张、门控与路由 |
| 6 | [推理机制](inference/README.md) | KV Cache、采样、FlashAttention | 能区分缓存、采样策略与注意力计算优化 |

上述顺序是本仓库的学习安排；模块页面提供待完成清单，具体个人进度在学习后填写。

## 首批主题索引

| 主题 | 笔记入口 | 重点 | 学习状态 |
| --- | --- | --- | --- |
| ReLU | [PyTorch 基础：ReLU](pytorch_basics/relu.md) | 逐元素运算、`clamp` 与梯度 | 已记录学习过程 |
| Scaled Dot-Product Attention | [SDPA](attention/sdpa.md) | 缩放、softmax 维度、mask | 待记录 |
| Multi-Head Attention | [MHA](attention/mha.md) | 多头拆分与合并、输出投影 | 待记录 |
| Multi-Query Attention | [MQA](attention/mqa.md) | 所有 query 头共享一组 K/V | 待记录 |
| Grouped-Query Attention | [GQA](attention/gqa.md) | query 头分组共享 K/V | 待记录 |

## 统一符号

| 符号 | 含义 |
| --- | --- |
| `B` | batch size |
| `T` | 无缓存自注意力中的序列长度 |
| `D` | hidden size / model dimension |
| `H_q` | query 头数 |
| `H_kv` | key/value 头数，K 与 V 头数相同 |
| `d` | 每个头的维度；本文示例约定 `D = H_q × d` |
| `T_q / T_kv` | query 长度 / key-value 长度，推理时可以不同 |

本仓库注意力笔记以输入 `[B, T, D]`、分头后 `[B, H, T, d]` 为默认约定；阅读其他实现时先核对维度顺序。

## 新增主题

复制 [LLM 模板](../templates/llm_note.md)到对应模块，补充链接和学习状态。所有模块的遗忘点集中维护在[本部分遗忘表](forgetting_log.md)，例如“GQA 的 K/V 投影输出维度为什么不是 D？”；复习历史不分散到各篇笔记。
