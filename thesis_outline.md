# ST4001 Thesis Outline — LongMedBench: Long-Horizon Clinical Decision-Making

> **Thesis Title (Proposed):**  
> **LongMedBench: Benchmarking Memory-Augmented Medical Agents for Long-Horizon Clinical Decision-Making**
>
> **Author:** Zihan Xu (3220110781)  
> **Supervisor:** Zuozhu Liu  
> **Major:** Computer Engineering  
> **Date:** May 2026

---

## Thesis Scope Note

The original LongMedBench project defined **three task suites**: Fact-based QA (T3-A), Temporal Reasoning (T3-N), and Long-Horizon Decision-Making (T3-D). This thesis focuses **exclusively on the Decision-Making task (T3-D)**. The other two tasks are undertaken by separate collaborators for their respective ST4001 theses. Throughout this outline, the framing is "within the context of the broader LongMedBench project, my contribution focuses on the decision-making dimension."

---

## Structural Overview (Based on `latex_2` Template)

| Section | File | Description |
|---------|------|-------------|
| Front Matter | `main.tex` | Title page, checklist, commitment |
| Acknowledgements | `data/ack.tex` | ~150 words |
| Abstract (EN) | `data/abstract.tex` | ≤300 words |
| Table of Contents | auto-generated | — |
| **Chapter 1** | `data/chap1.tex` | Introduction |
| **Chapter 2** | `data/chap2.tex` | Methodology & Experiments |
| **Chapter 3** | `data/chap3.tex` | Discussion & Conclusion |
| References | `data/ref.tex` or `ref-bib.tex` | GBT7714-numerical format |
| Appendix | `data/appendix.tex` | Supplementary materials |
| Biography | `data/biography.tex` | Author info |

---

## Detailed Chapter Outline

---

### Abstract (≤300 words)

**Structure:**
1. **Background (2-3 sentences):** Clinical practice is inherently longitudinal — clinicians synthesize evidence across repeated visits. Current medical LLM benchmarks focus on short-context QA and tool-use, failing to evaluate long-horizon reasoning.
2. **Method (3-4 sentences):** This thesis introduces the decision-making component of LongMedBench, constructed from MIMIC-IV EHR data (355 patients, 19.72 visits/patient). We design a three-tier memory architecture (Note, Event, Context Memory) and formulate long-horizon decision-making tasks that require the agent to predict the next clinical action given a partial patient trajectory.
3. **Key Finding (2-3 sentences):** Experiments across multiple LLMs (GPT-5-mini, DeepSeek-v3.2, Qwen-Turbo) reveal that (a) structured event-based memory outperforms unstructured note memory; (b) RAG and agent memory systems (Mem0) provide only marginal gains for decision-making; (c) agent performance depends primarily on immediate interaction context, not distant history retrieval.
4. **Implication (1 sentence):** This reveals a fundamental gap in current LLMs' ability to integrate longitudinal clinical information for proactive decision-making, highlighting directions for memory-augmented architectures.

**Keywords:** Medical AI Agents, Clinical Decision-Making, EHR Benchmark, Long-Horizon Reasoning, Memory-Augmented LLM

---

### Chapter 1: Introduction

**Page estimate:** 6–8 pages  
**Purpose:** Establish the problem, survey related work, state contributions.

#### 1.1 Clinical Decision-Making: The Long-Horizon Challenge

- Clinical reasoning is inherently longitudinal — patients accumulate medical histories spanning years and dozens of visits.
- Clinicians must aggregate evidence across diagnostic tests, imaging, medications, and evolving treatment responses.
- Example scenario: a patient with chronic conditions (e.g., kidney disease, diabetes) undergoes repeated hospitalizations — each admission builds on prior knowledge.
- Gap: Most existing medical LLM benchmarks evaluate *single-encounter* behavior (tool-use, diagnosis from one visit), failing to test *cross-visit temporal integration*.

**Key references:** MIMIC-IV overview [PhysioNet-mimiciv-3.1]; clinical reasoning literature [pmid41325597]

#### 1.2 Medical AI Agents and Benchmarks: A Critical Review

*This is the expanded literature review section. Structure as a systematic survey.*

