# PyTorch 基础

[返回 LLM 索引](../README.md) · [LLM 遗忘记录表](../forgetting_log.md) · [笔记模板](../../templates/llm_note.md)

读懂 LLM 实现中的张量操作，并能逐步写出每一步的 shape。

## 待完成主题

以下是学习提纲，尚未填入个人学习结果。学到某一项时，再用模板在本目录新增笔记，并将主题名改为笔记链接。

| 主题 | 需要回答的问题 | 学习状态 |
| --- | --- | --- |
| 张量形状与广播 | 区分 batch、序列、头、通道维度；解释广播发生在哪些维度。 | 待记录 |
| reshape / view / transpose / permute | 区分改变形状和交换维度；理解 stride 与连续性。 | 待记录 |
| matmul / einsum | 用下标说明哪些维度被收缩、哪些维度被广播。 | 待记录 |
| mask / softmax | 核对布尔语义、mask 的广播形状和 softmax 归一化方向。 | 待记录 |
| autograd / dtype / device | 解释梯度路径、detach，以及张量的精度和设备一致性。 | 待记录 |

## 阶段自测

能否不运行代码，推导 MHA 中从输入到输出的全部形状？

## 记录方式

一篇笔记对应一个主题。推导、关键 PyTorch 操作和自己的解释写进笔记；遗忘点、复习结果及下次日期统一记入 [LLM 遗忘记录表](../forgetting_log.md)。
