# 共性错误

[返回笔记索引](README.md) · [笔记遗忘记录表](forgetting_log.md)

将真实发生的错误总结成下次能执行的检查动作。题目或算法专属错误仍记录在对应笔记，这里归纳跨主题反复出现的模式。

| 错误模式 | 触发场景 / 来源 | 原因 | 下次的检查动作 |
| --- | --- | --- | --- |
| 对 `set` 使用下标 | [0128 最长连续序列](../leetcode_hot100/0128_longest_consecutive_sequence.md) | 集合不支持 `s[i]`；`range(s)` 也不能遍历集合 | 要集合中的数字时直接 `for i in s`；要原数组下标时遍历数组 |
| 中文全角标点进入代码 | [0128 最长连续序列](../leetcode_hot100/0128_longest_consecutive_sequence.md) | `if ...：` 的冒号是全角字符，Python 需要 `:` | 报 `SyntaxError` 时检查报错位置的标点是否为半角 |
| 忽略运行语言的 `range` 行为 | [0128 最长连续序列](../leetcode_hot100/0128_longest_consecutive_sequence.md) | Python 2 的 `range` 先建列表，提前 `break` 也省不了这一步；Python 3 按需迭代 | 超时先核对提交语言，再检查循环范围是否被反复完整创建 |

以上记录来自今天的实际报错与超时过程；个人复习结果记在[LeetCode 遗忘表](../leetcode_hot100/forgetting_log.md)。

可从以下问题整理：是否漏了边界输入？是否混淆下标与值？是否没有先写出张量形状？是否把广播、维度交换和内存复制当成同一回事？

需要安排复习时，链接到所属部分已有的遗忘条目，或在[笔记遗忘表](forgetting_log.md)新增通用问题。
