# AI Agent Skills

个人 AI Agent 技能仓库，存放可复用的 Agent 技能文件（TRAE / WorkBuddy 等）。

## 技能列表

### agent-selection-report (v2.0)

生成 AI Agent 个人选型报告。基于 v9 13 节结构（McKinsey 金字塔原理），P0-P3 证据分级，六维加权评分，HTML 输出。

**核心约束**：以用户指定的外接模型（如 DeepSeek API）为主线，评估 BYOK 兼容性和成本效率。

**文件结构**：
```
skills/agent-selection-report/
├── SKILL.md                    # 技能主指令（工作流、章节速查表、核心规则）
└── references/
    └── report-spec.md          # 权威规范（报告结构、评分框架、设计系统、质量清单）
```

**使用方式**：在 TRAE 对话中直接说"生成一份 AI Agent 选型报告"或"帮我对比 Agent 产品"。

### git-windows-schannel-fix

Windows / PortableGit 环境下向 GitHub 推送的排错与正确流程。覆盖：TLS schannel 吊销报错修复、凭据助手选 wincred（禁用 manager/GCM）、全局提交身份、fetch 后远程跟踪分支偶发不落盘的怪癖与最轻修法、多 git 共存不误伤、rebase/merge/fetch 在沙箱正常的澄清。

**适用**：WorkBuddy 内置 PortableGit 或自装 git（如 `C:\wnm\software\git`）直连 GitHub（HTTPS）。

**文件结构**：
```
skills/git-windows-schannel-fix/
└── SKILL.md                    # 排错流程 + 正常上传命令流 + 禁忌
```

## 目录结构

```
skill/
├── README.md
└── skills/
    ├── agent-selection-report/
    │   ├── SKILL.md
    │   └── references/
    │       └── report-spec.md
    └── git-windows-schannel-fix/
        └── SKILL.md
```

## 安装

将需要使用的技能文件夹从 `skills/` 下复制到对应 Agent 工作区的技能目录（如 TRAE 的 `.trae/skills/` 或 WorkBuddy 的 `.workbuddy/skills/`）即可。
