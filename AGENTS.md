# Agent Harness Engineering

## 项目简介

基于Agent Harness Engineering（智能体驾驭工程）顶层思想的多Agent软件开发实践方案，聚焦「人类制定驾驭规则、多Agent在约束框架内完成全流程开发」的上层落地。

核心逻辑：**人定驾驭规则 → Agent受控执行 → 全链路闭环校验 → 熵收敛治理**

## 核心文档索引

| 文档名称 | 文档路径 | 内容概述 |
|----------|----------|----------|
| 全局架构总览 | [docs/overview.md](docs/overview.md) | 核心思想、全局架构图、多Agent角色分工、全局前置条件 |
| 模块1：需求标准化转化 | [docs/requirements-standardization.md](docs/requirements-standardization.md) | 需求校准流程、校验标准、《需求校准指南》编写规范 |
| 模块2：架构设计与多维度评审 | [docs/architecture-design-review.md](docs/architecture-design-review.md) | 架构设计流程、多Agent评审机制、《方案设计文档编写指南》、《设计方案评审文档编写指南》 |
| 模块3：开发对齐与代码实现 | [docs/development-alignment.md](docs/development-alignment.md) | 开发前对齐确认、门禁机制、《设计方案对齐确认文档编写指南》、《方案对齐评估指南》 |
| 模块4：自动化验收与全流程闭环 | [docs/automated-acceptance.md](docs/automated-acceptance.md) | 全维度验收流程、版本打标、《验收标准文档编写指南》、《验收报告编写指南》 |
| 模块5：文档熵收敛治理 | [docs/doc-entropy-governance.md](docs/doc-entropy-governance.md) | 解决代码与架构设计文档脱节漂移问题；Git Hook（pre-push）刚性拦截 + Agent-6（文档熵收敛巡检Agent）兜底巡检双机制；《文档熵收敛巡检检查指南》 |
| 模块6：知识库文档组织与索引结构 | [docs/knowledge-base-organization.md](docs/knowledge-base-organization.md) | docs目录结构、文档命名规范、版本映射机制、锚点要求 |
| 模块6：指南与规约文档的自我迭代 | [docs/guide-self-iteration.md](docs/guide-self-iteration.md) | 指南生成策略、Agent自主学习进化机制、规约减重/增重流程与模板 |
| 模块7：统一交互入口集成方案 | [docs/unified-interaction-entry.md](docs/unified-interaction-entry.md) | 方式A（@单个Agent单步精细控制）与方式B（@Workflow-Agent全流程自动化）长期并存、按需选择；会议助手Agent（腾讯会议预约+会议纪要+需求草稿转化）；规约迭代闭环；Workflow-Agent新增条件 |
| 版本管理统一指南 | [docs/version-management-unified-guide.md](docs/version-management-unified-guide.md) | 版本号规范、版本映射表维护、需求ID与版本联动规则 |

## 项目级规范文档

| 文档名称 | 文档路径 | 内容概述 |
|----------|----------|----------|
| 系统架构 | [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md) | 系统架构、组件依赖 |
| 编码规范 | [docs/CONVENTIONS.md](docs/CONVENTIONS.md) | 命名规范、编码约束、代码风格 |
| 测试规范 | [docs/TESTING.md](docs/TESTING.md) | 单元测试、集成测试、验收测试标准 |
| 环境搭建 | [docs/ENVIRONMENT.md](docs/ENVIRONMENT.md) | 开发环境搭建、依赖安装、常用命令 |

## Agent 角色索引

