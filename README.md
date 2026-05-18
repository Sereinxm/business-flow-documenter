# 业务流程文档生成器（Business Flow Documenter）

基于证据分析，将大型代码仓库自动转化为结构化的业务流程文档。

## 简介

本工具通过多轮子智能体分析（Subagent Passes）追踪真实代码路径，从路由、服务层、数据模型、前端页面、集成系统、定时任务、权限验证和测试用例等多个维度提取业务逻辑，最终生成包含 Mermaid 图表的完整业务流程文档。

## 适用场景

- 接手遗留项目，需要快速理解业务逻辑
- 新人入职，需要系统化的业务流程参考
- 技术文档沉淀，为团队积累业务知识库
- 代码审计，梳理系统对外交互与权限边界

## 快速开始

### 1. 安装技能

```bash
npx skills install Sereinxm/business-flow-documenter
```

### 2. 使用方式

在目标代码仓库根目录下，向 AI 助手发出如下指令：

```
帮我分析这个项目并生成业务流程文档
```

或

```
请为这个仓库创建业务流文档，输出到 docs/business-flow.md
```

技能会自动：
- 运行 `scripts/create_workspace.py` 创建分析工作区（`.ai-analysis/`）
- 执行多轮子智能体分析，逐步挖掘业务逻辑
- 生成中间分析文件
- 最终输出 `docs/business-flow.md`

### 3. 手动创建工作区

也可以手动运行脚本初始化分析目录：

```bash
python scripts/create_workspace.py /path/to/your-repo
```

## 输出文件

```
your-repo/
├── .ai-analysis/                  # 中间分析笔记
│   ├── repo-map.md                # 仓库映射：技术栈、模块结构
│   ├── domain-model.md            # 业务领域模型：术语、实体、角色
│   ├── api-analysis.md            # API 入口分析：路由与应用映射
│   ├── service-flows.md           # 服务流程：用例、状态变更、分支
│   ├── data-model.md              # 数据模型：表结构、关系、生命周期
│   ├── frontend-flow.md           # 前端流程：页面、组件、API 调用
│   ├── integrations.md            # 集成分析：第三方系统交互
│   ├── tasks-and-messaging.md     # 任务消息：定时、异步、事件
│   ├── permissions-and-validation.md # 权限与验证
│   ├── tests-verification.md      # 测试验证：从测试推断业务规则
│   └── verification.md            # 最终验证：冲突、缺口、确认清单
└── docs/
    └── business-flow.md           # 最终业务流程文档
```

## 最终文档包含

- 系统架构概览与 Mermaid 架构图
- 业务领域模型（术语表、实体、角色）
- 核心业务流程总览图
- 详细流程章节（触发器、步骤、调用链、数据变更、状态流转）
- API 与业务操作映射表
- 数据模型与实体关系图（ER 图）
- 外部系统集成关系图
- 权限、验证与安全逻辑
- 定时任务与异步消息
- 从测试推断的业务规则
- 不确定项与人工确认清单
- 代码证据附录

## 核心原则

| 原则 | 说明 |
|------|------|
| **证据驱动** | 每个结论必须引用文件路径、函数、类、路由、Schema 等具体证据 |
| **追踪代码** | 不依赖 README 或目录名，深入追踪真实代码调用链 |
| **标注不确定性** | 无法确认的发现标注为「推测」或「需确认」 |
| **不修改源码** | 只产出文档和分析文件，零侵入 |

## 技能结构

```
├── SKILL.md                       # 技能完整说明与工作流
├── agents/
│   └── openai.yaml                # Agent 配置
├── references/
│   ├── evidence-rules.md          # 证据强度判断规则
│   ├── mermaid-guidelines.md      # Mermaid 图表生成规范
│   ├── business-flow-template.md  # 最终文档模板
│   └── subagent-playbook.md      # 子智能体分析操作手册
└── scripts/
    └── create_workspace.py        # 工作区初始化脚本
```

## 许可证

MIT
