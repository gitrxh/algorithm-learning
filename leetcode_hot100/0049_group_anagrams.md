# 0049 · Group Anagrams（字母异位词分组）

[题目网页](https://leetcode.cn/problems/group-anagrams/) · [题目索引](README.md) · [遗忘记录表](forgetting_log.md) · [上一题：Two Sum](0001_two_sum.md)

题型：Hash Table、String、Sorting。

## 第一次思路

使用字典分组：把字符串排序，排序结果相同的放到同一组。例如 `eat`、`tea`、`ate` 都对应 `aet`，因此想建立“排序结果 → 原字符串列表”的映射。

## 第一版错误代码

保留最初的写法；最后一行还存在 Python 语法错误，不能直接运行：

```python
class Solution(object):

    def groupAnagrams(self, strs):
        """
        :type strs: List[str]
        :rtype: List[List[str]]
        """

        s = {}

        for i in range(len(strs)):

            if strs[i].sort() in s:
                s[strs[i].sort()].add(strs[i])

            else:
                s[strs[i].sort()] = [strs[i]]

        return [for i in range(len(s)): s]
```

## 错误原因

1. `str` 没有 `.sort()` 方法；`sorted(strs[i])` 可以得到排序后的字符列表。
2. `sorted()` 返回 `list`，例如 `['a', 'e', 't']`。列表不能用作字典键，需要 `''.join(sorted(strs[i]))` 得到字符串 `"aet"`。
3. `s[key] = [strs[i]]` 创建的是列表。给这一组再加入字符串应使用 `.append()`；`.add()` 用于集合。
4. `return [for i in range(len(s)): s]` 不是合法的 Python 语法。字典已经保存了所有分组，直接用 `list(s.values())` 返回其值列表。

## 正确思路：排序后的字符串作键

遍历每个原字符串，先计算 `key = ''.join(sorted(strs[i]))`。同组字符串得到同一个键，把原字符串加入键对应的列表，最后返回所有列表。

## 最终代码

```python
class Solution(object):

    def groupAnagrams(self, strs):
        """
        :type strs: List[str]
        :rtype: List[List[str]]
        """

        s = {}

        for i in range(len(strs)):

            key = ''.join(sorted(strs[i]))

            if key in s:
                s[key].append(strs[i])

            else:
                s[key] = [strs[i]]

        return list(s.values())
```

## 示例理解

输入 `['eat', 'tea', 'tan', 'ate', 'nat', 'bat']` 时，排序键依次是 `aet、aet、ant、aet、ant、abt`，最终得到：

```python
{
    'aet': ['eat', 'tea', 'ate'],
    'ant': ['tan', 'nat'],
    'abt': ['bat'],
}
```

返回这些字典值组成的列表即可。

## 复杂度

设有 `n` 个字符串、最长长度为 `k`。

- 时间：`O(n · k log k)`；每个字符串需要排序，再构造键。
- 额外空间：`O(n · k)`；分组和排序键占用空间，单次排序还需要至多 `O(k)` 临时空间。

## 本题收获

哈希表分组的关键是找到**同组相同、异组不同**的稳定键。这道题的键是排序后的字符串，值是原字符串列表；[Two Sum](0001_two_sum.md) 的键是数字，值是下标。两道题都要先想清楚“键是什么，值要保存什么”。

复习时可闭卷回答：`sorted()` 返回什么类型？为什么要 `join`？列表加元素与字典取所有值分别用什么？需要复习的具体卡点记入[本部分遗忘表](forgetting_log.md)。
