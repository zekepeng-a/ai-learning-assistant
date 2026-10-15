# 相似项目研究与复用判断

研究日期：2026-10-15（Asia/Shanghai）。
范围：GitHub 搜索、README、许可证、默认分支近期提交和选定源码/测试。未安装运行参考项目，未调用真实模型，未验证其学习效果或商业需求。本文件是版本固定的研究记录，不是已经验收的产品方案。

## 结论

首个试运行和改造候选是 **Li-Evan/Bloom**：与“输入专业/技能 → 理解结构 → 分步学习”的距离近，中文流程直接，批注、回答和反馈确实进入下一课生成。**HKUDS/DeepTutor** 应同时作为“现成产品能做到什么”的比较对象；若现成软件已满足目标，应明确报告这一事实。**OATutor** 作为题目—技能—作答—后续练习的机制参考。

这些是有源码支持的候选，不等于三个产品都成熟、教学有效，更不等于我们重做就有价值。复用优先于从零重建；不把多个项目的全部功能拼起来。

## 证据标记

- FACT：本轮在固定版本文件或提交中直接观察到的内容。
- INFERENCE：由这些观察推导的适配性判断。
- PROPOSAL：建议 Codex 试验的选择，允许被运行证据推翻。
- OPEN：本轮尚未确定的问题。

## 项目比较

