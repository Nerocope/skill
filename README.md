# TRAE Skills

个人 TRAE 技能仓库，存放可复用的 AI Agent 技能文件。

## 技能列表

### agent-selection-report (v2.0)

生成 AI Agent 个人选型报告。基于 v9 13 节结构（McKinsey 金字塔原理），P0-P3 证据分级，六维加权评分，HTML 输出。

**核心约束**：以用户指定的外接模型（如 DeepSeek API）为主线，评估 BYOK 兼容性和成本效率。

**v2.0 更新**：
- 17 节 → 13 节，合并 4 对冗余章节
- 定位图 + 评分卡合并为同屏量化评估
- 推荐组合 + 候选池合并，决策路径融入
- 收录原则并入方法论，prescriptive/analytical 分离
- 定位图与评分卡覆盖一致（13 个候选含 HanaAgent）

**文件结构**：
```
agent-selection-report/
├── SKILL.md                    # 技能主指令（工作流、章节速查表、核心规则）
└── references/
    └── report-spec.md          # 权威规范（报告结构、评分框架、设计系统、质量清单）
```

**使用方式**：在 TRAE 对话中直接说"生成一份 AI Agent 选型报告"或"帮我对比 Agent 产品"。

## 目录结构

```
skill/
├── README.md
└── agent-selection-report/
    ├── SKILL.md
    └── references/
        └── report-spec.md
```

## 安装

将需要使用的技能文件夹复制到 TRAE 工作区的 `.trae/skills/` 目录下即可。
