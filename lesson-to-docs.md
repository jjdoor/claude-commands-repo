---
ai-readable: true
execution-mode: automatic
requires: [Read, Write, Grep, Glob, Sequential]
target-audience: ai-assistant
---

# AI执行指令：lesson-to-docs

**触发命令**: `/lesson-to-docs [topic] [flags]`

**我作为AI的执行目标**：智能分析对话内容，自动生成高价值技术文档和架构决策记录。

## AI执行能力

**我需要具备的分析能力**：
- **质量识别算法**：自动评估对话价值(A/B/C级)，计算文档化优先级
- **决策提取引擎**：识别架构决策点、技术选择、设计权衡
- **相似度检测**：检索existing文档，避免重复创建
- **知识图谱构建**：自动建立文档间关联关系
- **结构化生成**：基于模板自动填充内容
- **迭代追踪**：维护决策演进历史

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

## AI执行参数解析

**我如何解析用户输入**：

```yaml
# 命令解析规则
command_parsing:
  base_command: "/lesson-to-docs"
  optional_topic: 
    regex: "[a-zA-Z\\-]+"
    purpose: 约束文档生成领域
    
# 标志位执行逻辑  
flag_processing:
  quality_filter:
    "--quality A": 只处理架构决策内容
    "--quality B": 只处理技术方案内容
    "--quality C": 强制处理基础操作内容
    default: A+B 级别
    
  output_control:
    "--preview": 生成大纲但不写入文件
    "--force": 忽略重复检测，直接覆盖
    
  template_selection:
    "--adr": 使用 ADR-template.md 生成架构决策记录
    "--guide": 使用 implementation-guide-template.md 生成实施指南
    "--update [topic]": 更新existing文档而非新建
    
  automation_level:
    "--interactive": 显示分析过程和确认步骤
    "--all-topics": 自动处理所有高价值话题
    "--related": 同步更新相关文档链接
```

## AI内容评级算法

**我如何评估对话价值**：

```python
# 评级算法伪代码
def evaluate_conversation_quality(conversation):
    score = 0
    quality_indicators = {
        'A_level': {
            'keywords': ['架构', '设计', '决策', '模式', '选型', 'UX洞察', '安全风险'],
            'complexity_threshold': 0.8,
            'impact_scope': ['system_wide', 'cross_module', 'user_experience'],
            'decision_weight': 10
        },
        'B_level': {
            'keywords': ['实施', '解决方案', '优化', '技巧', 'bug排查'],
            'complexity_threshold': 0.6,
            'impact_scope': ['module_level', 'component_level'],
            'decision_weight': 7
        },
        'C_level': {
            'keywords': ['修复', '调整', '配置', '语法错误'],
            'complexity_threshold': 0.3,
            'impact_scope': ['file_level', 'line_level'],
            'decision_weight': 3
        }
    }
    
    # 执行评分逻辑
    for level, criteria in quality_indicators.items():
        if match_keywords(conversation, criteria['keywords']) and \
           complexity_score(conversation) >= criteria['complexity_threshold']:
            return level
            
    return 'C_level'  # default
```

**评级结果决定执行路径**：
- **A级** → 使用 `ADR-template.md` 生成架构决策记录
- **B级** → 使用 `implementation-guide-template.md` 生成实施指南
- **C级** → 提示跳过（除非 `--force` 标志）

## AI执行流程

**我的执行步骤**：

```yaml
execution_pipeline:
  step_1_scan:
    action: 扫描当前对话内容
    tools: [Read]
    output: conversation_context
    
  step_2_evaluate:
    action: 质量评级和价值评分
    algorithm: quality_scoring_function
    output: quality_level, topics_list
    
  step_3_similarity:
    action: 检测相似已有文档
    tools: [Grep, Glob]
    search_paths: ['dev-guides/**/*.md']
    output: similar_docs_list
    
  step_4_decision:
    action: 决定执行路径
    logic: |
      IF quality_level == 'C' AND not flags.force:
        RETURN suggest_skip()
      ELIF similar_docs_list.exists:
        RETURN suggest_update(similar_docs_list[0])
      ELSE:
        RETURN generate_new_document()
        
  step_5_generate:
    action: 选择模板并生成内容
    template_mapping:
      A_level: 'ADR-template.md'
      B_level: 'implementation-guide-template.md'
    tools: [Read, Write]
    
  step_6_finalize:
    action: 更新相关链接和索引
    tools: [Edit]
    targets: ['CLAUDE.md', 'README.md']
```

