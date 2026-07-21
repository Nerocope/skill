# TRAE Skills

TRAE Work 自定义技能集合。每个子目录是一个独立技能，可放入 `.trae/skills/` 目录使用。

## 技能列表

| 技能 | 说明 |
|------|------|
| [agent-selection-report](./agent-selection-report/) | AI Agent 选型报告生成，17 节模板 + P0-P3 证据分级 + 六维评分，面向个人用户，强调 BYOK/DeepSeek 成本控制 |

## 安装

将技能目录复制到工作区的 `.trae/skills/` 下：

```
.trae/skills/agent-selection-report/
├── SKILL.md
└── references/
    └── report-spec.md
```

## 使用

在对话中调用技能名称即可：

```
请使用 agent-selection-report 技能，帮我生成一份 AI Agent 选型报告。
外接模型：DeepSeek V4，当前 Agent：HanaAgent，场景：个人办公 + 编码。
```