| 项目 | 本轮定位 | 源码支持的可参考机制 | 主要边界 |
|---|---|---|---|
| [Bloom](https://github.com/Li-Evan/Bloom) | 最接近的轻量改造候选 | 大纲、逐课生成、划线问答、反馈回传、SQLite 历史 | 覆盖进度被命名成掌握进度；缺独立学习效果证据 |
| [DeepTutor](https://github.com/HKUDS/DeepTutor) | 现成产品比较对象、较完整能力参考 | 学习记录进入上下文，答案记录与后续练习机制，多种资料学习入口 | 功能/依赖面较广；选读评分器很粗，不能当通用能力判断 |
| [OpenTutor](https://github.com/zijinz456/OpenTutor) | 练习、诊断与复习机制参考 | 作答后更新练习记录、错题和概念状态；另有前置缺口检查 | 本地单用户 beta；部分功能有实验开关，近期活动含维护/宣传自动化 |
| [OATutor](https://github.com/CAHLR/OATutor) | 有学习科学研究背景的机制参考 | 题目—技能映射、BKT 更新、根据估计状态选题 | 依赖内容和技能标注；估计参数不能直接推广到任意技能 |
| [developer-roadmap](https://github.com/nilbuild/developer-roadmap) | 领域导航与资源组织参考 | 每个主题有对应内容文件和稳定节点标识 | 当前仓库主要是站点内容；许可证限制复用，默认仅链接和研究组织方式 |
| [AI Learning Path Generator](https://github.com/Enterprise-DNA-OS/ai-learning-path-generator) | 同类生成器对照 | 兴趣 → 主题 → 技能 → 计划的交互拆分 | 所读计划函数无检索，资源结构不含 URL，计划校验很浅；仅见一次发布提交 |

## 固定版本与观察到的维护

以下日期为默认分支所读 tip 的提交日期；仓库 pushed_at 可能因其他分支更新而更晚，二者不可混用。Stars 没有作为质量评分。

| 项目 | 本轮默认分支快照 | tip 日期（UTC） | 观察 |
|---|---|---|---|
| Bloom | `b3918981bb34ef5d3090dc81cc5b184555ae3bd6` | 2026-09-17 | 近期记录包含双语 UI 和生成超时相关修复 |
| DeepTutor | `6cf793bd868ba5ecbe64722936d4be8fab5a01df` | 2026-10-08 | tip 为 v1.6.14；前序提交包含研究和插件实现 |
| OpenTutor | `950ffb489a80e991eb4930c9b061a283d8d0b4ca` | 2026-10-13 | tip 为社区文案；不能仅用新提交证明功能持续成熟 |
| OATutor | `a40eff7d0bd8073d2f82dd879dba22da2b320a55` | 2026-10-10 | 前序提交涉及 Office Hours、对话记忆和内容绑定 |
| developer-roadmap | `143c7c053d30e305b04b1632ca8d601b0b8776f0` | 2026-10-15 | 内容同步提交 |
| AI Learning Path Generator | `cd83c806989ffb0c13f7a9c4ace85c603c774eb2` | 2026-04-10 | 提交列表仅返回初始开源发布 |

### 1. Bloom：接近目标，但必须纠正“覆盖即掌握”

FACT：
- [中文说明](https://github.com/Li-Evan/Bloom/blob/b3918981bb34ef5d3090dc81cc5b184555ae3bd6/README.zh.md) 提供主题、原文和项目文件三种入口；Web 模式采用 React、FastAPI、SQLite 和模型 API。
- [courses.py](https://github.com/Li-Evan/Bloom/blob/b3918981bb34ef5d3090dc81cc5b184555ae3bd6/backend/app/courses.py) 的 `generate_next_lesson` 读取上一课反馈、思考题回答与批注，并用于新一课提示；确实存在反馈回传。
- 同文件 `_auto_check_mastery` 根据生成课文的 mastery 注释更新大纲勾选；后续流程据勾选判断进入评估篇。生成内容覆盖一个主题，并不构成用户作答证据。
- [集成测试](https://github.com/Li-Evan/Bloom/blob/b3918981bb34ef5d3090dc81cc5b184555ae3bd6/backend/tests/integration/test_full_flow.py) 以 mock 模型模拟完整流程；本轮未执行这些测试。
- [大纲规则](https://github.com/Li-Evan/Bloom/blob/b3918981bb34ef5d3090dc81cc5b184555ae3bd6/skills/bloom-tutor/references/syllabus.md) 强调可验证行为，但同时要求按课文覆盖勾选条目。这种表述与掌握判断存在混淆。
- [LICENSE](https://github.com/Li-Evan/Bloom/blob/b3918981bb34ef5d3090dc81cc5b184555ae3bd6/LICENSE)：MIT。中文 GUIDE 仍出现旧仓库名，安装时核对当前 README 和实际目录。

INFERENCE：适合首先检查能否扩展现有阅读/反馈界面。应将“已讲解”“已练习”“有何证据支持理解”分开；README 中 2σ/前 2% 的宣传不是该软件的效果证明。

### 2. DeepTutor：先比较现成产品，避免重复实现

FACT：
- [README](https://github.com/HKUDS/DeepTutor/blob/6cf793bd868ba5ecbe64722936d4be8fab5a01df/README.md) 记载多种学习入口、资料工作区与发布记录；[pyproject](https://github.com/HKUDS/DeepTutor/blob/6cf793bd868ba5ecbe64722936d4be8fab5a01df/pyproject.toml) 显示依赖与集成范围较大。
- [prompt_blocks.py](https://github.com/HKUDS/DeepTutor/blob/6cf793bd868ba5ecbe64722936d4be8fab5a01df/deeptutor/agents/loop/prompt_blocks.py) 将 learning_journal_context 注入提示；[测试](https://github.com/HKUDS/DeepTutor/blob/6cf793bd868ba5ecbe64722936d4be8fab5a01df/tests/agents/chat/test_learning_journal_injection.py) 检查其位置和内容。
- 选读 [grading.py](https://github.com/HKUDS/DeepTutor/blob/6cf793bd868ba5ecbe64722936d4be8fab5a01df/deeptutor/learning/grading.py) 使用选项匹配、短答案文本相似度和开放题关键词覆盖；[mastery.py](https://github.com/HKUDS/DeepTutor/blob/6cf793bd868ba5ecbe64722936d4be8fab5a01df/deeptutor/learning/mastery.py) 使用近期正确率加权和少量样本上限。这里只描述该模块，不代表整个系统只有这些评分方式。
- [安装指南](https://github.com/HKUDS/DeepTutor/blob/6cf793bd868ba5ecbe64722936d4be8fab5a01df/docs-for-user/AGENT_SETUP.md) 提供 Agent 安装流程；需要知识库时还涉及 embedding 配置。
- [LICENSE](https://github.com/HKUDS/DeepTutor/blob/6cf793bd868ba5ecbe64722936d4be8fab5a01df/LICENSE)：Apache-2.0。

INFERENCE：适合用实际体验判断是否还有必要另做产品；不应因其功能多就照搬其 runtime、插件、MCP 和多 Agent 体系。关键词命中不能可靠判断开放式推理。

### 3. OpenTutor：实际作答链路可参考，成熟度要拆开看

FACT：
- [README](https://github.com/zijinz456/OpenTutor/blob/950ffb489a80e991eb4930c9b061a283d8d0b4ca/README.md) 将当前发布定位为本地单用户 beta；Windows 为社区支持。
- [quiz_submission.py](https://github.com/zijinz456/OpenTutor/blob/950ffb489a80e991eb4930c9b061a283d8d0b4ca/apps/api/routers/quiz_submission.py) 将作答写入记录，并尝试更新错题、概念状态和前置缺口提示；非 coding 分支的所读判题是规范化后的字符串相等。
- [knowledge_tracer.py](https://github.com/zijinz456/OpenTutor/blob/950ffb489a80e991eb4930c9b061a283d8d0b4ca/apps/api/services/learning_science/knowledge_tracer.py) 实现 BKT 并用启发式估计参数。
- [实验状态矩阵](https://github.com/zijinz456/OpenTutor/blob/950ffb489a80e991eb4930c9b061a283d8d0b4ca/docs/experimental-status-matrix.md) 列出 CAT、视觉、浏览器等开关；本轮未运行验证。
- [LICENSE](https://github.com/zijinz456/OpenTutor/blob/950ffb489a80e991eb4930c9b061a283d8d0b4ca/LICENSE)：MIT。

INFERENCE：可借鉴作答到后续干预的连接；不建议为了首版同时引入图谱、FSRS、认知负荷模型、动态布局和诊断 Agent。

### 4. OATutor：学习机制和内容工程参考

FACT：
- [README](https://github.com/CAHLR/OATutor/blob/a40eff7d0bd8073d2f82dd879dba22da2b320a55/README.md) 给出论文、内容来源、题目技能映射与参数结构。
- [BKT-brain.js](https://github.com/CAHLR/OATutor/blob/a40eff7d0bd8073d2f82dd879dba22da2b320a55/src/models/BKT/BKT-brain.js) 根据正误及 slip/guess/transit 参数更新估计。
- [defaultHeuristic.js](https://github.com/CAHLR/OATutor/blob/a40eff7d0bd8073d2f82dd879dba22da2b320a55/src/models/BKT/problem-select-heuristics/defaultHeuristic.js) 排除完成或过阈值题目，优先选择较低估计状态的题目。
- [代码许可证](https://github.com/CAHLR/OATutor/blob/a40eff7d0bd8073d2f82dd879dba22da2b320a55/LICENSE) 为 MIT；README 单独描述内容的 CC BY 4.0 与逐项署名。引用素材时须核对对应来源记录。

INFERENCE：重点学任务与证据如何连接；不把 BKT 百分比直接显示为任意领域的真实掌握程度。有论文不代表我们的新产品已得到验证。

### 5. developer-roadmap：参考组织方式，保留外链

FACT：
- [readme](https://github.com/nilbuild/developer-roadmap/blob/143c7c053d30e305b04b1632ca8d601b0b8776f0/readme.md) 说明当前仓库承载主题内容，不是整个网站源码。
- [license](https://github.com/nilbuild/developer-roadmap/blob/143c7c053d30e305b04b1632ca8d601b0b8776f0/license) 明确限制个人使用之外的内容复用及再发布，并允许分享仓库/网站链接。

PROPOSAL：默认引用链接、自行组织课程；未经授权不复制其图、文本或批量导入内容到产品。

### 6. AI Learning Path Generator：作为生成器基线

FACT：
- [README](https://github.com/Enterprise-DNA-OS/ai-learning-path-generator/blob/cd83c806989ffb0c13f7a9c4ace85c603c774eb2/README.md) 提供完整产品结构和 Supabase 部署说明。
- 所读 [generate-learning-journey/index.ts](https://github.com/Enterprise-DNA-OS/ai-learning-path-generator/blob/cd83c806989ffb0c13f7a9c4ace85c603c774eb2/supabase/functions/generate-learning-journey/index.ts) 将主题/技能转为模型提示；资源字段只有类型、标题、时长和是否免费；未见检索调用，计划分支仅浅检对象。
- 本轮提交列表只有 2026-04-10 的初始开源发布。GitHub 元数据标记 MIT。

INFERENCE：适合对照入口与信息分步呈现，不宜作为可靠课程/资源发现的现成答案。

## 下一步：用运行证据选择复用方式

PROPOSAL：
1. Codex 先在独立参考目录运行 Bloom，检查中文创建课程、批注、反馈到下一课的实际链路；记录耗时、配置与失败。
2. 对 DeepTutor 完成针对性体验/最小运行检查，比较同一个学习目标。若安装或凭据不足，报告具体阻塞，不能写“已验证”。
3. 用同一目标、资料、模型（接口允许时）比较现成方案与直接聊天；预算不能匹配时披露，不能据此得出模型能力差异。
4. 根据实际可复用程度决定：扩展 Bloom、扩展现成软件，或最小独立实现。尚未接受固定技术栈或新增架构。
5. 首个差异应落在帮助学习者解释、应用或判断上，而不是课程数量、Dashboard 数量或 Agent 数量。

OPEN：首批用户是谁、首个学习目标是什么、资料是否可获得、用户会不会实际完成练习、愿意持续使用或付费的原因。由真实使用检验；不能由本报告替代回答。
