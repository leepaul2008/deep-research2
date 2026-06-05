# 教训：Scrapling 跳过的代价

## 日期
2026-06-04（AI CLI 调研 Opencode 调研）

## 发生了什么

在 AI CLI 调研中，Phase 2a 的 Exa 搜索返回了丰富的高质量结果。然后执行流程直接**跳过**了 Step 2（Scrapling 批量抓取），转而从 Exa 返回的摘要片段中提取数据构建数据池。

## 后果

报告中出现来源归因混淆（AgentMarketCap 与 SourceryIntel 的数据被混用，同一数据在不同章节引用了不同来源），导致报告发布后需要修复。

## 根因

1. Exa 返回的摘要看起来"足够详细"，产生了虚假的充分感
2. 顺手从摘要提取数据比额外启动 Scrapling 更"快"
3. 当时 SKILL.md 的规则不够强硬（仅描述性语言，无阻断点）

## 已添加的防护

- [x] 硬规则"Scrapling 不可跳过、不可替代"（RULES.md）
- [x] ⚡ 阻断点：Scrapling 未完成禁止进入 Step 3（SKILL.md 阶段2a）
- [x] 补抓链路说明（bulk → stealthy → fetch 三级）
- [x] 来源全部失败时，标记"来源稀缺"跳过，不以 Exa 替代

## 未来的 agent 须知

不要跳过 Scrapling。Exa 摘要只是索引卡片，Scrapling 全文才是数据来源。无论 Exa 摘要看起来多详尽，都必须执行 Scrapling。规则无例外——quick 模式也不例外。
