# 推理机制

[返回 LLM 索引](../README.md) · [LLM 遗忘记录表](../forgetting_log.md) · [笔记模板](../../templates/llm_note.md)

把注意力机制连接到自回归生成流程，区分缓存、采样和计算优化。

## 待完成主题

以下是学习提纲，尚未填入个人学习结果。学到某一项时，再用模板在本目录新增笔记，并将主题名改为笔记链接。

| 主题 | 需要回答的问题 | 学习状态 |
| --- | --- | --- |
| KV Cache | prefill 与 decode 分别处理什么？缓存沿哪一维追加？ | 待记录 |
| 增量解码与因果 mask | 当前 query 的绝对位置是什么？允许访问哪些 key？ | 待记录 |
| Temperature / Top-k / Top-p | 如何从 logits 得到候选集合与最终 token？ | 待记录 |
| FlashAttention | 算法如何安排读写与分块？与 MHA/GQA 的关系是什么？ | 待记录 |

## 阶段自测

能否以已有 10 个 token、再生成 1 个 token 为例，写出 Q/K/V 与缓存的形状？

## 记录方式

一篇笔记对应一个主题。推导、关键 PyTorch 操作和自己的解释写进笔记；遗忘点、复习结果及下次日期统一记入 [LLM 遗忘记录表](../forgetting_log.md)。
