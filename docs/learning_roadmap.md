# All-in-RAG 学习路线图

> 本路线图基于项目章节结构（数据 → 索引 → 检索 → 生成 → 评估 → 实战）梳理，给出一条系统的学习路径：每一步标明学习目标、对应文档与代码、动手任务和自检清单。按顺序走完第 1~5 阶段，你就具备了独立开发 RAG 应用的能力。

## 路线总览

```
阶段1 入门认知          阶段2 数据与索引         阶段3 检索进阶          阶段4 生成与评估        阶段5 实战检验
┌──────────────┐   ┌──────────────────┐   ┌──────────────────┐   ┌───────────────┐   ┌──────────────────┐
│ 第1章 解锁RAG │ → │ 第2章 数据准备    │ → │ 第4章 检索优化    │ → │ 第5章 生成集成 │ → │ 第8章 项目实战一  │
│ · RAG简介     │   │ 第3章 索引构建    │   │ · 混合检索        │   │ 第6章 系统评估 │   │ 第9章 Graph RAG  │
│ · 环境准备    │   │ · 向量嵌入        │   │ · 查询构建        │   │ · 评估方法论   │   │  (选修进阶)      │
│ · 四步构建RAG │   │ · 多模态嵌入      │   │ · Text2SQL       │   │ · 评估工具     │   │                  │
└──────────────┘   │ · Milvus实战     │   │ · 查询重构        │   └───────────────┘   └──────────────────┘
                   │ · 索引优化        │   │ · 进阶技术        │
                   └──────────────────┘   └──────────────────┘
                            选修：第7章 知识图谱RAG / Agentic RAG · Extra-chapter 社区专题
```

## 前置准备

开始前请确认具备以下条件（对应第 1 章第 2 节）：

| 条件 | 说明 | 不满足时的补救 |
| --- | --- | --- |
| Python 3.12 | 项目代码基于 3.12.7 | 参考 [chapter1/virtualenv.md](chapter1/virtualenv.md) 配置虚拟环境 |
| Docker | Milvus 向量数据库通过 docker-compose 启动 | 安装 Docker Desktop 或参考官方文档 |
| LLM API Key | 教程默认使用 DeepSeek / AIHubmix 等平台 | 按第 1 章第 2 节申请，配置到环境变量 |
| Git / 命令行基础 | 克隆仓库、运行脚本 | 非硬性要求，遇到再补 |

**环境验证**：进入 `code/` 目录，先跑 `code/C1` 的最小示例；Milvus 相关实验前用 `code/docker-compose.yml` 启动服务。

---

## 阶段 1：入门认知（第 1 章）

**目标**：理解 RAG 是什么、为什么需要 RAG，并亲手跑通第一个最小 RAG 系统，建立"检索 + 生成"的整体直觉。

**学习顺序**：
1. [01_RAG_intro.md](chapter1/01_RAG_intro.md) —— RAG 双阶段架构、技术演进（Naive → Advanced → Modular）、适用场景判断
2. [02_preparation.md](chapter1/02_preparation.md) —— API Key 申请、仓库结构、开发环境
3. [03_get_start_rag.md](chapter1/03_get_start_rag.md) —— 四步构建 RAG：索引 → 检索 → 增强 → 生成

**动手任务**：
- [ ] 运行 `code/C1`，完成一次完整的"提问 → 检索 → 回答"流程
- [ ] 换一个自己的问题，观察检索到的文本块如何进入 Prompt

**自检清单**：
- [ ] 能说清 RAG 与直接问大模型的区别（知识时效性、幻觉抑制、私有知识）
- [ ] 能画出 RAG 离线索引 + 在线查询的两阶段流程
- [ ] 本地环境能成功调用 LLM API

---

## 阶段 2：数据与索引（第 2、3 章）

**目标**：掌握检索质量的"上游"——数据怎么切、向量怎么来、索引怎么建。这一阶段的质量直接决定整个 RAG 系统的下限。

**学习顺序**（3 章内部有依赖，按序学习）：
1. [04_data_load.md](chapter2/04_data_load.md) —— 多格式文档加载（PDF/Markdown/HTML，Unstructured 等工具）
2. [05_text_chunking.md](chapter2/05_text_chunking.md) —— 分块策略：固定长度、递归、语义分块及各自的取舍
3. [06_vector_embedding.md](chapter3/06_vector_embedding.md) —— Embedding 原理、MTEB 排行榜、模型选型
4. [07_multimodal_embedding.md](chapter3/07_multimodal_embedding.md) —— CLIP 图文对齐、多模态向量
5. [08_vector_db.md](chapter3/08_vector_db.md) —— 向量数据库的分类与核心概念（ANN、相似度度量）
6. [09_milvus.md](chapter3/09_milvus.md) —— Milvus Schema 设计、索引结构、实战检索（需 Docker）
7. [10_index_optimization.md](chapter3/10_index_optimization.md) —— 结构化索引、元数据过滤等优化手段

**动手任务**：
- [ ] 用 `code/C2` 对同一份 PDF 尝试两种分块策略，对比切分结果
- [ ] 用 `code/C3` 将分块向量化并写入 Milvus，完成一次向量检索
- [ ] （可选）跟随 `09_milvus.md` 完成多模态图文检索实验

**自检清单**：
- [ ] 能解释分块大小（chunk size）与重叠（overlap）对检索的影响
- [ ] 能说出至少 3 种常见向量索引类型（如 FLAT/HNSW/IVF）的适用场景
- [ ] 能独立完成"文档 → 分块 → 嵌入 → 入库 → 检索"全流程

