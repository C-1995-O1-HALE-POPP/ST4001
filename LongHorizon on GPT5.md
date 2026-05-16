## locomo: 

| Cat 1      | Cat 2              | Cat 3     | Cat 4                 | Cat 5       |
| ---------- | ------------------ | --------- | --------------------- | ----------- |
| Single-hop | Temporal reasoning | Multi-hop | Open-domain knowledge | Adversarial |

Acc：

| Model                            | Cat 1 | Cat 2 | Cat 3 | Cat 4 | Cat 5 |   Overall |
| -------------------------------- | ----: | ----: | ----: | ----: | ----: | --------: |
| **GPT-5.2 (Full History)**       | 0.419 | 0.423 | 0.218 | 0.697 | 0.740 | **0.600** |
| **GPT-5.2 + Observation RAG**    | 0.238 | 0.425 | 0.127 | 0.483 | 0.841 | **0.502** |
| **GPT-5-mini (Full History)**    | 0.474 | 0.566 | 0.379 | 0.721 | 0.222 | **0.532** |
| **GPT-5-mini + Observation RAG** | 0.233 | 0.478 | 0.213 | 0.445 | 0.859 | **0.502** |
| **GPT-5-mini + Summary RAG**     | 0.211 | 0.411 | 0.219 | 0.416 | 0.861 | **0.476** |
Recall Acc：

| Model / Setting                  | Cat 1 | Cat 2 | Cat 3 | Cat 4 | Cat 5 |   Overall |
| -------------------------------- | ----: | ----: | ----: | ----: | ----: | --------: |
| **GPT-5.2 + Observation RAG**    | 0.381 | 0.716 | 0.359 | 0.690 | 0.454 | **0.581** |
| **GPT-5-mini + Observation RAG** | 0.341 | 0.640 | 0.357 | 0.611 | 0.395 | **0.517** |
| **GPT-5-mini + Summary RAG**     | 0.288 | 0.571 | 0.342 | 0.594 | 0.603 | **0.537** |

GPT-5.2 和 GPT-5-mini 在不截断上下文时整体准确率更高。时间推理依然是弱点。对于不可回答问题比较谨慎

![[Pasted image 20260125220428.png]]

longMemBench:
仅相关信息：
Accuracy: 0.948
        single-session-assistant: 1.0 (56)
        single-session-user: 0.9714 (70)
        single-session-preference: 1.0 (30)
        temporal-reasoning: 0.9474 (133)
        multi-session: 0.9248 (133)
        knowledge-update: 0.9103 (78)
Saved to ../../generation_logs/full-history-session/gpt-5-mini/con/longmemeval_oracle.json_testlog_top1000context_jsonformat_useronlyfalse_20260124-1447none.eval-results-gpt-5-mini

全文：
Accuracy: 0.862
        single-session-assistant: 1.0 (56)
        temporal-reasoning: 0.782 (133)
        knowledge-update: 0.8205 (78)
        single-session-user: 1.0 (70)
        single-session-preference: 0.9667 (30)
        multi-session: 0.812 (133)

按轮次分割全文rag,topk=10：
Accuracy: 0.69
        knowledge-update: 0.8077 (78)
        single-session-preference: 0.7667 (30)
        multi-session: 0.4887 (133)
        single-session-user: 0.8714 (70)
        temporal-reasoning: 0.6767 (133)
        single-session-assistant: 0.7679 (56)
        
按会话分割全文rag,topk=10：
Evaluation results by task:
        single-session-user: 0.9286 (70)
        single-session-preference: 0.8 (30)
        single-session-assistant: 0.8571 (56)
        multi-session: 0.6692 (133)
        temporal-reasoning: 0.8045 (133)
        knowledge-update: 0.8718 (78)

Task-averaged Accuracy: 0.8219
Overall Accuracy: 0.802
Abstention Accuracy: 0.6667 (30)

MedagentBench："success rate": 0.57,
![[Pasted image 20260125221050.png]]
（在这种任务中纯llm的能力一般，并且基本上2-3步就能完成，也不算长程。MedagentBench：
```
"success rate": 0.57,
"average_history_length": 4.246
"max_history_length": 6
```
）
这也体现了gpt-5-mini本身跟随指令调用api的能力已经足够了，因为我们在300个任务中几乎没有发现因为调用api格式失败造成错误
#### （1）**Memory ≠ RAG，甚至很多时候 RAG 是负增益**
- LoCoMo：
    - GPT-5.2 **no-RAG > observation-RAG**
    - GPT-5-mini **no-RAG > observation-RAG / summary-RAG**
