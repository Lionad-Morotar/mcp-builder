# MCP Builder

[![Version](https://img.shields.io/badge/version-1.0.0-blue.svg)](https://github.com/Lionad-Morotar/mcp-builder/releases)

基于 Anthropic MCP 构建指南，结合 Arcade 的 Tools Pattern，帮助你创建高质量的 MCP（模型上下文协议）服务器及 MCP Tools。

## 安装

```bash
npx skills add -g Lionad-Morotar/mcp-builder
```

## 使用

```sh
/mcp-builder {你的要求，如"帮我创建一个 GitHub MCP 服务器"}
```

如果你的 IDE 不支持 SlashCommand，那么为了获得最可靠的结果，需要提示词前加上前缀，比如：

```plaintext
使用 mcp-builder 技能，{你的要求，如"帮我创建一个 GitHub MCP 服务器"}
```

这会明确触发技能并确保 AI 遵循文档化的模式。如果不加前缀，技能触发可能不一致，具体取决于你的提示词与技能描述关键词的匹配程度。

## Source

原技能见：[Anthropic's MCP Builder Skill](https://github.com/anthropics/skills/blob/main/skills/mcp-builder/SKILL.md)
