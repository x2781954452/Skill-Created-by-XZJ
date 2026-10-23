# Skill-Created-by-XZJ

XZJ 的个人技能库。每个技能单独放在一个文件夹里，新增技能无需再创建仓库。

## 技能目录

| 技能 | 用途 | 入口 |
| --- | --- | --- |
| `github-cloudflare-release` | 将完成的网站保存到 GitHub，核对 Cloudflare 免费额度、兼容性及外部费用，按用户决定部署；未接受限制时只保存 GitHub。 | [阅读 SKILL.md](github-cloudflare-release/SKILL.md) |

## github-cloudflare-release 的流程

```text
完成项目并验证
→ 保存到 GitHub
→ 检查 Cloudflare 兼容性、免费额度及外部服务费用
→ 向用户说明限制
→ 用户决定是否部署
→ 批准后连接 Cloudflare 自动发布
```

上述流程已在技能正文中展开为可执行规则。实际保存之前还会检查 GitHub 提交是否自动触发部署，避免费用审查之前就上线。

- 遇到未经用户接受的额度限制、收费条件或未知项，先停在 GitHub。
- 不擅自绑卡、升级套餐或开通付费服务。
- 已经批准的项目范围可以沿用；新增费用或限制重新说明。
- 这是一套按需执行的指令，不是持续监控器，也不自动保证零费用。

## 给读取本仓库的 Agent

1. 先阅读本文件的技能目录，了解有哪些技能及用途。
2. 查看各技能 `SKILL.md` 开头的 `name` 和 `description`，按实际任务选择需要的技能。
3. 使用前阅读对应技能的完整 `SKILL.md`，按需读取其引用的脚本、参考资料和模板。
4. 保存到 GitHub 不等于已安装。若要作为某个 Agent 工具的原生技能使用，应保留整个文件夹结构并安装到该工具支持的位置。
5. 仓库内容本身不授予账号访问、公开发布、付费或其他外部操作权限；遵循用户在当前任务中的授权。

## 文件结构

```text
github-cloudflare-release/
├── SKILL.md
└── agents/
    └── openai.yaml
```

`SKILL.md` 是主要指令；`agents/openai.yaml` 提供 Codex 的显示名称和调用提示。