- LongMemEval：
    - full-history > session-split RAG
    - multi-session / temporal / knowledge-update 对 RAG 极度敏感
现有 benchmark 中，RAG 更像是“补上下文”，但对**长期一致性 & 决策轨迹**是破坏性的。
## memory相关benchmark
- 通用多轮交互：
		https://github.com/snap-research/locomo
		https://github.com/xiaowu0162/LongMemEval
- 长上下文：
		https://github.com/google-deepmind/narrativeqa
		https://huggingface.co/datasets/Tongyi-Zhiwen/ruler-128k-subset
- 多跳推理：
		https://huggingface.co/datasets/hotpotqa/hotpot_qa/viewer/distractor/train?row=1

通用领域memory：
- 超长上下文任务；
- 多轮交互，集中在多跳推理、大海捞针，可以用来检测精确检索能力，针对EHR比如某时间用了什么药，以及哪些因素决定了当前诊断；
- 现有的memory任务大多只是对于历史记录的qa，memory系统只是会读取上下文，组织记忆然后回答具体的问题，基于历史决策的推断。

**gap在于验证memory能否影响当前agent的交互能力和操作执行能力**
（比如工具调用，申请检查，申请用药/手术，搜索知识库）

> LoCoMo / LongMemEval 的核心形式仍然是：
> - ❓“某个时间点发生了什么”
> - ❓“之前谁说过什么”
> - ❓“偏好是否被记住”
> 而**不是**：
> - “基于长期状态，我现在该不该换药？”
> - “我之前已经做过哪些检查，哪些是 redundant？”
> - “当前 decision 是否和历史 decision 一致？”

可以往agent方向上看：具体到medical agent现实世界模拟环境中的任务解决：
MedAgentBench 提供FHIR API及其文档，正确调用才能得到答案，最多需五个步骤完成；EHRSQL提供数据库，agent编写查询query
	多轮交互❌，Agent ✅ Memory❌ EHR✅
AgentClinic agent必须通过对话和主动数据收集来揭示患者的诊断结果 
	多轮交互✅，Agent ✅ Memory❓ EHR❌
	（Memory上面有检索机制，不算完善）
![[Pasted image 20260125221318.png]]
MedCalc-Bench基于病例note的推理性QA
	多轮交互❌，Agent ✅ Memory❌ EHR✅
medagents-benchmark 推理性QA

在医疗场景，memory 更合理的角色是：一个随时间演化的患者状态，应该服务于 decision continuity。目前没有benchmark探索，Memory 是否能减少无效交互、减少错误操作、提升任务完成率？”

%% research
https://github.com/Ayanami0730/deep_research_bench
https://github.com/youdotcom-oss/ydc-deep-research-evals
 %%
## medical agent相关benchmark：
https://github.com/glee4810/EHRSQL
https://github.com/wshi83/MedAgentGym
https://github.com/stanfordmlgroup/MedAgentBench
https://github.com/samuelschmidgall/agentclinic # 可以跑一下这个，比较综合
https://github.com/ncbi-nlp/MedCalc-Bench
https://github.com/gersteinlab/medagents-benchmark

https://arxiv.org/abs/2601.12661 # 还没开源
https://arxiv.org/pdf/2509.15957 # 还没开源
https://arxiv.org/abs/2601.13918v1  # 还没全部开源
https://arxiv.org/html/2601.01885v1 # 还没开源

