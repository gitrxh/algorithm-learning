# 0128 · Longest Consecutive Sequence（最长连续序列）

[题目网页](https://leetcode.cn/problems/longest-consecutive-sequence/) · [题目索引](README.md) · [遗忘记录表](forgetting_log.md)

题型：Array、Hash Table。记录日期：2026-09-23。最终通过的环境：Python 3。

## 第一次尝试：集合加连续查找

先把 `nums` 转为集合，利用快速成员查找判断某个数的后继是否存在。最初写成 `s[i] + j`，触发 `TypeError: 'set' object does not support indexing`：集合不支持按下标取第 `i` 个元素。随后改为 `nums[i] + j`，可以运行，但从每个数组元素向后重复扫描，出现超时。

另一次修改中，`if nums[i] - 1 in s：` 使用了全角冒号 `：`，触发 `SyntaxError`；Python 语法需要半角 `:`。`for i in range(s)` 也会报错，因为 `range()` 的参数应为整数，而 `s` 是集合。要遍历集合里的数字，应直接写 `for i in s`。

## 减少重复扫描

只有当 `i - 1` **不在**集合中时，`i` 才是某段连续序列的起点。若前一个数字存在，跳过 `i`，避免从同一序列的中间重新计数。

外层还需遍历去重后的集合 `s`，而不是原数组 `nums`。若数组含多个相同起点，遍历 `nums` 会把这一整段重复计数多次。

## 最后一次超时：Python 2 的 `range`

在遍历集合且只从起点扫描后，核心思路已经符合线性时间。提交所选语言仍为 Python 2 时，内层 `range(1, len(s) + 1)` 每次都会创建包含 `len(s)` 个整数的列表；即使循环很快 `break`，这一步也已完成。起点很多时会反复创建大列表，造成超时。切换到 Python 3 后，`range` 按需提供整数，这版代码通过。

另一种写法是用 `while i + length in s` 逐步增加长度；本次保留实际通过的 `for` 写法。参见 [Python 2 `range` / `xrange` 文档](https://docs.python.org/2/library/functions.html#range)与 [Python 3 `range` 文档](https://docs.python.org/3/library/stdtypes.html#ranges)。

## 最终代码（Python 3）

以下按最后一次提供并通过的逻辑整理，规范了空格和 Markdown 中的下划线转义：

```python
class Solution(object):
    def longestConsecutive(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        s = set(nums)
        max_length = 0
        for i in s:
            if i - 1 in s:
                continue
            for j in range(1, len(s) + 1):
                if i + j not in s:
                    break
            if j > max_length:
                max_length = j
        return max_length
```

例如 `nums = [100, 4, 200, 1, 3, 2]`，只有 `1`、`100`、`200` 是各自序列的起点；从 `1` 向后能找到 `2、3、4`，最长长度为 `4`。空数组返回 `0`，重复数字在转成集合后只检查一次。

## 复杂度与收获

- Python 3 中平均时间复杂度为 `O(n)`：建集合 `O(n)`；每个不同数字最多作为一次起点判断，并在所属连续段中被向后检查一次。集合成员查询平均 `O(1)`。
- 额外空间复杂度为 `O(n)`：集合保存不同数字。
- “连续段只从起点扫描”解决重复查找；“遍历集合”解决重复起点；运行语言的 `range` 行为也会影响实际耗时。

需要复习的具体卡点和下次日期见[本部分遗忘表](forgetting_log.md)。