**1.2.1 Evolution of Medical AI Benchmarks**
- Early benchmarks: MedBench [2023], PromptCBLUE [2023], CMB — single-turn QA on curated medical exam questions.
- EHR-based benchmarks: EHRSQL [2023] — text-to-SQL on structured EHR; EHR-R1 [2025] — reasoning-enhanced foundation models.
- Interactive agent benchmarks: MedAgentBench [2025], AgentClinic [2025] — simulated encounters with tool-use evaluation.
- Comprehensive evaluation: MedHelm [2026], HealthBench [2025] — multi-dimensional assessment.

**1.2.2 Limitations of Existing Benchmarks**
- **Short horizon:** Most frameworks evaluate ≤1 encounter; real care spans dozens of visits.
- **Retrieval-focused:** Benchmarks like RULER, LongMemEval test "needle-in-a-haystack" fact retrieval, not temporal reasoning or proactive planning.
- **No decision-making evaluation:** Even interactive benchmarks (MedAgentGym, FHIR-AgentBench) focus on tool-call accuracy, not longitudinal clinical planning.
- **Table 1 (adapted from MICCAI paper):** Systematic comparison of LongMedBench vs. existing benchmarks along dimensions: EHR-based, multi-visit, temporal reasoning, decision-making, memory granularity.

**1.2.3 Memory-Augmented LLM Architectures**
- RAG (Retrieval-Augmented Generation): External retrieval for long-context augmentation.
- Agent memory systems (Mem0): Persistent, evolvable memory for multi-session interactions.
- Lost-in-the-Middle problem [liu2023]: LLMs degrade when relevant information appears in the middle of long contexts.
- Gap identified: Existing memory architectures have not been systematically evaluated for *long-horizon clinical decision-making*.

**Key references for this section:**
- Benchmarks: MedAgentBench [jiang2025], AgentClinic [schmidgall2025], EHRSQL [lee2022], MedHelm [2026], HealthBench [2025], LongMemEval [wu2024], RULER [hsieh2024], TRIP-Bench [shen2026]
- Memory: Mem0 [reference from paper], Lost-in-Middle [liu2023]
- EHR foundations: EHR-R1 [2025], TIMER [2025], ER-Reason [2025]

#### 1.3 Research Gap and Motivation

- Three key gaps:
  1. No benchmark evaluates **proactive, next-step clinical planning** over extended patient histories.
  2. No systematic comparison of **memory granularities** (note-level, event-level, context-level) for clinical decision-making.
  3. Unknown whether existing memory augmentation (RAG, Mem0) actually improves longitudinal decision-making.
- This thesis addresses these gaps through the **decision-making component** of LongMedBench.

#### 1.4 Thesis Contributions and Scope

**Primary contributions (decision-making focused):**
1. **Data Construction:** A reproducible pipeline transforming MIMIC-IV into longitudinal event streams suitable for decision-making evaluation.
2. **Memory Architecture:** Design and implementation of a three-tier memory framework (Note Memory, Event Memory, Contextual Memory) providing controlled granularity for history access.
3. **Task Design:** Formulation of Long-Horizon Decision-Making (T3-D) tasks that require the agent to predict the next clinical action given partial patient trajectories.
4. **Experimental Findings:** Systematic evaluation of LLMs (GPT-5-mini, DeepSeek-v3.2, Qwen-Turbo) and memory strategies (Naive Long-Context, RAG, Mem0), revealing that decision-making performance depends primarily on immediate context, not historical retrieval.

**Scope delineation:**
- This thesis covers T3-D (Decision-Making) only.
- T3-A (Fact-based QA) and T3-N (Temporal Reasoning) are covered in collaborators' theses.
- The broader LongMedBench benchmark framework is collaborative; my contribution is the decision-making methodology, implementation, and analysis.

#### 1.5 Thesis Structure

- Chapter 1: Introduction and literature review (this chapter).
- Chapter 2: Methodology — data pipeline, memory architecture, decision-making task design, experimental setup and results.
- Chapter 3: Discussion, limitations, and conclusion.

---

### Chapter 2: Methodology and Experiments

**Page estimate:** 15–20 pages  
**Purpose:** Detailed technical exposition of data processing, memory architecture, task design, experimental protocol, and results.

#### 2.1 EHR Data Processing Pipeline

**2.1.1 Data Source: MIMIC-IV**
- Overview: >100,000 patients, structured (labs, prescriptions, admissions) + unstructured (radiology reports, discharge notes).
- Rationale for MIMIC-IV: publicly available, rich longitudinal structure, clinical realism.

