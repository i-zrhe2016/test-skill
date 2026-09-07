# test-skill

本仓库维护面向 Codex 的通用测试工作流 Skill，并保留前端真实浏览器点击测试作为条件式浏览器分支。

核心入口是 `test-workflow`：从需求、Ticket 功能清单和验收标准提炼最小高价值测试集，优先使用项目已有测试框架，按 `静态检查 -> focused tests -> integration/regression -> browser/E2E` 的成本梯度执行；复杂或高风险行为采用 RED -> GREEN，普通失败先直接诊断修复，重复/原因不明/高风险失败再升级到 targeted code review。

![test-workflow 通用测试验证梯度](docs/diagrams/test-workflow-flow.svg)

渲染产物：[SVG](docs/diagrams/test-workflow-flow.svg) / [PNG](docs/diagrams/test-workflow-flow.png)；图源：[test-workflow-flow.puml](docs/diagrams/test-workflow-flow.puml)。该图使用 `plantuml-skill` 通过公共 Kroki 渲染，内容仅包含公开的工作流信息。

## Skill 清单

| Skill | 用途 | 入口 |
|---|---|---|
| `test-workflow` | 通用单元、组件、API、集成、回归和条件式浏览器验证 | [`test-workflow/SKILL.md`](test-workflow/SKILL.md) |
| `frontend-click-test` | 旧的专用前端真实浏览器点击测试；保留兼容，新的工作流应优先使用 `test-workflow` | [`frontend-click-test/SKILL.md`](frontend-click-test/SKILL.md) |

## 文档索引

- [通用测试工作流架构](docs/architecture/test-workflow.md)：测试梯度、职责边界、风险分支和失败处理。
- [通用测试工作流运行手册](docs/testing/test-workflow.md)：模式选择、验收清单、执行顺序和报告格式。
- [前端点击测试架构](docs/architecture/frontend-click-test.md)：浏览器分支的职责边界和失败自动修复闭环。
- [前端点击测试运行手册](docs/testing/frontend-click-test.md)：Playwright CLI / Chromium 的真实页面验证细节。

## 推荐执行顺序

```text
Requirement / Ticket
        |
        v
Acceptance + Test Cases
        |
        v
Static / Type / Lint
        |
        v
Focused automated tests
        |
        +-- complex/high-risk --> RED -> Implement -> GREEN
        |
        v
Integration / affected regression
        |
        +-- browser-visible --> Browser / E2E
        |
        v
Test evidence
```

原则：使用能可靠证明行为的最低成本测试层，不把浏览器 E2E 当默认反馈循环，也不为了 GREEN 放宽断言、增加盲目 retry 或固定 sleep。

## 维护约定

- 优先复用项目已有测试框架、fixture、helper 和命令。
- 每个 Ticket 的内循环保持 focused；多个 Ticket 完成后再运行必要的集成/回归测试。
- 浏览器验证只用于真实用户交互或明确要求的 E2E 行为。
- 文档中的页面地址、账号、Cookie、Token 和测试数据使用占位符，不写入真实值。
