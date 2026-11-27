# RAG Studio

**面向知识密集型场景的 RAG / Agentic RAG 平台**，以可验证的检索质量、可追溯的回答证据和可观测的运行过程为核心，逐步构建从文档接入、知识加工、混合检索到智能问答与 Agent 工作流的完整能力。

> 当前仓库处于 V2 工程治理与能力升级阶段。本文区分现有基线能力和规划中的目标能力；未完成或未经过真实环境验证的功能不会标记为已交付。

## 项目简介

RAG Studio 起源于一个基于电商知识库的问答系统。项目希望解决的不只是“从文档中找到一段文字并生成回答”，而是建立一套可扩展、可评估、可观测、可部署的知识增强应用基础设施。

核心设计原则：

- **证据优先**：回答尽可能绑定检索证据、来源文档和页码；证据不足时明确表达不确定性。
- **模块可替换**：解析器、Embedding、Reranker、LLM、向量存储和检索策略通过适配层解耦。
- **质量可量化**：使用固定评估集和检索 / 生成指标衡量变更效果，避免仅凭主观体验判断。
- **过程可观测**：记录请求链路、检索阶段、模型调用、Token、延迟、成本和错误信息，并对敏感数据脱敏。
- **渐进式演进**：优先稳定基础 RAG，再逐步引入 Agentic RAG 与 Graph RAG，避免无必要的整体重写。

## 当前技术基线

根据当前源码恢复与基线验证记录，项目包含：

| 层次 | 当前基线 |
| --- | --- |
| 后端 | Python、FastAPI |
| 前端 | Vue 3 / Vite 工程 |
| 关系数据 | SQLite |
| 向量检索 | Milvus Lite |
| 检索能力 | 已验证基础混合检索链路 |
| API 能力 | 已有认证、问答及相关管理接口基础实现 |
| 测试 | 后端测试基线 33/33 通过；前端生产构建通过 |

以上是恢复阶段记录的基线结果，不代表所有生产能力已经完成。真实 Embedding、Rerank、LLM 调用及真实 SSE 问答仍需配置有效服务凭据后验证；历史并发压测结果也需要在受控环境中重新执行。

## 目标能力路线图

| 阶段 | 目标 | 主要内容 |
| --- | --- | --- |
| Phase 0 | 工程治理 | 配置、安全、依赖锁定、测试与可复现构建 |
| Phase 1 | Document Intelligence | PDF、DOCX、XLSX、PPTX、HTML 等文档解析与统一结构 |
| Phase 2 | Knowledge Processing | 智能分块、知识库管理、版本、增量索引与数据生命周期 |
| Phase 3 | Advanced Retrieval | Dense、BM25、Hybrid、RRF、Query Enhancement、Rerank |
| Phase 4 | Agentic RAG | Query Planner、多轮检索、证据判断、受控工具调用 |
| Phase 5 | Graph RAG | 实体关系抽取、知识图谱、多跳与图向量混合检索 |
| Phase 6 | RAG Evaluation | Recall@K、MRR、NDCG、Faithfulness、Citation Accuracy 等评估 |
| Phase 7 | Observability | Trace、Token、延迟、成本、缓存与错误监控 |
| Phase 8 | Production & Security | Docker、持久化、备份恢复、访问控制与限流 |
| Phase 9 | Frontend Productization | Dashboard、知识库、文档、检索实验室、Chat、Trace、评估与设置 |
| Phase 10 | Final Acceptance | 全新环境安装、端到端流程、部署和恢复验收 |

阶段名称表示规划目标，不表示相关能力已经实现。具体实施状态以项目路线图、阶段报告和可复现测试证据为准。

## 目标架构

    Frontend / Product
      Dashboard · Knowledge Bases · Documents · Chat
      Chunk Explorer · Retrieval Lab · Agent Trace · Evaluation
                             |
    Application / API
      Auth · User · Knowledge Base · Document · Chat
                             |
    RAG Core
      Document Intelligence -> Knowledge Processing
      Query Processing -> Retrieval -> Fusion -> Rerank
      Context Assembly -> Answer Generation
                             |
    Optional Agent & Graph
      Query Planner · Retrieval Agent · Tool Registry
      Entity / Relation Graph · Multi-hop Retrieval
                             |
    Infrastructure Adapters
      SQL · VectorStore · Embedding · Reranker · LLM
      File Storage · Observability · Evaluation

目标架构采用渐进式模块化设计。现有实现会先经过审计，再决定是否抽象或替换；不会仅为引入某个框架而重写稳定代码。Graph RAG 作为可选能力，其故障不应影响基础 RAG。

## 质量与安全原则

- 所有阶段使用统一状态：PASS、FAIL、PARTIAL、BLOCKED、NOT VERIFIED。
- 严格区分真实 Provider 调用、集成测试、模拟测试和 Mock 测试。
- 不以 Mock 结果冒充真实模型调用，不以历史压测数据冒充当前性能结果。
- API Key、JWT Secret 等敏感配置不得提交到 Git。
- 文档、数据库和索引变更必须考虑备份、迁移、回滚及数据一致性。
- 每个阶段独立测试、审计、提交并保留回滚点。
- 开发工作使用 feature 分支；main 不作为直接开发分支。

## 本地开发

当前仓库仍在工程治理阶段，启动步骤、环境变量和依赖版本以仓库后续补齐并验证的开发文档为准。不要将示例配置中的占位值当作可用凭据。

建议开发前先阅读：

- docs/architecture/RAG_STUDIO_ARCHITECTURE.md
- docs/architecture/DOCUMENT_INTELLIGENCE.md
- docs/architecture/RETRIEVAL_ENGINE.md
- docs/architecture/AGENTIC_RAG.md
- docs/architecture/RAG_EVALUATION.md
- docs/architecture/OBSERVABILITY.md
- docs/roadmap/RAG_STUDIO_ROADMAP.md
- docs/roadmap/IMPLEMENTATION_STATUS.md
- docs/roadmap/PHASE_DEPENDENCY_MAP.md
- docs/roadmap/ENGINEERING_GOVERNANCE.md

## 项目状态

项目正在从恢复后的基础 RAG 系统向专业级 RAG / Agentic RAG 平台演进。当前优先工作是完成工程治理和可复现基线，再按阶段建设文档理解、知识加工、高级检索、评估、观测、Agent、Graph 与生产部署能力。

**项目状态以代码、测试结果和阶段验收报告为准，而不是以路线图中的目标描述为准。**
