# Lesson to Documentation

**命令**: `/lesson-to-docs [topic] [flags]`

智能知识管理系统，将高价值技术对话转化为结构化文档和架构决策记录。

## 功能描述

**智能化**知识文档化系统：
- **内容质量评级**：自动评估对话价值(A/B/C级)，只记录高价值内容
- **架构决策追踪**：ADR格式记录设计决策和推理过程
- **智能去重**：检测相似文档，建议更新而非新建
- **关联知识网络**：建立文档间关系，形成知识图谱
- **预览确认**：生成前预览内容，避免垃圾文档
- **版本追踪**：记录设计演进历史

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

### 基础参数
- `[optional-topic]` - 可选主题分类（如"vue-components"、"api-design"）

### 质量控制
- `--quality [A|B|C]` - 质量过滤，只记录指定级别内容（默认：A+B）
- `--preview` - 预览模式，显示文档大纲但不生成（推荐）
- `--force` - 强制执行，覆盖已存在的文档

### 文档类型
- `--adr` - 生成架构决策记录(ADR)格式
- `--update` - 更新已有文档而非创建新文档
- `--guide` - 生成实施指南格式

### 智能选择
- `--interactive` - 强制交互模式，显示内容评级和选择界面
- `--all-topics` - 自动处理所有A/B级话题
- `--related` - 同时更新相关文档的关联信息

## 内容质量评级

### A级内容 (架构决策)
- **架构设计决策**：系统设计、技术选型、状态码设计
- **设计模式应用**：复杂业务场景的模式选择
- **核心业务逻辑**：多模块协作、关键算法
- **用户体验洞察**：基于场景的UX设计决策
- **安全风险识别**：威胁分析和防护策略

### B级内容 (技术方案)
- **实施指南**：具体的技术实现方案
- **问题解决过程**：复杂bug的排查思路
- **性能优化**：具体的优化手段和效果
- **工具使用技巧**：开发工具的高级用法

### C级内容 (基础操作)
- **日常bug修复**：语法错误、方法名修正
- **配置调整**：简单的配置文件修改
- **文档更新**：常规的文档维护

## 执行流程

### 智能分析流程
**对话扫描** → **内容评级** → **相似度检测** → **关联分析** → **预览生成** → **用户确认** → **文档生成** → **触发系统更新**

### 决策树逻辑
```
检测内容 → C级内容？→ 建议跳过(可--force强制)
         ↓
         B/A级内容 → 检测重复？→ 建议更新已有文档
                   ↓
                   新内容 → 生成预览 → 用户确认 → 生成文档
```

## 输出位置与格式

### 文档类型与位置
- **架构决策记录**: `dev-guides/architecture-decisions/ADR-XXX-[主题].md`
- **实施指南**: `dev-guides/[领域]/[主题]-implementation.md`
- **技术方案**: `dev-guides/[领域]/[主题]-solution.md`
- **触发配置**: `CLAUDE.md` 智能触发系统（增强格式）

### 模板文件位置
- **ADR模板**: `.claude/commands/lesson-to-docs/ADR-template.md`
- **实施指南模板**: `.claude/commands/lesson-to-docs/implementation-guide-template.md`

### 智能触发系统格式 (新)
```yaml
topic_key:
  path: dev-guides/path/to/doc.md
  keywords:
    - 主关键词(10)  # 权重
    - 次关键词(8)
  context: 业务领域|技术栈|使用场景
  related: [相关文档key1, 相关文档key2]
  decision_type: architecture|technical|business
  value_level: A|B
  last_updated: 2024-01-24
```

## 使用示例

### 基础使用
```bash
# 智能分析模式（推荐）- 评级+预览+确认
/lesson-to-docs

# 预览模式 - 只看不生成
/lesson-to-docs --preview

# 质量过滤 - 只记录A级内容
/lesson-to-docs --quality A
```

### 高级使用
```bash
# 架构决策模式 - ADR格式
/lesson-to-docs --adr

# 更新已有文档
/lesson-to-docs --update api-design

# 处理所有高价值内容
/lesson-to-docs --all-topics --quality A

# 交互模式查看详细分析
/lesson-to-docs --interactive --preview
```

### 典型工作流
```bash
# 1. 先预览内容价值
/lesson-to-docs --preview

# 2. 确认后生成文档
/lesson-to-docs --quality A

# 3. 更新相关文档链接
/lesson-to-docs --related
```

## 内容评级示例

### A级示例 (架构决策)
```
🎯 [A级] NFC活动状态码设计
  - 决策类型：架构决策
  - 影响范围：整个NFC系统
  - 关键洞察：NFC碰一碰无需返回按钮
  - 建议格式：ADR
  - 存储位置：architecture-decisions/
```

### B级示例 (技术方案)
```
🔧 [B级] Vue组件乐观更新模式
  - 决策类型：技术方案
  - 影响范围：前端组件
  - 解决问题：API失败时UI状态回滚
  - 建议格式：实施指南
  - 存储位置：vue-components/
```

### C级示例 (基础修复)
```
🛠️ [C级] ThinkPHP方法名修正
  - 决策类型：bug修复
  - 影响范围：单个文件
  - 修复内容：orderBy→order
  - 建议：无需文档化
```

## 应用场景

### 推荐使用
- ✅ 架构设计完成后
- ✅ 复杂问题解决后
- ✅ 用户体验优化后
- ✅ 性能瓶颈解决后
- ✅ 安全漏洞修复后

### 不推荐使用  
- ❌ 日常代码调试
- ❌ 简单配置修改
- ❌ 常规功能开发
- ❌ 文档格式调整
