---
name: project-bootstrap
description: Use when starting a new coding project, taking over an existing codebase without clear governance, or deciding the minimum README, AGENTS, PRD, SPEC, task, and Git structure before implementation.
---

# 轻量项目初始化

## 核心原则

先判级、再确认、后初始化。采用最小充分工程化，不复制企业级模板，不实现业务功能。后续规格、计划、修复、验证和审查交给 Dev Skills。

## 先判断

只读了解：新建或既有、一次性或长期、工具型或产品型、文件和模块规模、数据/安全/平台风险、协作者、测试与发布需求。

| 等级 | 典型情况 | 最小结构 |
|---|---|---|
| L0 临时 | 一次性、可重做 | 源码；必要时短 README |
| L1 微型 | 单脚本、低风险、持续维护 | README、AGENTS、源码、Git |
| L2 小型 | 状态复杂或产品型、长期维护 | README、AGENTS、SPEC、CHANGELOG、源码、Git |
| L3 标准 | 多模块、多人、自动测试 | README、AGENTS、src、tests、specs/docs、Git |
| L4 复杂 | 多服务、多团队、合规发布 | 仅经用户明确批准增加 ADR、CI、环境与发布治理 |

产品型项目按需增加 PRD：有目标用户、用户流程、范围取舍或成功指标时创建。纯技术工具默认不创建 PRD。PRD 写为什么/为谁/做什么；SPEC 写系统怎样工作，禁止重复。

## 初始化门禁

先输出：推荐等级、项目性质、最小目录、每份文档职责、Git方案、明确不创建的内容与理由。必须等待用户确认，确认前不创建、移动、删除或改写文件。

既有项目先读 README、AGENTS、Git状态和入口代码；有多个历史副本时先确认真实运行基线，不以文件名最新者猜测。不可逆整理必须单独批准。

确认后只创建获批内容，初始化单一 `main` 分支Git基线。已有Git不得重建；身份缺失不得虚构；不自动建远程仓库、推送、建PR或CI。

## 防过度工程化

默认禁止：Monorepo、多包、ADR目录、PR模板、复杂分支、工作树、CI/CD、发布机器人、重复规格、空文档、为未来假设预建模块。只有当前需求能证明收益且用户批准时才增加。

项目文档职责：README=入口和运行；AGENTS=代理约束；PRD=产品目标和范围；SPEC=行为与验收；CHANGELOG=已发布变化；任务卡=本次改动。不存在内容就不建空壳。

## 交接 Dev Skills

初始化结束后停止，不实现功能。模糊需求用 `dev-grill-docs`，复杂计划用 `dev-plan`，实现按需用 `dev-tdd`，Bug用 `dev-fix`，完成前用 `dev-verify`，提交前用 `dev-code-review`。明确小改可跳过规格和计划。

## 常见错误

- 代码小就省略产品边界：产品复杂度不等于代码行数。
- 产品小就上Monorepo/ADR/PR/CI：先用L2单体结构。
- 把所有规则写进AGENTS：产品行为放PRD/SPEC。
- 初始化时顺便开发：初始化和业务实现分开。
- 静态检查当运行通过：明确自动验证与人工验收边界。
