# Spaced Repetition Systems for AI Agents (SRSA)

## 功能
把 agent memory 转换为可复习的 SRS 卡片，并通过复习驱动 memory 自我修正。

## 文件
- scripts/
	- main.py
	- config.yaml
	- 其他python文件
	- cache/
		- fsrs.sqlite
        - ……
- doc/
    - script.md: 介绍脚本的使用方法
- SKILL.md: 为Agent准备的SKILL文档
- README.md

## 特点
解决痛点：
- 任何记忆系统，随着时间的推移，在后续操作、思考的影响下，原有的记忆将很难在对应情景下正确激活，即遗忘曲线
- Agent遗忘的直接原因有：记忆与情景不匹配，无法召回；干扰记忆影响正确记忆的激活；记忆不准确或不充足
- 当前解决Agent遗忘的方法有：
	- 前训练，即直接设计记忆系统，但并非直接解决记忆召回问题
	- 后训练，即面向情景整理已有记忆，但存在效率低下的问题

SRSA特点：
- 与任意agent记忆系统解耦，可尝试在不同记忆系统上提升效果
- 可自定义添加情景卡片，直接解决记忆召回问题
- FSRS算法让每日复习足够高效，可减少不必要的token消耗

## 前提
- 一个拥有记忆系统的Agent

## 使用方法
配置 `scripts/config.yaml`（可选）：
	- `desired_retention`：期望记忆保留率（0-1）
	- `database_path`：SQLite 路径（默认 `scripts/cache/fsrs.sqlite`）

## 检查状态
输出卡片总数、今日复习进度、未来一周每天到期卡片数和平均检索率。
```bash
uv run python scripts/main.py status
```

### 生成卡片
手动触发，增量更新卡片。
问题类型统一为开放式问答。

```bash
uv run python scripts/main.py card new -q "question" -a "answer"
uv run python scripts/main.py card override [CARD_ID] -q "question" -a "answer"
uv run python scripts/main.py card rm [CARD_ID]
```

### 复习
为agent制定定时复习任务，阅读skill以开始复习。
主要流程是：`AI运行脚本 - 输出第一问 - AI查看答案 - AI自评 - AI反思修改记忆 - AI请求下一问或结束`。

```bash
uv run python scripts/main.py review get-question
uv run python scripts/main.py review get-answer
uv run python scripts/main.py review rate [again|hard|good|easy]
```

三段输出约定：
- `review get-question`：输出 `CARD_ID` + 题干（QUESTION）
- `review get-answer`：输出 `CARD_ID` + 标准答案（ANSWER）
- `review rate`：输出评分结果、自评信息（历史正确性、剩余卡片进度、检索率变化）

一般来说，AI经过反思答题流程，结合历史正确性，之后的记忆修改操作有：
- 增：因缺失了某信息才答错；添加某信息可加快答题速度
- 删：因被某记忆误导了才答错；删除某记忆可加快答题速度
- 改：因某记忆不准确才答错；修改某记忆可加快答题速度

为了让下一次题能**答得对、答得快**，上述操作可相互组合。

此外还可质疑题目：
- 改：题干信息不足或模糊；所给答案不准确
- 删：该题无法在任何情况下回答

## 注意
- 状态约束：必须先 `get-question` -> `get-answer` -> `rate`，否则会报错或重复上一题
- `card override` 会重置该卡调度（CARD_ID 不变），`card rm` 为软删除（保留复习日志）
- 若每天复习卡片较多，使用时请注意会话上下文可能过大的问题，限制上下文的办法有
	- 压缩会话上下文
	- 拆分为多个子agent的复习
	- 拆分为一天的多次复习

## 参考
- [FSRS API documentation](https://open-spaced-repetition.github.io/py-fsrs/fsrs.html)：FSRS Python API 文档
