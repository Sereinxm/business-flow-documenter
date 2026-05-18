---
name: business-flow-documenter
description: 基于证据为大中型软件仓库生成业务流程文档。仅当用户明确要求"记录业务流程"、"生成业务流文档"或指定输出为 "docs/business-flow.md" 时才使用此技能。切勿在通用代码分析、代码探索或理解类请求中自动触发。
---

# 业务流程文档生成器

将大型代码仓库转化为经过验证的业务流程文档。最终输出为 `docs/business-flow.md`，中间分析笔记存放在 `.ai-analysis/` 目录下。

## 核心原则

- 不要仅总结 README 或文件夹名称，必须追踪真实代码路径。
- 所有重要结论必须有证据支撑，优先引用文件路径、函数、类、路由、Schema、数据迁移、测试文件和配置键。
- 不确定的发现应标注为 `推测` 或 `需确认`，禁止凭空编造业务逻辑。
- 通过将工作拆分为多个聚焦的子智能体（subagent）分析轮次来最大化上下文利用率。如果环境支持实际子智能体，则委托给它们；否则以顺序/并行分析轮次模拟，每轮写入独立笔记。
- 禁止修改业务代码，只创建文档和分析文件。
- 生成可渲染的 Mermaid 图表。
- 最终文档语言以用户要求为准，默认为中文。

## 仓库访问

如果可通过工具直接访问仓库则直接检查。如果只有上传的文件则使用上传文件。如果仓库不可访问，要求用户提供仓库上传、相关源码归档或连接器访问。

对于大型仓库，避免一次性加载过多内容。从索引文件、清单文件、路由文件、Schema 文件、测试文件和入口文件开始，然后仅围绕可能的业务流程深入分析。

## 标准工作流程

1. 创建或更新分析工作区。当有文件系统访问权限时，在目标仓库根目录下运行本技能的 `scripts/create_workspace.py`。
2. 按照 `references/subagent-playbook.md` 中的描述执行子智能体分析轮次。
3. 使用 `references/evidence-rules.md` 判断哪些结论足够可靠，适合写入最终文档。
4. 使用 `references/mermaid-guidelines.md` 起草图表。
5. 使用 `references/business-flow-template.md` 作为最终文档结构。
6. 定稿前执行验证轮次。
7. 有文件系统写权限时将最终文档保存为 `docs/business-flow.md`，否则提供 Markdown 内容供用户保存。

## 子智能体分析轮次

对大多数仓库使用以下分析轮次：

- 仓库映射：技术栈、入口点、分层结构、模块地图。
- 业务领域：业务术语、角色、实体、状态枚举。
- API/入口分析：HTTP 路由、RPC、GraphQL、Webhook、CLI、消息入口。
- 服务流程：用例、服务、工作流、状态变更、分支、事务。
- 数据模型：数据库表、ORM 模型、迁移、仓储、数据生命周期。
- 前端/UI：页面、路由、组件、状态管理、API 调用、用户操作。
- 集成：第三方 API、消息队列、存储、支付、通知、搜索、AI 服务。
- 任务与消息：定时任务、Worker、生产者、消费者、事件。
- 权限与验证：认证、守卫、角色、验证器、审计日志。
- 测试验证：从单元测试、集成测试和端到端测试中推断的业务规则。
- 最终验证：冲突、缺口、无支撑的声明、遗漏的流程。
- 文档撰写：生成最终 Markdown 和 Mermaid 图表。

跳过明显不适用的轮次，但在验证笔记中说明原因。

## 所需中间文件

当有文件系统访问权限时，创建以下文件：

```text
.ai-analysis/repo-map.md
.ai-analysis/domain-model.md
.ai-analysis/api-analysis.md
.ai-analysis/service-flows.md
.ai-analysis/data-model.md
.ai-analysis/frontend-flow.md
.ai-analysis/integrations.md
.ai-analysis/tasks-and-messaging.md
.ai-analysis/permissions-and-validation.md
.ai-analysis/tests-verification.md
.ai-analysis/verification.md
docs/business-flow.md
```

如果某轮次不适用，保留文件并写入「不适用」及原因。

## 最终文档要求

最终 `docs/business-flow.md` 必须包含：

- 文档范围与分析方法。
- 系统概览与架构。
- 业务领域模型。
- 核心业务流程总览。
- 详细流程章节，包含触发器、模块、步骤、调用链、数据变更、状态变更、分支和证据。
- API 或入口到业务操作的映射。
- 数据模型与实体关系。
- 外部系统与集成。
- 权限、验证与安全逻辑。
- 定时任务、异步任务与消息。
- 从测试推断的业务规则。
- 不确定项与确认清单。
- 代码证据附录。

在证据充分的情况下，至少包含以下 Mermaid 图表类型：

- `flowchart` — 系统架构。
- `flowchart` — 核心业务流程总览。
- `sequenceDiagram` — 一个或多个关键流程。
- `stateDiagram-v2` — 状态流转。
- `erDiagram` — 数据实体关系。
- `flowchart LR` — 外部系统交互。

## 最终响应格式

创建或起草最终文档后，按以下格式总结：

```markdown
## 结果

- 最终文档：`docs/business-flow.md`
- 中间分析：`.ai-analysis/`

## 分析摘要

- 识别的核心业务流程数：x
- 生成的 Mermaid 图表数：x
- 分析的 API/入口点数：x
- 分析的核心数据模型数：x

## 主要不确定项

1. ...

## 建议人工确认项

1. ...
```

如果在当前环境中为文件创建了文件，请酌情提供下载链接或路径。
