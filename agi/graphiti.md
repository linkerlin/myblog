---
title: Graphiti 详尽介绍

---

Graphiti 是一个专为 AI 代理设计的框架，用于构建和查询时间感知的知识图谱（Knowledge Graphs）。它特别适用于动态环境，能够持续整合用户交互、企业数据（结构化和非结构化）以及外部信息，形成一个连贯、可查询的图谱结构。与传统的检索增强生成（Retrieval-Augmented Generation, RAG）方法不同，Graphiti 支持增量数据更新、高效检索和精确的历史查询，而无需完全重新计算图谱。这使得它在开发交互式、上下文感知的 AI 应用中表现出色，例如聊天机器人、推荐系统或任务自动化代理。

Graphiti 的核心灵感来源于知识图谱的概念，即将事实表示为三元组（两个实体/节点及其关系/边），但它独特地处理关系的变化并维护历史上下文。这使得它区别于传统的知识图谱应用，能够为 AI 代理提供状态化的记忆层。根据相关论文《Zep: A Temporal Knowledge Graph Architecture for Agent Memory》，Graphiti 在代理记忆性能上达到了 state-of-the-art 水平。

#### 目的与背景
Graphiti 的主要目的是解决传统 RAG 系统在处理实时数据时的局限性。传统 RAG 依赖静态批处理索引，无法高效应对快速变化的环境（如实时用户交互或业务数据更新）。Graphiti 通过构建动态知识图谱，帮助 AI 代理实现：
- 动态用户交互和业务数据的整合与维护。
- 基于状态的推理和任务自动化。
- 使用语义、关键词和图遍历等复杂查询方法，支持演化数据。

它最初作为 Zep（一个 AI 代理内存层）的动力源，已在实际应用中证明其在实时知识管理方面的优势。

#### 关键特征
Graphiti 的特征聚焦于实时性、准确性和可扩展性。以下是其主要特征的表格总结：

| 特征                  | 描述                                                                 |
|-----------------------|----------------------------------------------------------------------|
| 实时增量更新          | 支持立即整合新数据事件（episodes），无需批处理重新计算整个图谱。     |
| 双时态数据模型        | 跟踪事件发生时间和摄取时间，支持精确的时点查询（point-in-time queries）。 |
| 高效混合检索          | 结合语义嵌入、关键词搜索（BM25）和图遍历，实现低延迟、高召回率的查询，而不依赖 LLM 摘要。 |
| 自定义实体定义        | 通过 Pydantic 模型允许开发者定义灵活的本体（ontology）和实体类型。   |
| 可扩展性              | 支持并行处理大型数据集，适用于企业级环境。                           |

这些特征使 Graphiti 在动态 AI 应用中脱颖而出，例如从静态 RAG 转向实时更新，避免了昂贵的 LLM 调用和幻觉（hallucinations）问题。

#### 底层原则解析
Graphiti 的设计原则基于知识图谱的本质，但扩展到时间维度和动态环境。核心原则包括：

1. **知识图谱表示**：知识以节点（实体）和边（关系）形式存储，形成三元组网络。这允许复杂关系建模，例如“用户 A 喜欢产品 B 在时间 T”。不同于静态图，Graphiti 处理关系的演化（如关系从“喜欢”变为“不喜欢”），通过版本控制维护完整历史。

2. **时间感知（Temporal Awareness）**：采用双时态模型（bi-temporal），区分“事件何时发生”（valid time）和“何时被系统记录”（transaction time）。这确保查询能回溯历史状态，例如查询“在 2025 年 6 月，用户对某个 LLM 的推荐是什么？”。原则上，这避免了数据覆盖问题，支持准确的时序推理。

3. **混合检索机制**：不依赖单一搜索方法，而是融合：
   - **语义搜索**：使用嵌入（embeddings）捕捉含义相似性。
   - **关键词搜索（BM25）**：基于 Lucene TF-IDF 的精确匹配。
   - **图遍历**：探索节点关系，揭示隐藏连接。
   这些通过 Reciprocal Rank Fusion (RRF) 融合结果，提高精度和速度。原则是减少对 LLM 的依赖（仅用于初始提取），从而降低成本和延迟。<grok:render card_id="3544cd" card_type="citation_card" type="render_inline_citation">
