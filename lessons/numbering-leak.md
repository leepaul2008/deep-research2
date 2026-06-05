# 教训：章节编号泄露到报告正文

## 日期
2026-06-04（Opencode 调研、草书学习指南）

## 发生了什么

两份报告的章节标题中都出现了 C1/ch-01 等内部编号：

- Opencode 报告：`## C1 — 核心观点`
- 草书报告：`## C1` + `## ch-01` ~ `ch-08` 共 9 处

## 根因

主 agent 在构建 Sisyphus-Junior 的 prompt 时，将阶段1 oracle 输出中的 `id` 字段（如 "ch-01"）与 `title` 字段拼接成了章节标题传给子 agent。子 agent 直接使用了这个编号+标题的组合。

## 已添加的防护

- [x] Step 1 明确"章节标题只取 `title` 字段，不附带 `id`"
- [x] 章节 agent prompt 明确"标题不附带任何编号"
- [x] 阶段3c 验收增加全局 grep 检查
- [x] `id` 格式改为 ISO 2145（1, 2, 1.1），减少"ch-01"式编号自动转化为标题的风险

## 未来的 agent 须知

给章节 agent 的 prompt 中，标题必须只取自 oracle 的 `title` 字段。永远不要将 `id` 拼入标题。
