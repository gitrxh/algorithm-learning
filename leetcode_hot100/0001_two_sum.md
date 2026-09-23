# 0001 · Two Sum（两数之和）

[题目网页](https://leetcode.cn/problems/two-sum/) · [题目索引](README.md) · [遗忘记录表](forgetting_log.md) · [下一题：Group Anagrams](0049_group_anagrams.md)

题型：Array、Hash Table。

## 第一次思路

用 `set` 保存之前出现过的数字。遍历到 `nums[i]` 时，查找 `target - nums[i]` 是否已出现；如果出现，说明找到了两个数。

## 第一版错误代码

保留最初的写法，便于回看当时的卡点：

```python
class Solution(object):

    def twoSum(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """

        s = set()
        s.add(nums[0])

        for i in range(1, len(nums)):

            if target - nums[i] in s:
                return s.list, i

            s.add(nums[i])

        return None
```

## 错误原因

1. `set` 能判断某个数字是否出现过，却没有保存它在 `nums` 中的下标。本题需要返回两个下标，因此要保存“数字 → 下标”。
2. `set` 没有 `.list` 属性。即使将集合转成列表，也不能得到元素在原数组中的正确下标。

## 正确思路：用字典保存数字和下标

设 `s` 为 `{已出现的数字: 其下标}`。遍历当前数字前，先检查补数 `target - nums[i]` 是否在字典中；若在，返回字典里的下标和 `i`；若不在，再存入当前数字。**先查再存**，可以避免同一个元素匹配自己。

## 最终代码

```python
class Solution(object):

    def twoSum(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """

        s = {}

        for i in range(len(nums)):

            if target - nums[i] in s:
                return [s[target - nums[i]], i]

            s[nums[i]] = i

        return None
```

## 示例理解

`nums = [2, 7, 11, 15]`、`target = 9`。扫描 `2` 时需要 `7`，字典为空，于是记录 `{2: 0}`；扫描 `7` 时需要 `2`，从字典拿到下标 `0`，返回 `[0, 1]`。

## 复杂度

- 时间：平均 `O(n)`；遍历 `n` 个元素，字典查询和插入平均为 `O(1)`。
- 额外空间：`O(n)`；最坏情况下存储此前的数字和下标。

## 本题收获

`set` 适合判断“出现过吗”；`dict` 能回答“这个值对应什么信息”。Two Sum 的键是数字，值是下标；[Group Anagrams](0049_group_anagrams.md) 使用同一种结构完成分组，但键和值不同。

复习时可闭卷回答：哈希表存什么？为何先查再存？需要复习的具体卡点记入[本部分遗忘表](forgetting_log.md)。