**2.1.2 Patient Filtering Strategy**
- Retain only abnormal lab results (preserves clinical saliency, reduces noise).
- Require complete admission/discharge records.
- Select patients with ≥15 hospitalizations → 355 patients, 6,999 visits.
- Statistical characterization: mean 19.72 visits/patient (median=18.00, SD=5.72); mean 44.91 medical events/visit (median=25.00, SD=70.32).
- Justification: Dense multi-visit structure needed for longitudinal reasoning evaluation.
- **Acknowledge limitation:** Filtering creates a high-acuity subpopulation; generalizability implications discussed in Chapter 3.

**2.1.3 Event Stream Construction**
- Formal definition of medical event $E^k_i = \{a^k_i, t^k_i, p^k_i, o^k_i\}$:
  - $a^k_i$: action type (imaging, lab test, medication, etc.)
  - $t^k_i$: timestamp
  - $p^k_i$: action parameter (modality for imaging, drug name for medication)
  - $o^k_i$: observation result (radiology report, lab value)
- Visit structure: $\mathcal{V}_i = \{E^{adm}_i, E^1_i, E^2_i, \dots, E^{dis}_i\}$
- Patient stream: $\mathcal{S_P} = \{\mathcal{V}_1, \mathcal{V}_2, \dots, \mathcal{V}_n\}$

**2.1.4 Note Processing**
- Two note types: radiology notes (converted to structured imaging events via regex parsing) and discharge notes (preserved as unstructured text $N_i$, split into $N^{adm}_i$ and $N^{dis}_i$).
- Discharge notes used for ground truth extraction in decision-making tasks.

**2.1.5 Pipeline Reproducibility**
- Standardized extraction, preprocessing, task generation, and evaluation scripts.
- End-to-end reproducible pipeline design.

#### 2.2 Three-Tier Memory Architecture

*This is the core architectural contribution. Expand significantly beyond the paper's brief description.*

**2.2.1 Design Rationale**
- Clinical information exists at multiple granularities: (a) high-level visit summaries, (b) individual medical events, (c) immediate interaction context.
- Different memory granularities serve different decision-making needs: overview for treatment trajectory understanding, event-level for specific test/treatment tracking, context-level for current-state awareness.
- The architecture enables controlled experiments by ablating memory granularity.

**2.2.2 Note Memory (Coarse-Grained)**
- Composition: Admission/discharge summaries ($N^{adm}_i$, $N^{dis}_i$) for each historical visit.
- Format: Natural language text, approximately 200-500 words per visit.
- Strengths: Compact, captures clinical narrative.
- Weaknesses: Lossy summarization; loses fine-grained temporal and event details.

**2.2.3 Event Memory (Fine-Grained)**
- Composition: Structured JSON objects for each medical event ($a^k_i, t^k_i, p^k_i, o^k_i$).
- Format: Time-ordered event stream with typed actions and structured observations.
- Strengths: Complete information preservation, enables precise temporal reasoning.
- Weaknesses: Large context footprint; risk of "lost-in-the-middle" effect.

**2.2.4 Contextual Memory (Immediate)**
- Composition: The current visit's partial event history (events up to the decision point).
- Format: Same as event memory, but limited to the ongoing visit.
- Purpose: Represents the agent's immediate clinical context — what has happened *during this admission*.

**2.2.5 Memory Provision Strategies**
- Three strategies for providing historical memory to the agent:
  1. **Naive Long-Context:** All historical visits' memory (note or event) provided in full; model must attend across entire context.
  2. **RAG (Retrieval-Augmented Generation):** Historical memory indexed; top-k relevant chunks retrieved per query; concatenated with current context.
  3. **Mem0 (Agent Memory System):** Persistent memory store with add/search/update operations; agent actively manages memory across sessions.

#### 2.3 Long-Horizon Decision-Making Task (T3-D)

**2.3.1 Task Formulation**
- **Goal:** Evaluate the agent's ability to make proactive, contextually appropriate clinical decisions based on partial patient trajectories.
- **Setup:** Agent receives (a) current visit's partial event history, (b) historical memory (at controlled granularity), (c) a decision prompt.
- **Task:** Predict the next clinical action that should be taken — from a predefined action space $\mathcal{A}$ (e.g., order imaging, prescribe medication, perform lab test, admit/discharge).
- **Ground truth:** Extracted from actual clinical records (what the real clinician did next).

