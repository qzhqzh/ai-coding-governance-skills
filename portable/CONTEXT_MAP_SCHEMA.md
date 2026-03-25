# context-map.json Schema (Informal)

用途：让不同 AI 平台都能用同一份“目录语义区”配置来绑定治理更新。

推荐文件路径：
- `./.ai-coding/context-map.json`

推荐内容结构（你可以扩展，但保持基本字段）：
```json
{
  "version": 1,
  "zones": {
    "docs": ["docs/**"],
    "core": ["src/**/domain/**", "src/**/service/**"],
    "adapter": ["src/**/api/**", "src/**/routes/**", "src/**/controllers/**"],
    "data": ["prisma/**", "migrations/**", "db/**", "**/schema.prisma"],
    "tests": ["tests/**", "**/__tests__/**", "**/*.test.*", "**/*.spec.*"]
  }
}
```

解释：
- `version`：治理协议版本号（允许未来升级）
- `zones`：zoneName -> glob patterns

zoneName 只要在你所有 skill/平台 prompt 中保持一致即可。

