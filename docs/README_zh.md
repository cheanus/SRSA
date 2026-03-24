# SRSA: 智能体的间隔复习系统 (The Spaced Repetition Systems for Agents)

**教会你的 AI 智能体去记忆——并随时间不断改进其记忆。**

SRSA (Spaced Repetition Systems for AI Agents) 是一个**记忆自我改进层**，它将智能体的记忆转换为可复习的卡片，并利用间隔复习技术不断完善它们。

与仅存储和检索信息的传统记忆系统不同，SRSA 使智能体能够：

- 面向问题的记忆增强
- 检测缺失的知识
- 纠正错误的记忆
- 移除误导性的记忆

## 为什么需要 SRSA

大多数 AI 智能体记忆系统都会随着时间的推移出现**记忆漂移**。

常见问题包括：

- 检索不匹配：记忆存在但无法在正确的上下文中被激活。
- 记忆干扰：冲突的记忆降低了召回准确性。
- 记忆退化：记忆变得过时、不完整或不准确。

这些问题类似于认知科学中的发现，如**遗忘曲线**和检索失败。

目前的方法主要集中在：

**预处理方法**
设计更好的记忆结构。

**后处理方法**
总结或重组织记忆。

然而，它们并没有在智能体运行期间**直接有效地提高记忆召回的可靠性**。

SRSA 引入了**间隔检索 + 记忆自我纠正**来解决这个问题。

## 核心理念

SRSA 创建了一个持续的记忆改进过程：

```
卡片生成 → 定期复习 → 自我评估 → 记忆更新
```

在复习过程中，智能体会：

1. 回忆知识
2. 与标准答案对比
3. 评估正确性
4. 反思错误
5. 修改记忆

这使得记忆能够持续进化。

目标：

> 下次：回答更准确、更快速。

## 架构

SRSA 作为任何智能体记忆系统之上的一个层运行。

```
Agent (智能体)
  ↓
Memory System (记忆系统)
  ↓
SRSA Skill (SRSA 技能)
  - Card Generator (卡片生成器)
  - FSRS Scheduler (FSRS 调度器)
  - Review Loop (复习循环)
  - Memory Self-Correction (记忆自我纠正)
  ↓
Improved Memory (改进后的记忆)
```

关键设计原则：

- **记忆是动态的：**
智能体的知识应该不断进化。

- **召回才是真正的瓶颈：**
单纯的存储并不能解决记忆问题。

- **复习促进学习：**
周期性的检索会加强记忆路径。

## 特性

- 🎬 **基于场景的召回改进**：自定义卡片帮助解决检索失败。
- 🔓 **与记忆系统解耦**：适用于**任何**智能体记忆架构。
- 🚀 **高效调度**：使用 **FSRS 算法**最大限度地减少不必要的复习和 Token 使用。
- 🔧 **记忆自我修复**：智能体在召回失败后更新记忆。
- 📈 **复习分析**：跟踪召回表现和记忆随时间的演变。

## 快速开始

使用**技能 (Skills)** 可以轻松地将 SRSA 集成到你的智能体工作流中。

将仓库克隆到 `skills/` 目录下：

```bash
git clone https://github.com/cheanus/SRSA
cd SRSA
```

添加一张新卡片（或者直接要求智能体从其记忆中生成卡片）：

```bash
uv run python scripts/main.py card new \
-q "法国的首都是哪里？" \
-a "巴黎"
```

最后，要求智能体读取技能并每天或按需运行复习。

## 项目结构

```
scripts/
    main.py           # 主程序
    config.yaml       # 配置文件
    cache/
        fsrs.sqlite   # 数据库
doc/
    script.md         # 脚本使用文档
SKILL.md              # 技能定义
README.md             # 英文文档
```

## 配置

可选配置：`scripts/config.yaml`

你可以参考 [FSRS 文档](https://open-spaced-repetition.github.io/py-fsrs/fsrs.html) 了解详情。

## 使用方法

请参阅 [script.md](docs/script.md) 了解命令用法。

## 注意事项

如果每日复习量变大，建议的解决方案是：

- 压缩对话上下文
- 将复习任务分配给多个智能体
- 每天运行多次复习环节

## 复习示例

```
问：用户喜欢哪类新闻？

智能体回答：
技术和科学新闻。

正确答案：
技术、科学和国际新闻。

反思：
相关记忆不完整，需要更新以包含国际新闻。

行动：
更新记忆，加入关于国际新闻的信息。
```

## 致谢

- [Free Spaced Repetition Scheduling Algorithm](https://github.com/open-spaced-repetition/free-spaced-repetition-scheduler)
- [FSRS python package](https://open-spaced-repetition.github.io/py-fsrs/fsrs.html)
- [Spaced Repetition Research](https://supermemo.guru/wiki/Spaced_repetition)
- [Memory-R1](https://arxiv.org/abs/2508.19828)
- [Linux Do](https://linux.do/)

## 授权证

MIT License
