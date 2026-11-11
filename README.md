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

### profile-from-chatlogs

用任意聊天资料（AI 对话导出、微信记录、邮件、日记）对一个人做基于证据的人本分析，产出约 1 万字的连续长文而非指标诊断报告。

**解决的核心问题**：长聊天记录装不进上下文（会被被动压缩丢失信息）、只看单方发言导致语境失真、把用户粘贴进来的他人内容误当成本人自述。

**关键机制**：
- 开工前先 4 问拷问需求形态（照镜子 / 捅破 / 做决定，读法，揭穿分寸）
- 恢复"用户提问 + AI 回复"完整回合序列，不只看单方
- 分区分卷：按时间切 3~4 区，高价值会话抽成 key 卷整读，全量生成索引；单卷 ≤12 万字符，避免被压缩
- 逐卷结构化笔记（必须保留原句证据）→ 二级合并找跨时期矛盾模式
- 引文归因审核：区分本人自述 / 转述他人 / 粘贴的第三方材料
- 成文：平实但深的口吻，引文可点击跳回原文，正文连续不设核对附录区

**文件结构**：
```
skills/profile-from-chatlogs/
└── SKILL.md                    # 拷问问卷 + 归因规则 + 六步工作流 + 常见坑
```

**使用方式**：把聊天记录交给 Agent，说"用 profile-from-chatlogs 分析我"。

## 目录结构

```
skill/
├── README.md
└── skills/
    ├── agent-selection-report/
    │   ├── SKILL.md
    │   └── references/
    │       └── report-spec.md
    ├── git-windows-schannel-fix/
    │   └── SKILL.md
    └── profile-from-chatlogs/
        └── SKILL.md
```

## 安装

将需要使用的技能文件夹从 `skills/` 下复制到对应 Agent 工作区的技能目录（如 TRAE 的 `.trae/skills/` 或 WorkBuddy 的 `.workbuddy/skills/`）即可。
