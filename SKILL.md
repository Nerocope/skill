---
name: agent-selection-report
description: Generate AI Agent selection reports for personal users. Use when user wants to compare AI Agent products, pick the best Agent for personal use, create an Agent buyer's report, or evaluate Agent tools with BYOK/DeepSeek cost constraints. Follows a fixed 17-section template with evidence grading (P0-P3), weighted scoring, and HTML output via html-report skill.
---

# AI Agent Selection Report

Generate a professional AI Agent buyer's selection report for personal users. The report helps users pick the most cost-effective Agent given their preferred external API model.

## Before You Start

**Read `references/report-spec.md`** before generating any report content. It contains the authoritative specifications for:
- §一–§二: Report positioning and 17-section template with missions
- §三: Agent inclusion principles (5 categories)
- §四: Evidence grading P0-P3 (with source HTML format)
- §五: 6-dimension scoring framework (weights + scale definitions)
- §六: Design system (CSS variables, typography, components)
- §七: Quality checklist (structure, sources, content, data)
- §八: Common pitfalls (10 items with fixes)
- §九: Search query templates (Chinese + English)
- §十: Default agent coverage list for §5

**When spec.md and these instructions conflict, spec.md wins.**

## Core Principles

1. **User-first perspective**: Report is for personal users. Focus on cost, ease of use, and practical availability — not enterprise features.
2. **External API first**: The user's preferred API model is the primary cost constraint. Evaluate agents by BYOK support quality.
3. **Evidence grading**: P0 (official docs/pricing) > P1 (GitHub/official blog) > P2 (third-party reviews) > P3 (community). P0 is the final arbiter.
4. **Reader-ready output**: No AI process traces, no conversational language, no internal revision notes. Every price has a date stamp.

## Mandatory Workflow

### Step 1: Gather Core Constraints
Determine from the user:
- Preferred external model (e.g., DeepSeek V4)
- Current Agent (if any)
- Primary use cases (office, coding, document generation, etc.)
- Monthly budget

### Step 2: Web Search (P0 First)
Search for the latest information using the query templates in spec.md §九. Search order:
1. **P0**: Official pricing pages, official docs
2. **P1**: GitHub repos, official blogs, product websites
3. **P2**: Third-party reviews, tech media

Search both Chinese and English. Use Chinese results for China availability judgments; use English results for version numbers and technical details.

### Step 3: Assemble Report
Follow the 17-section template from spec.md §二. For each section:
- Write the conclusion first, then the evidence
- Every factual claim gets a source citation `[N]`
- Use the inclusion principles from spec.md §三 to decide which agents go where

**Section quick reference** (see spec.md §二 for full missions and content specs):

| # | Section | Mission |
|---|---------|---------|
| 0 | 先看结论 | 30-second answer |
| 0.5 | 决策路径 | Let reader choose themselves |
| 1 | 推荐组合 | Concrete portfolio recommendations |
| 1.5 | 路线选择 | Open-source vs commercial vs hybrid |
| 2 | 个人场景建议 | Scene-based recommendations |
| 3 | 成本实证 | Real API bill evidence |
| 4 | 定位图 | 2D positioning map |
| 5 | 原生与外接 | Native vs BYOK comparison per agent |
| 6 | 评分卡 | 6-dimension weighted scoring |
| 7 | 当前Agent评估 | User's current agent deep-dive |
| 8 | 个人候选池 | 4-group candidate pool |
| 9 | BYOK 生态 | BYOK ecosystem overview |
| 10 | 企业边界 | Enterprise boundary clarification |
| 11 | 国内可用性 | China availability matrix |
| 12 | 风险与触发条件 | Risks and re-evaluation triggers |
| 13 | 方法与证据 | Methodology and evidence grading |
| 14 | 数据附录 | Data appendix |
| — | 来源 | Source list with P0-P3 labels |

### Step 4: Quality Check
Run through the complete checklist in spec.md §七. Pay special attention to:
- Search and remove ALL AI process trace keywords listed in spec.md §七 内容检查
- Search and remove ALL colloquial expressions
- Verify every `[N]` citation has a matching `<li id="cite-N">`
- Verify every price is tagged "截至 YYYY-MM-DD"
- Verify P0/P2 conflicts are resolved in favor of P0

### Step 5: Output
Use `html-report` skill to generate the final HTML. Apply the design system from spec.md §六 exactly:
- CSS variables, typography, layout, and component conventions as specified
- Source citation format: `<sup><a href="#cite-N">[N]</a></sup>` inline, `<li id="cite-N">` in footer
- All source URLs must be clickable `<a href="..." target="_blank">`

## Key Rules Summary

- **P0 trumps P2**: If a news article says something different from the official pricing page, the official page wins. Tag the P2 source "未在官方来源确认".
- **Version numbers**: Only from P1 GitHub Releases pages, never from blog posts.
- **Coverage**: At least 2-3 agents per group (desktop, coding, automation, native benchmarks). See spec.md §三 for inclusion rules.
- **Every section has a reason**: See spec.md §二 for the mission of each section.
- **Avoid all 10 pitfalls**: See spec.md §八 for the full list with fixes.