<argument name="citation_id">32</argument>
</grok:render>

4. **增量与高效**：更新仅影响相关部分，而非全局重建。这基于图数据库的原子操作原则，确保在高并发环境下的性能。

5. **自定义与灵活性**：开发者定义实体类型，遵循 Pydantic 的类型安全原则，支持自定义本体。这体现了“开发者优先”的设计，避免了硬编码限制。

这些原则使 Graphiti 适用于 AI 代理的“记忆”管理，支持状态化交互（如记住过去对话并基于新事实更新推荐）。

#### 架构解析
Graphiti 的架构强调模块化和集成性：
- **核心组件**：知识图谱构建器、查询引擎和时态管理器。图谱存储在图数据库后端，如 Neo4j（5.26+）或 FalkorDB（1.1.2+），用于嵌入存储。
- **LLM 集成**：支持 OpenAI、Google Gemini、Anthropic 或 Groq 等服务，用于推理和嵌入生成。也可使用 Azure OpenAI 或本地 LLM（如 Ollama）。
- **可选服务**：
  - **MCP 服务器**：Model Context Protocol，用于 AI 代理的语义搜索和图维护。
  - **REST 服务**：基于 FastAPI，提供 API 接口。
- **运行时**：支持并行处理（在兼容环境中），确保大规模数据处理。
架构抽象了底层复杂性，开发者只需配置数据库和 LLM 即可启动。

#### 如何工作
Graphiti 的工作流程分为摄取（Ingestion）和检索（Retrieval）两个阶段：

1. **数据摄取**：
   - 以“事件”（episodes）形式上传数据（如文本或 JSON）。
   - 系统自动提取实体和关系，使用 LLM 生成嵌入。
   - 更新图谱：新增节点/边，同时记录时态信息。示例：上传 JSON 关于新 LLM 发布的更新，Graphiti 会创建节点（如“Claude 4”）和边（如“发布于 2025-07-01”），并维护历史版本。

2. **查询与检索**：
   - 使用混合搜索：例如，查询“最新 LLM 推荐”，系统结合语义匹配、关键词和图距离重排序结果。
   - 支持中心节点搜索（探索特定节点的关系）和配方搜索（预定义查询模板，如 NODE_HYBRID_SEARCH_RRF）。
   - 输出：返回相关节点/边列表，可直接喂给 AI 代理用于生成响应。

整个过程实时发生，支持低延迟查询（无需 LLM 摘要）。在示例项目中，上传多阶段 LLM 数据后，代理能动态切换推荐（如从 Gemini 切换到 Claude）。

#### 安装与使用
安装简单，需要 Python 3.10+、图数据库和 LLM API 密钥：
- 核心安装：`pip install graphiti-core` 或 `uv add graphiti-core`。
- FalkorDB 支持：`pip install graphiti-core[falkordb]`。
- 额外 LLM：`pip install graphiti-core[anthropic,groq,google-genai]`。
- 数据库设置：使用 Neo4j Desktop 或 Docker 运行 FalkorDB（`docker run -p 6379:6379 -p 3000:3000 -it --rm falkordb/falkordb:latest`）。

快速启动示例（从 GitHub examples/quickstart）：
- 连接数据库：初始化 Neo4j/FalkorDB。
- 添加事件：`add_episode(text="Anthropic released Claude 4 on July 1, 2025.")`。
- 搜索：`search_relationships(query="最新 LLM")`，返回相关图元素。

更多用例包括与 LangChain 集成，用于状态化聊天机器人。
#### 其他注意事项
Graphiti 收集匿名使用统计以改进框架，可通过环境变量禁用。它开源于 GitHub，支持社区贡献。相比 Eclipse Graphiti（图形编辑器框架）或其他同名项目，此 Graphiti 更聚焦 AI 和知识管理。
