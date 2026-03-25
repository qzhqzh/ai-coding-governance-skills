# AI Coding Governance Playbook (Portable)

目的：把“垂直开发 + docs 绑定 + progress 日志”固化成可复用流程，让不同 AI coding 平台（Codex/Claude/Copilot/Trae 等）都能按同一套治理机制工作。

范围：技术栈无关；只依赖你在仓库中存在一个 `docs/` 以及一个目录语义区映射配置（`context-map`）。

## 1) 目录语义区（Zones）
你需要在仓库里创建 `./.ai-coding/context-map.json`。推荐的 zones（可改名/可增减）：
- `docs`：文档与知识（`docs/**`）
- `core`：业务核心/领域逻辑（例如 `src/**/domain/**`、`src/**/service/**`）
- `adapter`：接口适配层（例如 API handler/路由控制器）
- `data`：数据模型/迁移/Schema
- `tests`：测试代码（`tests/**`、`__tests__`、`*.test.*`）

zones 的意义：当 AI 触达某个区域时，对应要更新哪些治理资产。

## 2) Progress（垂直切片日志）的“最小结构”
每次实现/修复/重构形成一个“垂直切片（Vertical Slice）”，必须落到 `docs/progress/<slice-id>.md`。

slice 最小字段（建议结构化书写）：
- `Slice ID`
- `Goal`
- `Plan summary`
- `Boundaries (touched zones)`
- `Implementation notes`
- `Evidence`（跑了哪些测试/命令，有什么关键输出或链接）
- `Risks & invariants check`（哪些不变量被验证/无法验证就写风险）
- `Next`（下一步具体要做什么）
- `Files changed`

## 3) Bootstrap（中途初始化 docs）
触发时机：仓库已确定大方向（有 plan/技术方案），但 `docs/` 没有治理骨架。

Bootstrap 要确保以下文件存在（允许用 TODO 占位）：
- `docs/architecture.md`
- `docs/invariants.md`
- `docs/interfaces.md`（或 `api-contracts.md`）
- `docs/adr/`（至少一个初始 ADR：`0001-initial.md`）
- `docs/progress/index.md`
- `docs/progress/SINGLE_SLICE_TEMPLATE.md`
- `./.ai-coding/context-map.json`
- `./.ai-coding/governance-version.txt`

## 4) Maintainer（后续每次改动都绑定 docs）
触发时机：你让 AI 做了具体实现/修复任务。

Maintainer 的工作：
1. 根据你打算改的文件路径 + `context-map.json` 推断 `touched zones`。
2. 创建一个新的 vertical slice 日志文件，并更新 `docs/progress/index.md`。
3. 依据 touched zones 更新 governance 文档（弱约束；不要求你一定改“所有”文档）。
   - touched `docs`：slice 里写清楚“改了哪些 docs 内容”
   - touched `adapter`：更新 `docs/interfaces.md`（请求/返回/错误/语义对齐）
   - touched `core`：更新 `docs/invariants.md`（状态机/幂等/并发等不变量）
   - touched `data`：更新 `docs/architecture.md` 与可能的 `docs/adr/*`
   - touched `tests`：slice 的 Evidence 里写清测试命令与结果摘要
4. governance alignment check：必须在 slice 里写“哪些不变量被验证/无法验证的风险与下一步”。

## 5) 生成策略（避免上下文乱掉）
把“治理要求”写进输出结构，而不是靠记忆：
- AI 每次输出都必须包含 slice 结构化字段
- AI 每次必须明确指出 touched zones 与 Evidence
- 不要只改代码不落 docs；也不要只写 docs 不给测试证据