**2.3.2 Action Space Design**
- Categories: Imaging (CT, X-ray, MRI, Ultrasound), Lab Tests (blood work, cultures), Medications (prescriptions, adjustments), Procedures, Admission/Discharge actions.
- Action granularity: Specific enough to be clinically meaningful, coarse enough for reliable evaluation.
- Ground truth extraction: Parsed from discharge notes and event sequences using regex and structured field extraction.

**2.3.3 Decision Prompt Construction**
- Agent prompt structure:
  1. System prompt: Clinical role definition + task instructions
  2. Current visit context: Partial event stream up to decision point
  3. Historical memory: Note/event/context memory from prior visits
  4. Decision query: "Based on the patient's history and current status, what should the next clinical action be?"

**2.3.4 Evaluation Metrics**
- **Exact Match Accuracy:** Proportion of decisions where predicted action matches ground truth exactly (action type + parameter).
- **Hit-Any Accuracy:** Proportion where predicted action type matches ground truth type (relaxed metric).
- **Per-action-type breakdown:** Accuracy stratified by action category (imaging, labs, medication, etc.).
- **Memory ablation analysis:** Performance comparison across Note Memory vs. Event Memory vs. different history lengths.

**2.3.5 Task Statistics**
- Total decision-making instances: ~9,598 (from the MICCAI paper statistics).
- Distribution of action types.
- Average context length required per decision instance.

#### 2.4 Experimental Setup

**2.4.1 Models Evaluated**
| Model | Context Window | Notes |
|-------|---------------|-------|
| GPT-5-mini | 400K tokens | Best performer overall |
| DeepSeek-v3.2 | 128K tokens | With and without thinking mode |
| Qwen-Turbo | 128K tokens | Baseline for ablation studies |

**2.4.2 Experimental Conditions (Ablation Design)**
1. **Memory Granularity:** Note Memory vs. Event Memory
2. **Memory Architecture:** Naive Long-Context vs. RAG vs. Mem0
3. **History Length:** Visit injection ablation ($n = 0, 1, 2, 3, \infty$ historical visits)
4. **Thinking Mode:** Standard vs. chain-of-thought (DeepSeek-v3.2-thinking)

**2.4.3 Evaluation Protocol**
- Closed-source models: API-based evaluation with standardized prompts.
- Open-source models: Local deployment with inference optimization.
- Statistical significance: Report mean and standard deviation across multiple runs.

#### 2.5 Results and Analysis

**2.5.1 Overall Decision-Making Performance**

| Model | Memory | T3-D Accuracy |
|-------|--------|:-------------:|
| GPT-5-mini | Event | **0.607** |
| GPT-5-mini | Note | **0.607** |
| DeepSeek-v3.2 | Event | 0.607 (no thinking) / 0.593 (thinking) |
| Qwen-Turbo | Event | 0.604 |

> Note: T3-D accuracy ~55-62% across all models, indicating significant room for improvement.

**2.5.2 Impact of Memory Granularity**
- Key finding: Structured event memory *slightly* outperforms note memory for most models, but the difference is minimal for decision-making.
- Interpretation: For proactive clinical planning, the *immediate* context (current visit events) dominates; historical granularity matters less.

**2.5.3 Impact of Memory Architecture**

From ablation study (visit injection + memory architectures):

| Condition | T3-D Accuracy |
|-----------|:-------------:|
| Base ($n=0$, no history) | **0.61** |
| RAG (event memory) | 0.51 |
| Mem0 (event memory) | ~0.53 |
| Full history ($n=\infty$, event) | ~0.60 |

- **Striking finding:** Adding more history via RAG or increasing injected visits *does not improve* and sometimes *degrades* decision-making performance.
- Mem0 and RAG provide *no significant advantage* over the naive long-context baseline for T3-D.
- The base condition (no history, only current visit context) achieves comparable or better accuracy.

**2.5.4 Analysis: Why Memory Augmentation Fails for Decision-Making**
- **Immediate context dominance:** Decision-making tasks require reacting to the *current* clinical situation; distant history provides diminishing returns.
- **Noise from irrelevant history:** Including all historical events introduces noise that distracts from the current decision.
- **Lost-in-the-Middle:** When history is long, relevant past information may be "lost" in the middle of the context [liu2023].
- **Thinking mode penalty:** DeepSeek-v3.2-thinking performs *worse* (0.593 vs. 0.607), likely because thinking makes the agent more conservative and prone to asking questions rather than making decisions.

