### KnowFlow 知汇 · 可追溯 RAG 知识库

<p class="project-meta"><strong>独立开发（全栈）</strong> · 2024.09.15 – 2026.02.20 · Python 3.13 · FastAPI · SQLAlchemy · SQLite（WAL + FTS5）· Chroma · DashScope（Qwen / text-embedding-v3）· Vue 3 + Vite + Element Plus · pytest / Vitest / GitHub Actions</p>

面向企业与个人的可追溯 RAG 知识库，目标是让大模型的回答严格回到用户自己的资料而不是自由发挥，并使检索全过程可追踪。用户可以创建并分类管理多个知识库，上传 txt、md、pdf、docx、pptx 五种文档；系统在后台异步完成解析、按标题层级构建 Section 章节树、按段落感知规则切分文本块，并同步写入事实数据库与向量索引。

问答环节采用「FTS5 词面 + 语义向量 + RRF 融合」的三通道混合检索，检索通道可在系统设置中切换为 vector / lexical / hybrid；命中的参考片段连同相似度分数、章节路径一起交给通义千问，生成带 `[资料 N]` 引用标记与检索 trace 的回答。回答前有统一的拒答阈值门禁，证据不足时直接拒答而不调用 LLM。系统同时支持五种学习任务模式（question / explain / compare / summarize / exercise）、三种隐私模式（local / hybrid / cloud）、多轮会话与指代消解，以及可从会话一键沉淀的 note / exercise / mistake 学习记录。

<p class="project-label">核心工作</p>

- **架构分层**：确立「SQLite 保存事实数据 / Chroma 保存可重建向量索引 / 云端模型为可降级外部能力」的三层数据边界；无 API Key 时自动降级为本地确定性向量与本地抽取式回答，知识库索引可随时重建而不丢失上传资料。
- **混合检索**：实现 FTS5 中文双字分词 + 向量通道 + RRF 融合，以及统一的拒答阈值语义。
- **文档与提示词**：完成文档结构化解析与 Section 章节分块，并加入提示词注入防护。
- **可靠性**：处理 WAL 并发、消息先落库再调 LLM、索引重建防重入与失败补偿，以及启动时的孤儿向量对账。
- **工程与质量**：搭建覆盖 176 项 pytest 与 47 项 Vitest 的 CI 质量体系；把前端主包体积从 1.17MB 优化到 473KB。
- **管理能力与交付**：提供状态统计、Provider 健康面板、流式数据导出与二次确认清理；后端可同源托管前端静态资源，并附 Docker 与 docker-compose 本地部署形态。

[[Code]](https://github.com/LBStruggleee/KnowFlow)

---

### DataQuery Copilot · 自然语言数据查询工具

<p class="project-meta"><strong>独立开发（全栈）</strong> · 2025.03.12 – 2025.06.26 · Python 3.10+ · FastAPI · SQLite（WAL）· DeepSeek API（OpenAI 兼容接口）· pandas · matplotlib · React + TypeScript + Vite · Playwright · Docker / Render</p>

把「自然语言 → SQL 生成 → 安全校验 → 执行 → 数据清洗 → 可视化输出」全链路打通的数据查询工具：用户用自然语言提问，DeepSeek 大模型依据表结构生成 SQLite 语句，经多层安全防护后执行，结果自动完成缺失值填充、格式标准化等清洗，并按数据特征自动选择柱状图、折线图、饼图或散点图输出。

系统提供 CLI 与 HTTP API + React 工作台两种使用形态：CLI 支持交互式问答、单次查询与批量查询并输出汇总报告，工作台支持侧栏切换与上传数据集、填写访问密钥、分页查看结果与图表预览。平台内置 API Key 签发与吊销、按 Key 的滑动窗口限流与日配额、统一错误码契约与 request_id 链路追踪，构成可安全对外开放的接口层；数据层支持 CSV / Excel（.xlsx）导入 SQLite 并自动推断字段类型，上传有 50MB 与数据集数量上限保护。

<p class="project-label">核心工作</p>

- **数据接入层**：实现 CSV / Excel 导入与字段类型自动推断，以及交互 / 单次 / 批量三种 CLI 模式、FastAPI + React 工作台。
- **SQL 安全体系**：字符串与注释剥离 + 词边界关键词黑名单 + 多语句拒绝的校验层，`sqlite3` set_authorizer 表白名单执行层与 set_progress_handler 执行超时中断；另有 SQL 失败自动修正重试与 LRU 查询缓存。
- **接口与部署**：交付 API Key 签发吊销、滑动窗口限流与日配额、统一错误码与 request_id 追踪；提供非 root 容器镜像、Render 部署方案（持久磁盘、示例库播种、免鉴权健康探针）与 SQLite 在线备份脚本。
- **质量体系**：搭建覆盖 252 项 pytest、24 项 Vitest 与 14 项 Playwright E2E 的 CI 测试体系。

[[Code]](https://github.com/LBStruggleee/DataQuery-Copilot)