---

## 阶段 3：检索进阶（第 4 章）

**目标**：这是 RAG 效果提升的核心章节。朴素向量检索只解决了"能查"，本章解决"查得准"。

**学习顺序**：
1. [11_hybrid_search.md](chapter4/11_hybrid_search.md) —— 稠密 + 稀疏（BM25）混合检索、RRF 融合
2. [12_query_construction.md](chapter4/12_query_construction.md) —— 从自然语言构建结构化查询、自查询检索器
3. [13_text2sql.md](chapter4/13_text2sql.md) —— 自然语言转 SQL，打通结构化数据源
4. [14_query_rewriting.md](chapter4/14_query_rewriting.md) —— 查询改写、HyDE、退步提示、逻辑/语义路由
5. [15_advanced_retrieval_techniques.md](chapter4/15_advanced_retrieval_techniques.md) —— 重排序（Rerank）、C-RAG 等高级技术

**动手任务**：
- [ ] 在 `code/C4` 中对比纯向量检索与混合检索的召回差异
- [ ] 实现一个查询改写实验：同一问题改写前后检索结果对比
- [ ] （可选）跑通 Text2SQL 示例，用自己的数据库表测试

**自检清单**：
- [ ] 能解释为什么 BM25 与向量检索互补，RRF 如何融合两路结果
- [ ] 能描述 Rerank（交叉编码器）与双塔召回的区别
- [ ] 知道什么场景该用查询路由、什么场景该用查询改写

---

## 阶段 4：生成与评估（第 5、6 章）

**目标**：补全 RAG 链路的"出口"——把检索结果可控地变成回答；并学会用指标量化系统效果，让优化有据可依。

**学习顺序**：
1. [16_formatted_generation.md](chapter5/16_formatted_generation.md) —— 结构化输出、格式控制（JSON/表格等）
2. [18_system_evaluation.md](chapter6/18_system_evaluation.md) —— RAG 三元组（上下文相关性/答案忠实度/答案相关性）等评估方法论
3. [19_common_tools.md](chapter6/19_common_tools.md) —— Ragas、Phoenix 等评估工具实操

**动手任务**：
- [ ] 给 `code/C5` 的生成环节加上格式约束，稳定输出 JSON
- [ ] 用评估工具给自己的阶段 2~3 检索结果打分，记录基线指标

**自检清单**：
- [ ] 能说出 RAG 评估要分别看"检索"和"生成"两侧的哪些指标
- [ ] 能跑通一次完整的自动化评估并解读结果

---

## 阶段 5：实战检验（第 8 章，必修；第 9 章，选修）

**目标**：把前四个阶段串成一个真实可运行的"今天吃什么"美食推荐 RAG 系统，检验独立开发能力。

**第 8 章学习顺序**：
1. [01_env_architecture.md](chapter8/01_env_architecture.md) —— 项目架构与环境配置
2. [02_data_preparation.md](chapter8/02_data_preparation.md) —— 菜谱语料的加载与分块（对应 `data/C8`）
3. [03_index_retrieval.md](chapter8/03_index_retrieval.md) —— 索引构建与检索优化
4. [04_generation_sys.md](chapter8/04_generation_sys.md) —— 生成集成与系统整合

**动手任务**：
- [ ] 从零跑通完整系统，替换成自己感兴趣的数据集
- [ ] 用阶段 4 的评估方法给系统打分，找出最弱的环节并针对性优化

**第 9 章（选修）**：在实战一基础上引入 Graph RAG：
[图RAG架构设计](chapter9/01_graph_rag_architecture.md) → [图数据建模](chapter9/02_graph_data_modeling.md) → [Milvus索引构建](chapter9/03_index_construction.md) → [智能查询路由](chapter9/04_intelligent_query_routing.md)

---

## 选修拓展（完成主线后按兴趣选学）

| 方向 | 内容 | 适合谁 |
| --- | --- | --- |
| 图结构知识 | [20_kg_rag.md](chapter7/20_kg_rag.md) 基于知识图谱的 RAG | 想处理多跳推理、复杂关联查询 |
| 智能体化 | [21_agentic_rag.md](chapter7/21_agentic_rag.md) Agentic RAG | 想让 RAG 具备自主决策、工具调用能力 |
| 社区专题 | [Extra-chapter](https://github.com/datawhalechina/all-in-rag/tree/main/Extra-chapter) —— Neo4J 实践、多模态 Omni Embedding 等 | 按专题按需查阅 |

> 第 10 章"项目实战二"目前规划中，学习计划无需等待。

## 常见问题与建议

1. **不要跳过阶段 2 直接学阶段 3**：不理解嵌入与索引的来源，检索优化只能是调参玄学。
2. **每一章都动手**：`code/C1~C9` 与 `data/` 是配套资产，看懂 ≠ 会写，跑通才算数。
3. **Docker 起不来 Milvus**：先确认 Docker 运行中，再在 `code/` 目录执行 `docker compose up -d`；端口冲突时修改 `docker-compose.yml` 映射。
4. **API 费用控制**：前期实验用小模型、限制 max_tokens；评估阶段批量调用前先小样本试跑。
5. **遇到问题**：优先对照本文档自检清单定位环节（数据 → 索引 → 检索 → 生成），再去项目 Discussions 提问。
