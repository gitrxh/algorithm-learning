# algorithm-learning

个人算法学习仓库：**LeetCode Hot 100 网页版刷题 + 以 PyTorch 为主的 LLM 算法学习**。

在网页完成练习，在这里保存思路、关键代码片段、易错点和复习记录。所有内容均为 Markdown，可直接在 GitHub 浏览和编辑，无需配置本地运行环境。

## 学习入口

| 部分 | 内容 | 遗忘记录表 |
| --- | --- | --- |
| [LeetCode Hot 100](leetcode_hot100/README.md) | 官方题单、题目笔记、解题思路与复杂度 | [刷题遗忘记录](leetcode_hot100/forgetting_log.md) |
| [LLM 算法 · PyTorch](llm_algorithms/README.md) | MHA / MQA / GQA、位置编码、归一化、FFN、推理 | [LLM 遗忘记录](llm_algorithms/forgetting_log.md) |
| [模板](templates/README.md) | 题解模板、LLM 笔记模板、遗忘记录模板 | [模板遗忘记录](templates/forgetting_log.md) |
| [学习笔记](notes/README.md) | Python / PyTorch 细节、共性错误、复习方法 | [笔记遗忘记录](notes/forgetting_log.md) |

## 目录结构

```text
algorithm-learning/
├── README.md
├── leetcode_hot100/
│   ├── README.md                 # 分类导航与题目索引
│   ├── forgetting_log.md         # 本部分的遗忘与复习记录
│   └── 0001_two_sum.md           # 题号_英文题名，按需新增
├── llm_algorithms/
│   ├── README.md                 # 学习路线与主题索引
│   ├── forgetting_log.md
│   ├── pytorch_basics/           # 张量、维度变换、广播、自动求导
│   ├── attention/                # SDPA、MHA、MQA、GQA
│   ├── position_encoding/        # RoPE 等位置编码
│   ├── normalization/            # LayerNorm、RMSNorm
│   ├── feed_forward/             # FFN、SwiGLU、MoE
│   └── inference/                # KV Cache、采样、推理优化
├── templates/
│   ├── README.md
│   ├── forgetting_log.md
│   ├── leetcode_note.md
│   ├── llm_note.md
│   └── forgetting_log_template.md
└── notes/
    ├── README.md
    ├── forgetting_log.md
    ├── review_guide.md
    ├── python_tricks.md
    ├── pytorch_tricks.md
    └── common_errors.md
```

## 每次学习怎么记录

1. **练习**：LeetCode 在[官方 Hot 100 网页题单](https://leetcode.cn/studyplan/top-100-liked/)完成；LLM 按主题阅读、推导并整理 PyTorch 思路。
2. **写笔记**：复制[题目模板](templates/leetcode_note.md)或 [LLM 模板](templates/llm_note.md)，保留本次真正学到的内容；代码片段按需粘贴。
3. **记遗忘点**：想不起来、需要提示或再次出错时，在所属部分的 `forgetting_log.md` 中添加一行，写清具体卡点与纠正要点。
4. **安排复习**：先看“下次复习”日期，合上答案重做或口述，再追加结果。具体规则见[复习指南](notes/review_guide.md)。

## 维护约定

- 题目文件使用 `四位题号_英文题名.md`；LLM 文件使用 `主题/算法名.md`，例如 `attention/gqa.md`。
- 学习笔记按需新增，并在所属部分的 README 添加链接；主题导航里的“待记录”不代表已经学过。
- 每个部分只维护一张遗忘记录表；子主题通过表中的“对象 / 链接”区分，同一遗忘点不重复建档。
- 题目和算法笔记保存知识内容，遗忘表保存个人复习历史。初始化的空行是占位，不代表实际学习或遗忘记录。
- 学习状态统一为 `待记录 / 学习中 / 待复习 / 已掌握`；遗忘条目状态统一为 `待复习 / 巩固中 / 已掌握`。