| Agent | 角色名称 | 企微身份 | 触发方式 | 核心职责 | 执行依据 |
|-------|----------|----------|----------|----------|----------|
| Agent-0 | 需求校准Agent | @需求校准 | 人类手动触发 | 将口语化需求转化为标准化PRD文档 | 《需求校准指南》 |
| Agent-1 | 高级架构师Agent | @方案设计 | 人类手动触发 | 基于PRD设计架构方案与验收标准 | 《方案设计文档编写指南》 |
| Agent-2 | 平级评审Agent | @方案评审 | 人类手动触发 | 评审架构方案，提出分级问题与修改建议 | 《设计方案评审文档编写指南》 |
| Agent-3 | 高级开发Agent | @对齐确认 | 人类手动触发 | 对齐设计方案，编写代码与单元测试 | 《设计方案对齐确认文档编写指南》《方案对齐评估指南》 |
| Agent-4 | 验收Agent | @验收 | 人类手动触发 | 依据验收标准执行全维度自动化验收 | 《验收标准文档编写指南》《验收报告编写指南》 |
| Agent-5 | 规约指南Agent | @规约指南 | 人类在企微群中 @ 触发 | 接收Review反馈，维护与迭代全套规约指南文档 | 各类指南文档 |
| Agent-6 | 文档熵收敛巡检Agent | 无（不占企微席位） | Git pre-push 钩子自动触发；主干合并增量触发；每日22:00全量定时触发 | 全链路文档熵收敛巡检，校验版本联动与锚点一致性，阻断不合规推送 | 《文档熵收敛巡检检查指南》 |
| 专家Agent（3类） | 安全/性能/合规评审Agent | 各自独立身份 | 人类手动触发 | 补充评审，覆盖安全、性能、合规三个专项维度 | 各专项评审规则 |
| 会议助手Agent | 会议助手Agent | @会议助手 | 人类手动触发 | 预约腾讯会议、生成结构化会议纪要、将会议内容整理为可输入 Agent-0 的需求草稿 | docs/unified-interaction-entry.md §5 |
| Workflow-Agent | 全流程编排Agent | @Workflow-Agent | 人类手动触发（质量达标后新增入群） | 按序编排调度 Agent-0~4 完成全流程，推送关键节点产出物 | 各Agent指南文档 |

> Agent-6 由 Git pre-push 钩子自动触发，不在企微群中以数字人员工形式存在；其余 Agent-0 至 Agent-5 及会议助手Agent 长期常驻企微群，Workflow-Agent 在各 Agent 输出质量稳定达标后新增入群。

## Agent执行指南文档

| 指南名称 | 文档路径 | 使用Agent |
|----------|----------|-----------|
| 《会议助手执行指南》 | [docs/guide/meeting-assistant-guide.md](docs/guide/meeting-assistant-guide.md) | 会议助手Agent |
| 《需求校准指南》 | [docs/guide/requirement-calibration-guide.md](docs/guide/requirement-calibration-guide.md) | Agent-0 |
| 《方案设计文档编写指南》 | [docs/guide/solution-design-guide.md](docs/guide/solution-design-guide.md) | Agent-1 |
| 《设计方案评审文档编写指南》 | [docs/guide/design-review-guide.md](docs/guide/design-review-guide.md) | Agent-2 |
| 《设计方案对齐确认文档编写指南》 | [docs/guide/design-alignment-guide.md](docs/guide/design-alignment-guide.md) | Agent-3 |
| 《方案对齐评估指南》 | [docs/guide/design-alignment-evaluation-guide.md](docs/guide/design-alignment-evaluation-guide.md) | Agent-3 |
| 《验收标准文档编写指南》 | [docs/guide/acceptance-criteria-guide.md](docs/guide/acceptance-criteria-guide.md) | Agent-4 |
| 《验收报告编写指南》 | [docs/guide/acceptance-report-guide.md](docs/guide/acceptance-report-guide.md) | Agent-4 |
| 《文档熵收敛巡检检查指南》 | [docs/guide/doc-entropy-check-guide.md](docs/guide/doc-entropy-check-guide.md) | Agent-6（文档熵收敛巡检Agent） |

## 目录结构速览

```
/
├── AGENTS.md                  # 本文件：项目索引入口
├── agent-harness-engineering.md  # 完整原始方案文档（归档用）
├── 版本映射表.xlsx              # 需求ID→版本→文档路径→Commit映射
└── docs/
    ├── overview.md            # 全局架构总览
    ├── requirements-standardization.md  # 模块1
    ├── architecture-design-review.md    # 模块2
    ├── development-alignment.md         # 模块3
    ├── automated-acceptance.md          # 模块4
    ├── doc-entropy-governance.md        # 模块5
    ├── knowledge-base-organization.md   # 模块6（文档组织）
    ├── guide-self-iteration.md          # 模块6（指南迭代）
    ├── unified-interaction-entry.md     # 模块7（含会议助手Agent）
    ├── version-management-unified-guide.md  # 版本管理统一指南
    ├── guide/                 # Agent执行指南文档目录
    ├── demand/                # 需求产出物目录（按需求ID组织）
    ├── slim/                  # 规约提案管理目录（减重+增重）
    └── reference/             # 参考文档（术语表、变更日志等）
```

## 快速开始

1. 阅读 [全局架构总览](docs/overview.md)，理解核心思想和角色分工；
2. 按 [全局落地前置条件](docs/overview.md#14-全局落地前置条件必做否则无法推进) 完成环境搭建和规则文档准备；
3. 从 [模块1：需求标准化转化](docs/requirements-standardization.md) 开始，按模块顺序推进；
4. 遇到规约问题，参考 [指南与规约文档的自我迭代](docs/guide-self-iteration.md)。
