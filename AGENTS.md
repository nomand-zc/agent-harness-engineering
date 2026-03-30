# Agent Harness Engineering

## 核心文档

| 文档 | 路径 | 内容 |
|------|------|------|
| 架构设计 | [ARCHITECTURE.md](ARCHITECTURE.md) | 系统定位、调度流程、组件依赖图、状态机 |
| 编码规范 | [docs/CONVENTIONS.md](docs/CONVENTIONS.md) | 强制约束、命名规范 |
| 测试规范 | [docs/TESTING.md](docs/TESTING.md) | 单元/集成/基准测试规范 |
| 文档编写规范 | [docs/DOC_CONVENTIONS.md](docs/DOC_CONVENTIONS.md) | 文档职能分类、内容准入原则、修改质量门槛、引用规范 |
| 验收标准 | [docs/COMMIT_ACCEPTANCE.md](docs/COMMIT_ACCEPTANCE.md) | 覆盖率、通过率、pre-commit 门禁的完整验收条件 |
| Code Review 规范 | [docs/CODE_REVIEW.md](docs/CODE_REVIEW.md) | pre-push AI 代码审查机制、三级 checklist、报告格式 |
| 文档同步规范 | [docs/DOC_SYNC.md](docs/DOC_SYNC.md) | pre-push 文档漂移检测、14 个文档 checklist、内容约束 |
| 本地环境 | [docs/ENVIRONMENT.md](docs/ENVIRONMENT.md) | Docker 依赖启动、集成测试、基准测试、Makefile 命令速览 |

## 目录结构（一句话速览）

```
account/          聚合根：Account + ProviderInfo + Status + TrackedUsage + AccountStats
balancer/         负载均衡器：Pick（三种模式）+ ReportSuccess/Failure + 占用控制
  occupancy/      并发占用控制器（Unlimited / FixedLimit / AdaptiveLimit）
selector/         选号策略接口 + 内置策略（account 级 + group 级）
resolver/         服务发现层：从存储解析可用 Provider 和 Account
storage/          定义存储接口，以及后端存储驱动的接口实现（Memory / SQLite / MySQL / Redis）
  filtercond/     通用过滤条件表达式树
health/           健康检查编排器 + 内置检查项 + ReportHandler
  checks/         内置检查项（credential / probe / recovery / refresh / usage / usage_rules）
circuitbreaker/   熔断器：连续失败阈值 + 动态计算
cooldown/         冷却管理器：限流后暂停选号
usagetracker/     账号额度用量追踪管理, 同时也是做主动用量限流控制的依据，避免在使用账号时候被动发现账号的限流情况
cli/              命令行管理工具，主要用于方便快速测试账号管理的各个功能
```