**2.5.5 Error Analysis**
- **Action type confusion:** Which action categories are most frequently confused?
- **Temporal sensitivity:** Does the agent's accuracy degrade as the decision point moves deeper into a visit?
- **Patient complexity:** Does performance correlate with patient visit count or event density?

**2.5.6 Comparison with Existing Benchmarks**
- Position LongMedBench decision-making results in context of MedAgentBench, AgentClinic, etc.
- Note: Direct comparison is limited because no existing benchmark evaluates the same task type.

#### 2.6 Ablation and Extended Analysis

**2.6.1 Visit Injection Study**
- Systematic variation of $n$ (number of historical visits provided).
- Key result: Performance plateaus or degrades beyond $n=1$ for decision-making.

**2.6.2 Memory Granularity by Model**
- Event vs. Note memory comparison across all models.
- GPT-5-mini: No difference between event and note for T3-D.
- Qwen-Turbo: Slight event advantage.

**2.6.3 Context Length vs. Performance**
- Analysis of whether longer contexts (more history) improve or degrade decision accuracy.
- Lost-in-the-Middle evidence: Accuracy drops when relevant history is embedded in long contexts.

---

### Chapter 3: Discussion and Conclusion

**Page estimate:** 4–5 pages  
**Purpose:** Synthesize findings, acknowledge limitations, propose future directions.

#### 3.1 Summary of Findings

- **Finding 1:** Current LLMs achieve modest decision-making accuracy (55-62%) on the T3-D task, revealing significant headroom.
- **Finding 2:** Structured event memory provides slight advantages over unstructured note memory, but the effect is subtle for decision-making.
- **Finding 3:** Memory augmentation architectures (RAG, Mem0) offer *no significant improvement* for clinical decision-making — immediate context dominates.
- **Finding 4:** Chain-of-thought reasoning can *degrade* decision performance by inducing conservatism.
- **Central insight:** The bottleneck in long-horizon clinical decision-making is not *retrieval* of historical information, but *integration* of historical context into proactive planning.

#### 3.2 Implications for Medical AI Agents

- **For benchmark design:** Decision-making tasks must be explicitly designed to test historical integration, not just retrieval. Need tasks where history *changes* the correct decision.
- **For memory architectures:** Current RAG/agent-memory approaches optimize for retrieval accuracy, but clinical decision-making requires *reasoning over* retrieved information. Future architectures should incorporate structured reasoning layers.
- **For LLM development:** Models need better mechanisms for selectively attending to relevant history while ignoring irrelevant context noise.

#### 3.3 Limitations

- **Data scope:** MIMIC-IV is a single-institution dataset (Beth Israel Deaconess); generalizability to other clinical settings unverified.
- **Patient filtering bias:** The ≥15 hospitalization criterion creates a high-acuity, chronic-disease subpopulation. Findings may not extend to lower-acuity settings.
- **Task simplification:** The decision-making task reduces clinical complexity to action-type prediction; real clinical decisions involve nuanced reasoning about drug dosages, contraindications, and patient preferences.
- **Evaluation metrics:** Exact-match accuracy may penalize clinically reasonable alternatives; future work should incorporate clinical expert validation.
- **Action space coverage:** The defined action space may not capture all clinically relevant actions (e.g., consultations, referrals).
- **Single-modality:** Current benchmark uses text-only representations; real EHR includes imaging, waveforms, and structured ontologies.

#### 3.4 Future Work

1. **Task refinement:** Design decision tasks where historical context *changes* the correct answer (adversarial design), to better stress-test temporal integration.
2. **Memory architecture improvements:** Develop memory systems that support structured reasoning over historical information, not just retrieval — e.g., temporal attention mechanisms, graph-based memory representations.
3. **Multi-modal extension:** Incorporate imaging data, lab time-series, and medication graphs into the decision-making environment.
4. **Human baseline:** Conduct expert clinician evaluation to establish human-level performance on the T3-D task.
5. **Cross-institutional validation:** Extend the pipeline to other EHR databases (e.g., eICU, HIRID) to test generalizability.
6. **Longitudinal outcome correlation:** Study whether better decision-making accuracy correlates with simulated patient outcomes.

