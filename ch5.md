# 第5章 代码模型谱系

**一句话**：`CodeXGLUE` 到 `Qwen2.5-Coder` 是一张按任务与成本取舍的谱系表，不是一条谁取代谁的进化链。

**为什么需要它**：面对 `StarCoder`、`Code Llama`、`DeepSeek-Coder`、`Qwen2.5-Coder` 这些名字，团队最常问「哪个最好」。这个问题没有答案，因为每个模型对应的任务、数据口径、上下文预算与开放方式不同。把谱系铺开，才能按补全、理解、生成、修复四类任务分别选，并且知道每个分数是在什么 `benchmark` 上拿到的。

**核心结构**：`CodeXGLUE` 把代码理解与生成整理成统一 `benchmark` 集合，`code search`、`clone detection`、`code summarization` 等任务由此可比较；`CodeBLEU` 针对代码生成提出兼顾语法与语义结构的自动指标，弥补仅用 `BLEU` 的不足；`CodeGen` 与 `CodeRL` 分别代表多轮生成与用执行反馈强化学习的路线，`CodeRL` 把 `reward model` 接到生成过程；`StarCoder` 与 `Code Llama` 给出开放权重路线上的代码基础模型；`DeepSeek-Coder` 与 `Qwen2.5-Coder` 把 `repository-level` 上下文、补全场景与 `instruction tuning` 放到更显眼的位置。

**关键设计**：选型要同时看四件事：任务是不是需要 `fill-in-the-middle`，上下文预算能否覆盖 `repository-level` 证据，`tokenizer` 与词表是否适配目标语言，以及 `model card` 与 `data leakage` 处理是否透明。综合报告口径把「模型能力」与「工程可交付性」分开看，并强调新模型不一定在旧任务上全面占优。

**拆解**：
### 1. 理解任务与生成任务不同
`clone detection`、`type prediction`、`code summarization` 属于理解类任务，判定方式仍是 `benchmark`；`code completion` 与 `program synthesis` 属于生成类，需要执行判定。同一模型在两个任务上的表现不必一致，选型时先把任务归类。

### 2. 指标也在演化
`BLEU` 来自自然语言翻译，对代码来说会忽略结构；`CodeBLEU` 加入语法与语义结构的考虑，但仍不能替代 `functional correctness`。把指标当成筛选器而不是目标，才不会被指标牵着改产品方向。

### 3. 开放权重带来的工程变量
`StarCoder`、`Code Llama` 等开放模型让团队可以改 `tokenizer`、做 `fine-tuning`、控制部署位置；代价是要自己承担 `pre-training` 之外的适配与运维。`DeepSeek-Coder` 与 `Qwen2.5-Coder` 的报告把补全与仓库上下文作为重点，选型时要回到自己的任务上验证，而不是照抄报告排名。

### 4. 数据与泄漏是选型的一部分
`decontamination` 与 `data leakage` 在榜单讨论里常被简化，在工程上却影响可信度：如果评测集被模型见过，分数就不能代表泛化。选型时把 `model card` 与数据处理说明读一遍，比只看分数更能预测真实表现。

**今天怎么做**：先写清任务的四类归属与上下文预算，再从谱系中挑两到三个候选做同仓库对照；对照同时报执行判定与静态指标，并记录 `model card` 中可核验的数据说明。不要让榜单顺序替你做决定。

<div class="chapend" markdown="1">

**容易误解的地方**：新版本不等于全任务更强；`CodeBLEU` 也不是 `functional correctness` 的替代。开放权重降低的是部署门槛，不自动降低评测与治理成本。

**记住这一句**：模型谱系是取舍表，任务、上下文、指标与数据透明度共同决定选型。

</div>
