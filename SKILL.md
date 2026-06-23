---
name: development-process-skills
description: Use when starting, planning, changing, implementing, testing, or accepting a software project under the user's document-first SOP. Enforces a progressive workflow: clarify requirements with grill-skills, then requirements doc → logical architecture → technical architecture → test docs → database/code implementation → module tests → full integration/acceptance testing with a 95%+ target. Each document phase must stop for user review before moving on.
---

# Development Process Skills

## 0. Progressive Disclosure Rule

Keep this skill lightweight by default. Do **not** dump the full process every time.

Use three levels of detail:

1. **Level 1 · Operating spine**: always keep in mind and mention briefly when relevant.
2. **Level 2 · Current phase checklist**: reveal only the checklist for the phase currently being executed.
3. **Level 3 · Templates/details**: reveal only when writing that artifact or when the user asks for details.

Default response after activation:

```markdown
我会按 development-process-skills 执行：需求澄清 → 需求文档 → 逻辑架构 → 技术架构 → 测试文档 → 库表/代码 → 模块单测 → 全量联调验收。每个文档阶段完成后停下来等你审核。

当前阶段：<阶段名>。
下一步：<具体动作>。
```

---

## 1. Operating Spine · Always Enforce

### Phase order

```text
0. 判断任务类型
1. grill-skills 聊清需求
2. 需求文档，完成后等用户审核
3. 技术无关的逻辑架构文档，完成后等用户审核
4. 细化技术架构文档，完成后等用户审核
5. 测试计划/测试用例文档，完成后等用户审核
6. 库表/迁移/初始化数据实现
7. 代码按模块实现
8. 每完成一个模块，立即单测
9. 全模块联调 + 验收测试
10. 覆盖率目标 95%+
```

### Non-negotiable gates

- **No code before docs** for new projects, major features, behavior changes, or architecture changes.
- **Each document layer requires user review** before the next layer.
- **Document changes before code changes** when behavior, architecture, API, data model, or tests are affected.
- **Module done means module tests passed**; otherwise it is not done.
- **Final done means integration/acceptance verified** and coverage reported.

### Change order for existing projects

When modifying existing behavior:

```text
需求文档（若需求变）
→ 逻辑架构（若职责/流程变）
→ 技术架构（若实现形态变）
→ 测试用例文档
→ 库表（若数据变）
→ 代码
→ 单测
→ 联调/验收
```

Mechanical edits may skip heavy documentation only when they do not change behavior; state why.

---

## 2. Phase Router · Decide What to Do Now

Classify the user request first:

| Request type | Start phase |
|---|---|
| New project / vague idea | Phase 1 |
| Major feature / new module | Phase 1 or 2, depending on existing approved docs |
| Architecture/data/API change | Update Phase 2/3 docs first |
| Behavior change | Update requirements + tests first |
| Bug fix | Verify expected behavior in docs/tests; document if missing |
| Test/doc-only request | Relevant document phase |
| Pure typo/mechanical change | Direct fix allowed, explain it is mechanical |

If unsure, choose the safer document-first phase.

---

## 3. Current Phase Checklists

Reveal only the checklist for the current phase.

### Phase 1 · Requirements Clarification

Use `grill-skills` style questioning.

Clarify only what affects design:

- Users and scenarios.
- Goals / non-goals.
- Core workflows.
- Roles and permissions.
- Data to persist.
- External integrations.
- Success and acceptance criteria.
- Constraints and risks.

Output: requirements document.  
Gate message:

```markdown
需求文档已沉淀完成。请你审核，确认后我再进入逻辑架构设计阶段。
```

### Phase 2 · Logical Architecture

Write technology-independent architecture: what the system does, module responsibilities, data flow, state, rules, errors, contracts, Mermaid diagrams.

Avoid specific frameworks, database engines, package names, exact API signatures unless they are requirement constraints.

Output: `架构.md` or project convention.  
Gate message:

```markdown
逻辑架构文档已沉淀完成。请你审核，确认后我再进入技术架构细化阶段。
```

### Phase 3 · Technical Architecture

