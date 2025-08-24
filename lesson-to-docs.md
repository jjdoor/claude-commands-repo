# Lesson to Documentation

**命令**: `/lesson-to-docs [topic] [flags]`

智能经验文档化系统，自动将问题解决过程转化为结构化知识文档。

## 功能描述

**完全自动化**的经验文档化系统：
- 自动分析当前对话内容，提取技术问题、解决方案、失败尝试
- 自动创建目录结构和技术文档
- 自动更新CLAUDE.md智能触发关键词系统
- **零用户干预**，一键完成所有文档化操作

## 命令配置

```yaml
---
command: "/lesson-to-docs"
category: "Knowledge Management"
purpose: "Intelligent experience documentation and knowledge base management"
wave-enabled: false
performance-profile: "standard"
---
```

## 系统集成

- **Auto-Persona**: Scribe, Analyzer, Mentor
- **MCP Integration**: Sequential (conversation analysis), Context7 (documentation patterns)
- **Tool Orchestration**: [Read, Write, Edit, Grep, Glob]

## 参数说明

- `[optional-topic]` - 可选主题分类（如"vue-components"、"api-design"）
- `--interactive` - 强制交互模式，即使单话题也显示选择界面
- `--all-topics` - 自动处理所有检测到的话题，无需用户选择
- `--list-only` - 只列出检测到的话题但不生成文档（预览模式）
- `--force` - 强制执行，覆盖已存在的文档

## 核心功能
AI自动分析对话内容，生成技术文档并更新智能触发系统。


## 执行流程

### 执行步骤
**话题检测** → **用户选择**
**单话题**：直接生成文档  
**多话题**：展示选择界面 → 生成选中文档 → 更新CLAUDE.md触发系统

## 输出位置

- **主要文档**: `dev-guides/[专题]/`
- **触发配置**: `CLAUDE.md` 智能触发系统

## 使用示例

```bash
# 基本用法：自动分析对话，多话题时展示选择界面
/lesson-to-docs

# 指定主题分类（跳过话题检测）
/lesson-to-docs vue-components

# 自动处理所有话题：无需用户选择，全部记录
/lesson-to-docs --all-topics
```

## 触发场景

技术问题解决 | 最佳实践发现 | 系统性bug修复 | 功能集成完成 | 性能优化完成 | 环境配置完成
