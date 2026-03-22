# 脚本
## 概述
使用一个 Python 脚本完成所有交互。

推荐在仓库根目录执行：
```bash
uv run python scripts/main.py [COMMAND]
```

## 命令
## 状态
```bash
uv run python scripts/main.py status
```

返回：
- 总卡片数
- 今日复习进度（已复习/今日总任务）
- 未来一周每天到期卡片数
- 平均检索率

### 生成卡片
```bash
uv run python scripts/main.py card new -q "question" -a "answer"
```

返回 `CARD_ID`。

```bash
uv run python scripts/main.py card override [CARD_ID] -q "question" -a "answer"
```

覆盖题干与答案，保持 `CARD_ID` 不变，并重置调度。

```bash
uv run python scripts/main.py card rm [CARD_ID]
```

软删除卡片，保留历史复习日志。

### 复习
```bash
uv run python scripts/main.py review get-question
```

返回 `CARD_ID` 和题干（QUESTION）。

```bash
uv run python scripts/main.py review get-answer
```

返回 `CARD_ID` 和标准答案（ANSWER）。

```bash
uv run python scripts/main.py review rate [RATING]
```

`RATING` 包括：`again`、`hard`、`good`、`easy`。

返回：
- 本次评分结果
- 历史正确性
- 剩余到期卡片进度
- 检索率变化
- 反思提示（增/删/改/题目质疑）

注：
- 必须遵循 `get-question -> get-answer -> rate`，否则会报错或重复上一题
- 只有在 `rate` 后才能调用下一次 `get-question`
- 状态缓存存储在 SQLite 的 `review_state` 表：当前卡片、是否看答案、是否已评分
- `CARD_ID` 是唯一递增整数

## 第三方包
- [FSRS API documentation](https://open-spaced-repetition.github.io/py-fsrs/fsrs.html)：FSRS Python API 文档