#### 3.5 Conclusion

- Recap the motivation: Real clinical care is longitudinal; AI evaluation must reflect this.
- Restate contributions: (1) Data pipeline for longitudinal decision-making evaluation, (2) Three-tier memory architecture enabling controlled granularity experiments, (3) T3-D task design and implementation, (4) Empirical finding that memory augmentation fails to improve decision-making.
- Final message: The path to clinically capable AI agents requires not just better retrieval, but fundamentally better architectures for integrating longitudinal clinical information into proactive decision-making.

---

## References (Preliminary Bibliography)

### Core References (from LongMedBench paper)

1. MIMIC-IV database [PhysioNet-mimiciv-3.1]
2. LongMemEval [wu2024]
3. TRIP-Bench [shen2026]
4. RULER [hsieh2024]
5. Lost-in-the-Middle [liu2023]
6. MedAgentBench [jiang2025]
7. AgentClinic [schmidgall2025]
8. EHRSQL [lee2022]
9. TIMER [2025]
10. ER-Reason [2025]
11. HotpotQA [yang2018]
12. NarrativeQA [kočiský2017]

### Additional References (from ehrBenchmarkSurvey)

13. **MedAlign** [2024] — Clinician-generated dataset for instruction following with EHR
14. **EHR-R1** [2025] — Reasoning-enhanced foundational LM for EHR
15. **FHIR-AgentBench** [2025] — Benchmarking LLM agents for interoperable EHR QA
16. **BRIDGE** [2025] — Benchmarking LLMs for understanding real-world clinical practice text
17. **MedHelm** [2026] — Holistic evaluation of LLMs for medical tasks
18. **MedAgentGym** [2025] — Scalable agentic training for biomedical data science
19. **HIRID-ICU-Benchmark** [2021] — ML benchmark on high-resolution ICU data
20. **EMRQA** [2018] — Large corpus for QA on electronic medical records
21. **PromptCBLUE** [2023] — Chinese prompt tuning benchmark for medical domain
22. **HealthBench** [2025] — Evaluating LLMs towards improved human health
23. **MedBench** [2023] — Large-scale Chinese benchmark for medical LLMs

### Methodology References (to be expanded)

24. RAG architectures (Lewis et al., 2020 + recent advances)
25. Agent memory systems (Mem0, MemGPT, etc.)
26. Clinical decision support systems (historical overview)
27. LLM evaluation methodology (standard references)

---

## Appendix Suggestions

- **Appendix A:** Full action space taxonomy for T3-D
- **Appendix B:** Example decision-making prompts (full text)
- **Appendix C:** Detailed per-model per-condition result tables
- **Appendix D:** Error analysis case studies (qualitative examples)
- **Appendix E:** Data processing scripts architecture overview

---

## Writing Plan and Timeline

| Phase | Dates | Tasks |
|-------|-------|-------|
| **Chapter 1 Draft** | May 11–14 | Expand lit review section (1.2) to ~5 pages; draft all subsections |
| **Chapter 2 Draft** | May 14–17 | Write methodology sections 2.1–2.4; compile all tables/figures |
| **Chapter 2 Results** | May 17–18 | Write results (2.5) and ablation (2.6); integrate with chapter 2 |
| **Chapter 3 Draft** | May 18–19 | Write discussion and conclusion |
| **References** | May 19 | Compile and format bibliography |
| **Abstract + Front Matter** | May 19 | Write abstract; update title page; add acknowledgements |
| **Full Draft Review** | May 20 | Self-review; supervisor feedback |
| **Final Polishing** | May 20–22 | Formatting, proofreading, final compilation |

---

## Notes on Formal Requirements (from template)

- **Font:** Times New Roman, size 12 for body text, double line spacing
- **Chapter titles:** Bold, size 18
- **Section titles:** Bold, size 14
- **Sub-section titles:** Bold, size 12
- **References:** Cited by number in square brackets `[1]`, GBT7714-numerical format
- **Paragraph alignment:** Fully justified
- **Tables/Figures:** Captions bold, size 12, single line spacing, centered
- **Abstract:** ≤300 words, separate page
- **Compilation:** XeLaTeX (`xelatex main.tex`)