**决策树简化表示**：
```
质量评级 → C级? → 提示跳过 (--force可覆盖)
        ↓
        A/B级 → 相似文档? → 提示更新
               ↓
               新内容 → 选择模板 → 填充数据 → 生成文档
```

## AI数据映射与输出

**我如何确定输出位置**：

```yaml
# 输出路径生成规则
path_generation:
  A_level_content:
    base_path: "dev-guides/architecture-decisions/"
    filename_pattern: "ADR-{number:03d}-{topic-slug}.md"
    number_source: existing_adr_count + 1
    topic_slug: sanitize(extract_main_topic())
    
  B_level_content:
    base_path: "dev-guides/{domain}/"
    filename_pattern: "{topic-slug}-implementation.md"
    domain: detect_domain_from_conversation()
    topic_slug: sanitize(extract_implementation_topic())

# 模板读取配置
template_sources:
  adr_template: ".claude/commands/lesson-to-docs/ADR-template.md"
  implementation_template: ".claude/commands/lesson-to-docs/implementation-guide-template.md"
  
# 数据映射规则
data_extraction:
  conversation_analysis:
    main_topic: extract_primary_decision_point()
    background: summarize_problem_context(max_words=200)
    solutions: identify_considered_options()
    final_choice: extract_final_decision()
    reasoning: extract_decision_rationale()
    
  metadata_generation:
    current_date: get_system_date()
    author: "Claude AI Assistant"
    decision_id: generate_sequential_id()
    
# 触发系统更新格式
claude_md_trigger_format:
```yaml
{topic_key}:
  path: {generated_file_path}
  keywords: {extracted_keywords_with_weights}
  context: {domain}|{tech_stack}|{usage_scenario}
  related: {linked_documents}
  decision_type: {A_level: "architecture", B_level: "technical"}
  value_level: {quality_level}
  last_updated: {current_date}
```

## AI执行示例

**我如何响应不同的命令输入**：

```yaml
# 场景1: 无参数智能分析模式
user_input: "/lesson-to-docs"
my_execution:
  - step: 扫描对话内容
  - step: 识别到A级架构决策内容 
  - step: 选择ADR模板，生成预览
  - step: 询问用户确认后生成文档

# 场景2: 预览模式
user_input: "/lesson-to-docs --preview"  
my_execution:
  - step: 分析对话，评级为B级
  - step: 生成实施指南大纲
  - step: 显示预览但不写入文件

# 场景3: 强制A级处理
user_input: "/lesson-to-docs --quality A"
my_execution:
  - step: 只查找架构决策内容
  - step: 如果没有A级内容，提示无匹配内容
  - step: 如果有A级内容，生成ADR文档
```

**高级场景执行**：

```yaml  
# 场景4: 强制ADR模式
user_input: "/lesson-to-docs --adr"
my_execution:
  - step: 强制使用ADR模板
  - step: 即使内容是B级也按架构决策处理
  - step: 生成到architecture-decisions/目录

# 场景5: 更新现有文档
user_input: "/lesson-to-docs --update api-design"  
my_execution:
  - step: 搜索api-design相关文档
  - step: 找到existing文档并读取
  - step: 合并新对话内容到现有文档

# 场景6: 批量处理模式
user_input: "/lesson-to-docs --all-topics --quality A"
my_execution:
  - step: 扫描对话中所有A级话题
  - step: 为每个话题生成单独的ADR
  - step: 批量更新触发系统配置
```

## AI分析输出示例

**我如何向用户展示分析结果**：

```yaml
# 分析报告格式
analysis_output:
  quality_assessment:
    detected_level: "A级架构决策"
    confidence_score: 0.9
    key_indicators: ["状态码设计", "系统架构", "用户体验决策"]
    
  content_extraction:
    main_decision: "NFC活动状态码标准化"
    alternatives: ["简单404统一", "详细状态区分", "混合模式"]  
    chosen_solution: "详细状态区分(404/410/412/413/423)"
    key_reasoning: ["用户体验优化", "错误信息精确", "前端状态管理"]
    
  generation_plan:
    template_choice: "ADR-template.md"
    output_path: "dev-guides/architecture-decisions/ADR-003-nfc-status-code-design.md"
    related_docs: ["nfc-activity.html", "NfcController.php"]
    
  preview_content: |
    # ADR-003: NFC活动状态码设计
    ## 背景
    NFC活动页面需要区分不同的错误状态...
    ## 考虑的方案
    ### 方案A: 统一404错误...
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
