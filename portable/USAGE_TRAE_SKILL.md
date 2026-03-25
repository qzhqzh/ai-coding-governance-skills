# Usage (Trae Skill)

Trae 自带 skill 功能时，推荐把 governance playbook 作为 skill 的“默认工作流指令”。

建议你在 Trae 里创建两个 skill（更清晰，低耦合）：
1. `ai-coding-project-doc-bootstrap`
2. `ai-coding-doc-maintainer`

Skill 内容（你可以直接粘贴）：
- bootstrap：只做 docs/progress/context-map/governance-version 的初始化与骨架落地
- maintainer：每次实现/修复后推断 touched zones，追加 vertical slice 日志，并按 zones 更新相应治理文档

关键点：
- Trae skill 不需要“支持所有模型/限制”，只要能输出一致的 slice 结构化字段即可。
- 保持 zoneName 在所有 skill 中一致（以 `context-map.json` 为准）。

