# Usage (Claude VS Code)

Claude VS Code 的“可迁移”接入方式：通过项目内指令文件/上下文文件，让 Claude 始终遵守同一套治理要求。

建议做法（任选其一，按你的插件习惯选择）：
1. 在仓库根目录创建 `CLAUDE_PROJECT_INSTRUCTIONS.md`，把本目录 `PLAYBOOK.md` 的关键章节复制进去或引用。
2. 或者把关键指令直接写进你的 Claude 插件“Project”/“Instructions”输入框。

你需要确保 Claude 每次收到请求（实现/修复/重构）时，输出包含：
- vertical slice 日志字段结构
- touched zones
- Evidence（测试/命令/链接）

如果你希望更自动化：让 Claude 先读取 `./.ai-coding/context-map.json`，再决定 touched zones，并写入 `docs/progress/<slice-id>.md`。