Convert logical design into concrete implementation design.

Include:

- Tech stack and versions.
- Module/package structure.
- Dependency direction.
- Database/storage choices.
- API/interface shape.
- Config, integration, security, logging, testing strategy.

For complex projects, use 总-分 documents.

Gate message:

```markdown
技术架构文档已沉淀完成。请你审核，确认后我再进入测试用例文档阶段。
```

### Phase 4 · Test Documentation

Write test plan and module-split test cases before implementation.

Requirements:

- Put under `/test` unless project convention differs.
- One file per module plus index/plan.
- Cover positive, abnormal, and boundary cases.
- Map every acceptance point to at least one case.
- Target business acceptance coverage: **95%+**.

Gate message:

```markdown
测试用例文档已沉淀完成。请你审核，确认后我再进入库表与代码实现阶段。
```

### Phase 5 · Database Implementation

Implement schema/migrations/init data/indexes after docs are approved.

Check:

- Tables map to documented data requirements.
- Constraints prevent invalid states.
- Indexes support documented queries.
- Seed/mock data supports tests.

### Phase 6 · Module Implementation

For each module:

1. Confirm scope from docs.
2. Implement only that module’s responsibility.
3. Add/update unit tests.
4. Run module tests.
5. Fix before moving on.
6. Report status.

Module completion report:

```markdown
模块：<name>
实现内容：...
测试：<command/result>
覆盖：...
遗留：...
下一步：...
```

### Phase 7 · Integration and Acceptance

Run full verification:

- Unit tests.
- Integration tests.
- API/manual tests if needed.
- DB migration/init verification.
- External/mock integration verification.
- Acceptance checklist.
- Coverage report, target **95%+**.

If target is not reached, explain uncovered areas and risks.

---

## 4. Artifact Templates · Reveal Only When Needed

### Requirements document skeleton

```markdown
# 项目需求文档

## 1. 背景与目标
## 2. 用户角色
## 3. 使用场景
## 4. 功能需求
## 5. 非功能需求
## 6. 数据需求
## 7. 外部依赖 / 集成
## 8. 边界与非目标
## 9. 验收标准
## 10. 待确认问题
```

### Logical architecture skeleton

```markdown
# 架构设计文档

## 1. 文档定位
## 2. 架构总览
## 3. 核心原则
## 4. 模块职责边界
## 5. 核心数据流
## 6. 状态 / 记忆 / 数据策略
## 7. 模块间信息契约
## 8. 异常与降级流程
## 9. 待确认问题
```

### Technical architecture skeleton

```markdown
# 技术架构文档

## 1. 技术栈与版本
## 2. 工程结构
## 3. 模块落地设计
## 4. 数据库 / 存储设计
## 5. 接口 / 契约设计
## 6. 外部集成
## 7. 配置与环境
## 8. 安全与异常处理
## 9. 测试策略
## 10. 待确认技术项
```

### Test case fields

```markdown
| 字段 | 内容 |
|---|---|
| 用例编号 | MOD-001 |
| 模块 / 功能点 |  |
| 用例标题 |  |
| 前置条件 |  |
| 测试步骤 | 1. ... |
| 预期结果 | 可断言、明确 |
| 对应需求/设计来源 | 章节或编号 |
| 优先级 | 高/中/低 |
| 用例类型 | 正向/异常/边界 |
```

### Acceptance report skeleton

```markdown
# 验收测试报告

## 1. 测试范围
## 2. 测试环境
## 3. 测试执行摘要
## 4. 覆盖率统计
## 5. 模块测试结果
## 6. 联调测试结果
## 7. 验收用例通过情况
## 8. 缺陷与修复记录
## 9. 未覆盖风险
## 10. 结论
```

---

## 5. Response Discipline

- Be concise: show the current phase, current action, and gate only.
- Do not print all phases unless the user asks for the full process.
- Use tasks for multi-step work.
- Read existing project conventions before creating or editing artifacts.
- Use Context7 for library/framework/API docs when needed.
- Report test outcomes truthfully.
- If a phase is being skipped, explicitly state why it is safe or ask for approval.
