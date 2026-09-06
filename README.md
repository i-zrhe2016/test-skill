# test-skill

本仓库维护面向 Codex 的前端真实浏览器点击测试 skill。它用于在一个前端功能点完成后，从需求和实际改动中提炼最小验收清单，并使用 Playwright CLI 驱动 bundled Chromium 完成可视化验证，最后输出简洁的 Markdown 报告。

## 整体架构

![frontend-click-test 真实浏览器验收闭环](docs/diagrams/frontend-click-test-flow.svg)

渲染产物：[SVG](docs/diagrams/frontend-click-test-flow.svg) / [PNG](docs/diagrams/frontend-click-test-flow.png)；图源：[frontend-click-test-flow.puml](docs/diagrams/frontend-click-test-flow.puml)。该图使用 `plantuml-skill` 通过公共 Kroki 渲染，适用于本仓库公开的 skill 架构信息。

数据流的核心边界是：skill 负责确定范围、编排操作和记录结果；Playwright CLI 负责浏览器控制；Chromium 负责提供真实页面行为。代码阅读、静态 HTML 或 `curl` 不能替代浏览器验证。

## Skill 清单

| Skill | 用途 | 入口 |
|---|---|---|
| `frontend-click-test` | 对页面、组件和用户交互执行最小真实浏览器点击验证 | [`frontend-click-test/SKILL.md`](frontend-click-test/SKILL.md) |

## 文档索引

- [前端点击测试架构](docs/architecture/frontend-click-test.md)：职责边界、数据流、状态和安全约束。
- [前端点击测试运行手册](docs/testing/frontend-click-test.md)：前置检查、执行步骤、证据留存和报告格式。
- [PlantUML 数据流图源文件](docs/diagrams/frontend-click-test-flow.puml)：可复用的架构图源文件。

## 维护约定

一个功能点完成后应尽快完成一次定向点击测试闭环。Skill 文本发生变化时，同步检查运行手册和架构说明；文档中的页面地址、账号、Cookie、Token 和测试数据一律使用占位符，不写入真实值。
