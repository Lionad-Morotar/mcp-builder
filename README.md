# MCP Builder

基于 Anthropic MCP 构建指南，结合 Arcade 的 Tools Pattern，帮助你创建高质量的 MCP（模型上下文协议）服务器。

## 简介

MCP Builder 是一个全面的 MCP 服务器开发指南和工具集，旨在帮助开发者构建高质量、可维护的 MCP 服务器。它提供了：

- **完整的工作流程**：从研究规划到实现、测试和评估的端到端指南
- **多语言支持**：TypeScript 和 Python 的详细实现指南
- **设计模式**：54 个工具设计模式，涵盖 10 个关键分类
- **评估框架**：创建和运行评估测试 MCP 服务器有效性

## 安装和使用

```bash
npx skills add Lionad-Morotar/mcp-builder
```

如果你的 IDE 不支持 SlashCommand，那么为了获得最可靠的结果，需要提示词前加上前缀，比如：

```plaintext
使用 mcp-builder 技能，{你的要求，如"帮我创建一个 GitHub MCP 服务器"}
```

这会明确触发技能并确保 AI 遵循文档化的模式。如果不加前缀，技能触发可能不一致，具体取决于你的提示词与技能描述关键词的匹配程度。

## 核心功能

- **四阶段工作流**：研究规划 → 实现 → 审查测试 → 创建评估
- **语言特定指南**：TypeScript（推荐）和 Python 的完整实现指南
- **工具设计模式**：54 个模式帮助设计高质量的 MCP 工具
- **评估框架**：创建评估问题集测试 MCP 服务器有效性
- **最佳实践**：来自 Anthropic 和 Arcade 的 MCP 开发最佳实践

## 项目结构

```
mcp-builder/
├── README.md                 # 项目说明
├── LICENSE.txt               # 许可证
├── skills/
│   └── mcp-builder/
│       ├── SKILL.md          # Skill 定义和使用指南
│       ├── reference/        # 参考文档
│       │   ├── mcp_best_practices.md    # MCP 最佳实践
│       │   ├── node_mcp_server.md       # TypeScript 指南
│       │   ├── python_mcp_server.md     # Python 指南
│       │   ├── tools_patterns.md        # 工具设计模式
│       │   └── evaluation.md            # 评估指南
│       └── scripts/          # 评估脚本
│           ├── connections.py
│           ├── evaluation.py
│           └── example_evaluation.xml
```

## 快速开始

1. **了解 MCP 协议**：阅读 [MCP 最佳实践](./skills/mcp-builder/reference/mcp_best_practices.md)
2. **选择技术栈**：
   - TypeScript：[TypeScript 指南](./skills/mcp-builder/reference/node_mcp_server.md)
   - Python：[Python 指南](./skills/mcp-builder/reference/python_mcp_server.md)
3. **设计工具**：参考 [工具设计模式](./skills/mcp-builder/reference/tools_patterns.md)
4. **创建评估**：使用 [评估指南](./skills/mcp-builder/reference/evaluation.md) 创建测试

## 设计模式分类

工具设计模式涵盖 10 个关键分类：

| 分类 | 核心问题 |
|------|----------|
| Tool Types | Query、Command 还是 Discovery？ |
| Tool Interface | Agent 如何理解和调用？ |
| Tool Discovery | Agent 如何找到合适的 Tool？ |
| Tool Composition | 是否应该捆绑多个操作？ |
| Tool Execution | 同步、异步还是事务性？ |
| Tool Response | 结果应该是什么样？ |
| Tool Context | 身份和状态如何管理？ |
| Tool Resilience | 如何从失败中恢复？ |
| Tool Security | 如何控制访问？ |
| Integration | 如何连接外部系统？ |

## 许可证

详见 [LICENSE.txt](./LICENSE.txt)