## medical agent framework：
[1] Y. Liao et al., “**AgentEHR**: Advancing Autonomous Clinical Decision-Making via Retrospective Summarization,” Jan. 20, 2026, arXiv: arXiv:2601.13918. doi: 10.48550/arXiv.2601.13918.
[2] S. Ouyang et al., “**ReasoningBank**: Scaling Agent Self-Evolving with Reasoning Memory,” Sep. 29, 2025, arXiv: arXiv:2509.25140. doi: 10.48550/arXiv.2509.25140.
[3] Y. Liao, S. Jiang, Y. Wang, and Y. Wang, “**ReflecTool**: Towards Reflection-Aware Tool-Augmented Clinical Agents,” Jun. 16, 2025, arXiv: arXiv:2410.17657. doi: 10.48550/arXiv.2410.17657.
[4] W. Shi et al., “**EHRAgent**: Code Empowers Large Language Models for Few-shot Complex Tabular Reasoning on Electronic Health Records,” Oct. 04, 2024, arXiv: arXiv:2401.07128. doi: 10.48550/arXiv.2401.07128.
[5] S. Schmidgall, R. Ziaei, C. Harris, E. Reis, J. Jopling, and M. Moor, “**AgentClinic**: a multimodal agent benchmark to evaluate AI in simulated clinical environments,” May 25, 2025, arXiv: arXiv:2405.07960. doi: 10.48550/arXiv.2405.07960.
[6] Q. Wang, R. Sheng, Y. Li, H. Qu, Y. Sun, and M. Zhu, “**MedKGI**: Iterative Differential Diagnosis with Medical Knowledge Graphs and Information-Guided Inquiring,” Jan. 04, 2026, arXiv: arXiv:2512.24181. doi: 10.48550/arXiv.2512.24181.
[8]R. Xu _et al._, “**MedAgentGym**: A Scalable Agentic Training Environment for Code-Centric Reasoning in Biomedical Data Science,” Oct. 05, 2025, _arXiv_: arXiv:2506.04405. doi: [10.48550/arXiv.2506.04405](https://doi.org/10.48550/arXiv.2506.04405).

## memory agent：
[1] C. Packer et al., “MemGPT: Towards LLMs as Operating Systems,” Feb. 12, 2024, arXiv: arXiv:2310.08560. doi: 10.48550/arXiv.2310.08560.
[2] P. Rasmussen, P. Paliychuk, T. Beauvais, J. Ryan, and D. Chalef, “Zep: A Temporal Knowledge Graph Architecture for Agent Memory,” Jan. 20, 2025, arXiv: arXiv:2501.13956. doi: 10.48550/arXiv.2501.13956.
[3] P. Chhikara, D. Khant, S. Aryan, T. Singh, and D. Yadav, “Mem0: Building Production-Ready AI Agents with Scalable Long-Term Memory,” Apr. 28, 2025, arXiv: arXiv:2504.19413. doi: 10.48550/arXiv.2504.19413.
[4] Y. Wang and X. Chen, “MIRIX: Multi-Agent Memory System for LLM-Based Agents,” Jul. 10, 2025, arXiv: arXiv:2507.07957. doi: 10.48550/arXiv.2507.07957.
[5] C. Hu et al., “EverMemOS: A Self-Organizing Memory Operating System for Structured Long-Horizon Reasoning,” Jan. 09, 2026, arXiv: arXiv:2601.02163. doi: 10.48550/arXiv.2601.02163.
[6] J. Kang, M. Ji, Z. Zhao, and T. Bai, “Memory OS of AI Agent,” May 30, 2025, arXiv: arXiv:2506.06326. doi: 10.48550/arXiv.2506.06326.
[7] W. Xu, Z. Liang, K. Mei, H. Gao, J. Tan, and Y. Zhang, “A-MEM: Agentic Memory for LLM Agents,” Oct. 08, 2025, arXiv: arXiv:2502.12110. doi: 10.48550/arXiv.2502.12110.

矩阵：
任务领域 // Agent 能力 // Memory增强

|               | QA  | 多轮对话 | 报告生成 | EHR兼容 | 知识检索 | 研究决策 | 数据处理 | 工具调用 | 长上下文 | 记忆维护 |
| ------------- | --- | ---- | ---- | ----- | ---- | ---- | ---- | ---- | ---- | ---- |
| AgentEHR      |     |      |      |       |      |      |      |      |      |      |
| ReasoningBank |     |      |      |       |      |      |      |      |      |      |
| ReflecTool    |     |      |      |       |      |      |      |      |      |      |
| EHRAgent      |     |      |      |       |      |      |      |      |      |      |
| AgentClinic   |     |      |      |       |      |      |      |      |      |      |
| MedKGI        |     |      |      |       |      |      |      |      |      |      |
| MedCopilot    |     |      |      |       |      |      |      |      |      |      |
|               |     |      |      |       |      |      |      |      |      |      |
| MemGPT        |     |      |      |       |      |      |      |      |      |      |
| Zep           |     |      |      |       |      |      |      |      |      |      |
| Mem0          |     |      |      |       |      |      |      |      |      |      |
| MIRIX         |     |      |      |       |      |      |      |      |      |      |
| MemoryOS      |     |      |      |       |      |      |      |      |      |      |
| A-MEM         |     |      |      |       |      |      |      |      |      |      |
|               |     |      |      |       |      |      |      |      |      |      |
| Ours          |     |      |      |       |      |      |      |      |      |      |

https://papers.miccai.org/miccai-2025/0537-Paper2575.html

框架对于不同LLM的表现 vs 基线模型仅环境：

环境？
- 检测提供（EHR）
- 病例检索（EHR）
- 知识库
- 执行用药/手术

