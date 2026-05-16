# Main Statistic
现有问题benchmark任务设定区分度不高。我们先评估是否和准确率的统计相关。排除weight decay，只根据gt事件做匹配：

| Metric           | Value   |
|------------------|---------|
| total            | 47214   |
| with_gt          | 47204   |
| hit_any          | 26121   |
| exact_match      | 14601   |
| hit_any_acc      | 0.553364|
| exact_match_acc  | 0.309317|

| QType | total | hit_any | exact_match | hit_any_acc | exact_match_acc |
| ----- | ----: | ------: | ----------: | ----------: | --------------: |
| T3-N  | 19314 |    6084 |        5913 |    0.315070 |        0.306214 |
| T3-A  | 18302 |   14228 |        2879 |    0.777571 |        0.157340 |
| T3-D  |  9598 |    5809 |        5809 |    0.605356 |        0.605356 |

| Agent      | total | hit_any | exact_match | hit_any_acc | exact_match_acc |
| ---------- | ----: | ------: | ----------: | ----------: | --------------: |
| llm_agent  | 29641 |   16510 |        9270 |    0.556999 |        0.312742 |
| mem0_agent |  8923 |    4900 |        2754 |    0.549759 |        0.308987 |
| rag_agent  |  8650 |    4711 |        2577 |    0.544624 |        0.297919 |

| Memory | total | hit_any | exact_match | hit_any_acc | exact_match_acc |
| ------ | ----: | ------: | ----------: | ----------: | --------------: |
| event  | 23284 |   12868 |        7222 |    0.552773 |        0.310237 |
| note   | 23930 |   13253 |        7379 |    0.553939 |        0.308422 |

| Agent-Memory      | total | hit_any | exact_match | hit_any_acc | exact_match_acc |
| ----------------- | ----: | ------: | ----------: | ----------: | --------------: |
| llm_agent::event  | 14440 |    8054 |        4544 |    0.557756 |        0.314681 |
| llm_agent::note   | 15201 |    8456 |        4726 |    0.556279 |        0.310901 |
| mem0_agent::event |  4519 |    2540 |        1458 |    0.562694 |        0.322995 |
| mem0_agent::note  |  4404 |    2360 |        1296 |    0.536486 |        0.294612 |
| rag_agent::event  |  4325 |    2274 |        1220 |    0.525780 |        0.282081 |
| rag_agent::note   |  4325 |    2437 |        1357 |    0.563468 |        0.313757 |

| Agent-Memory      | QType | total | hit_any | exact_match | hit_any_acc | exact_match_acc |
| ----------------- | ----- | ----: | ------: | ----------: | ----------: | --------------: |
| llm_agent::event  | T3-N  |  5908 |    1870 |        1826 |    0.316520 |        0.309072 |
| llm_agent::event  | T3-A  |  5596 |    4370 |         904 |    0.780915 |        0.161544 |
| llm_agent::event  | T3-D  |  2936 |    1814 |        1814 |    0.617847 |        0.617847 |
| llm_agent::note   | T3-N  |  6216 |    1937 |        1889 |    0.311615 |        0.303893 |
| llm_agent::note   | T3-A  |  5898 |    4619 |         937 |    0.783147 |        0.158867 |
| llm_agent::note   | T3-D  |  3087 |    1900 |        1900 |    0.615484 |        0.615484 |
| mem0_agent::event | T3-N  |  1848 |     601 |         597 |    0.325569 |        0.323402 |
| mem0_agent::event | T3-A  |  1752 |    1375 |         297 |    0.785714 |        0.169714 |
| mem0_agent::event | T3-D  |   919 |     564 |         564 |    0.614379 |        0.614379 |
| mem0_agent::note  | T3-N  |  1802 |     552 |         539 |    0.306667 |        0.299444 |
| mem0_agent::note  | T3-A  |  1706 |    1311 |         260 |    0.769366 |        0.152582 |
| mem0_agent::note  | T3-D  |   896 |     497 |         497 |    0.555307 |        0.555307 |
| rag_agent::event  | T3-N  |  1770 |     543 |         505 |    0.306780 |        0.285311 |
| rag_agent::event  | T3-A  |  1675 |    1241 |         225 |    0.740896 |        0.134328 |
| rag_agent::event  | T3-D  |   880 |     490 |         490 |    0.556818 |        0.556818 |
| rag_agent::note   | T3-N  |  1770 |     581 |         557 |    0.328249 |        0.314689 |
| rag_agent::note   | T3-A  |  1675 |    1312 |         256 |    0.783284 |        0.152836 |
| rag_agent::note   | T3-D  |   880 |     544 |         544 |    0.618182 |        0.618182 |
按 config（完整列表）

| Config                                         | QType | total | hit_any | exact_match | hit_any_acc | exact_match_acc |
| ---------------------------------------------- | ----- | ----: | ------: | ----------: | ----------: | --------------: |
| llm_eval_event_0.0_deepseek-v3.2               | T3-N  |   901 |     283 |         283 |    0.314095 |        0.314095 |
| llm_eval_event_0.0_deepseek-v3.2               | T3-A  |   853 |     685 |         146 |    0.803048 |        0.171161 |
| llm_eval_event_0.0_deepseek-v3.2               | T3-D  |   448 |     278 |         278 |    0.620536 |        0.620536 |
| llm_eval_event_0.0_deepseek-v3.2_9999_thinking | T3-N  |   502 |     138 |         138 |    0.274900 |        0.274900 |
| llm_eval_event_0.0_deepseek-v3.2_9999_thinking | T3-A  |   478 |     363 |          76 |    0.759414 |        0.158996 |
| llm_eval_event_0.0_deepseek-v3.2_9999_thinking | T3-D  |   248 |     149 |         149 |    0.600806 |        0.600806 |
| llm_eval_event_0.0_qwen-turbo                  | T3-N  |   901 |     280 |         271 |    0.310766 |        0.300777 |
| llm_eval_event_0.0_qwen-turbo                  | T3-A  |   853 |     664 |         136 |    0.778429 |        0.159437 |
| llm_eval_event_0.0_qwen-turbo                  | T3-D  |   448 |     278 |         278 |    0.620536 |        0.620536 |
| llm_eval_event_0.0_qwen-turbo_0                | T3-N  |   901 |     296 |         287 |    0.328524 |        0.318535 |
| llm_eval_event_0.0_qwen-turbo_0                | T3-A  |   853 |     665 |         139 |    0.779601 |        0.162954 |
| llm_eval_event_0.0_qwen-turbo_0                | T3-D  |   448 |     278 |         278 |    0.620536 |        0.620536 |
| llm_eval_event_0.0_qwen-turbo_2                | T3-N  |   901 |     294 |         279 |    0.326304 |        0.309656 |
| llm_eval_event_0.0_qwen-turbo_2                | T3-A  |   853 |     656 |         129 |    0.769050 |        0.151231 |
| llm_eval_event_0.0_qwen-turbo_2                | T3-D  |   448 |     276 |         276 |    0.616071 |        0.616071 |
| llm_eval_event_0.0_qwen-turbo_5                | T3-N  |   901 |     283 |         272 |    0.314095 |        0.301887 |
| llm_eval_event_0.0_qwen-turbo_5                | T3-A  |   853 |     660 |         129 |    0.773740 |        0.151231 |
| llm_eval_event_0.0_qwen-turbo_5                | T3-D  |   448 |     277 |         277 |    0.618304 |        0.618304 |
| llm_eval_event_1.0_gpt-5-mini_9999             | T3-N  |   901 |     296 |         296 |    0.328524 |        0.328524 |
| llm_eval_event_1.0_gpt-5-mini_9999             | T3-A  |   853 |     677 |         149 |    0.793669 |        0.174678 |
| llm_eval_event_1.0_gpt-5-mini_9999             | T3-D  |   448 |     278 |         278 |    0.620536 |        0.620536 |
| llm_eval_note_0.0_deepseek-v3.2                | T3-N  |   901 |     243 |         243 |    0.269700 |        0.269700 |
| llm_eval_note_0.0_deepseek-v3.2                | T3-A  |   853 |     673 |         125 |    0.788980 |        0.146542 |
| llm_eval_note_0.0_deepseek-v3.2                | T3-D  |   448 |     278 |         278 |    0.620536 |        0.620536 |
| llm_eval_note_0.0_deepseek-v3.2_9999_thinking  | T3-N  |   420 |     122 |         121 |    0.290476 |        0.288095 |
| llm_eval_note_0.0_deepseek-v3.2_9999_thinking  | T3-A  |   404 |     308 |          62 |    0.762376 |        0.153465 |
| llm_eval_note_0.0_deepseek-v3.2_9999_thinking  | T3-D  |   207 |     123 |         123 |    0.594203 |        0.594203 |
| llm_eval_note_0.0_qwen-turbo                   | T3-N  |  1291 |     413 |         399 |    0.319907 |        0.309063 |
| llm_eval_note_0.0_qwen-turbo                   | T3-A  |  1229 |     971 |         207 |    0.790073 |        0.168430 |
| llm_eval_note_0.0_qwen-turbo                   | T3-D  |   640 |     391 |         391 |    0.610938 |        0.610938 |
| llm_eval_note_0.0_qwen-turbo_0                 | T3-N  |   901 |     285 |         278 |    0.316315 |        0.308546 |
| llm_eval_note_0.0_qwen-turbo_0                 | T3-A  |   853 |     667 |         137 |    0.781946 |        0.160610 |
| llm_eval_note_0.0_qwen-turbo_0                 | T3-D  |   448 |     277 |         277 |    0.618304 |        0.618304 |
| llm_eval_note_0.0_qwen-turbo_2                 | T3-N  |   901 |     286 |         276 |    0.317425 |        0.306326 |
| llm_eval_note_0.0_qwen-turbo_2                 | T3-A  |   853 |     664 |         138 |    0.778429 |        0.161782 |
| llm_eval_note_0.0_qwen-turbo_2                 | T3-D  |   448 |     277 |         277 |    0.618304 |        0.618304 |
| llm_eval_note_0.0_qwen-turbo_5                 | T3-N  |   901 |     286 |         270 |    0.317425 |        0.299667 |
| llm_eval_note_0.0_qwen-turbo_5                 | T3-A  |   853 |     669 |         132 |    0.784291 |        0.154748 |
| llm_eval_note_0.0_qwen-turbo_5                 | T3-D  |   448 |     277 |         277 |    0.618304 |        0.618304 |
| llm_eval_note_1.0_gpt-5-mini_9999              | T3-N  |   901 |     302 |         302 |    0.335183 |        0.335183 |
| llm_eval_note_1.0_gpt-5-mini_9999              | T3-A  |   853 |     667 |         136 |    0.781946 |        0.159437 |
| llm_eval_note_1.0_gpt-5-mini_9999              | T3-D  |   448 |     277 |         277 |    0.618304 |        0.618304 |

![[Pasted image 20260326153611.png]]
# Question Validation

分析有效问题的比例。我们重新使用了Long-Context LLM的完整回答结果（n=2022），排除在所有模型回答中score都是0/1的结果。

| eligible questions | valid count | valid ratio | all zero  | all zero ratio | all one count | all one ratio | remaining |
| ------------------ | ----------- | ----------- | --------- | -------------- | ------------- | ------------- | --------- |
| 2202               | 925         | 0.420073    | 461       | 0.209355       | 816           | 0.370572      | 925       |

| qtype | eligible questions | valid count | valid ratio | all zero | all zero ratio | all one | all one ratio | remaining |
| ----- | ------------------ | ----------- | ----------- | -------- | -------------- | ------- | ------------- | --------- |
| T3-A  | 853                | 289         | 0.338804    | 59       | 0.069168       | 505     | 0.592028      | 289       |
| T3-D  | 448                | **8**       | 0.017857    | 167      | 0.372768       | 273     | 0.609375      | 8         |
| T3-N  | 901                | 628         | 0.697003    | 235      | 0.260821       | 38      | 0.042175      | 628       |
T3-D不合理，我们只保留会产生分歧的有效问题进行讨论，看是否依然匹配我们的结果：
# Cleaned Statistic
| Setting            | questions | weighted_acc |
| ------------------ | --------: | -----------: |
| Original (Full)    |     47204 |     0.434546 |
| Disagreement Only  |     19803 |     0.381975 |
| Δ (After - Before) |    -27401 |    -0.052571 |

| QType | Original Acc | Cleaned Acc |         Δ |
| ----- | -----------: | ----------: | --------: |
| T3-N  |     0.315070 |    0.379469 | +0.064399 |
| T3-A  |     0.470942 |    0.381366 | -0.089576 |
| T3-D  |     0.605565 |    0.597697 | -0.007868 |

| config              | Overall(clean) | Overall(orig) | T3-N(clean) | T3-N(orig) | T3-A(clean) | T3-A(orig) |
| ------------------- | -------------- | ------------- | ----------- | ---------- | ----------- | ---------- |
| deepseek_event      | 0.408          | 0.441         | 0.390       | 0.314      | 0.441       | 0.481      |
| deepseek_think      | 0.342          | 0.410         | 0.312       | 0.275      | 0.399       | 0.452      |
| qwen_event          | 0.380          | 0.440         | 0.379       | 0.311      | 0.376       | 0.482      |
| qwen_event0         | 0.402          | 0.446         | 0.411       | 0.329      | 0.376       | 0.477      |
| qwen_event2         | 0.387          | 0.438         | 0.408       | 0.326      | 0.342       | 0.463      |
| qwen_event5         | 0.384          | 0.438         | 0.390       | 0.314      | 0.367       | 0.475      |
| gpt_event           | 0.415          | 0.447         | 0.411       | 0.329      | 0.419       | 0.481      |
| deepseek_note       | 0.351          | 0.416         | 0.326       | 0.270      | 0.396       | 0.463      |
| deepseek_note_think | 0.336          | 0.418         | 0.320       | 0.290      | 0.363       | 0.461      |
| qwen_note           | 0.395          | 0.442         | 0.389       | 0.320      | 0.402       | 0.483      |
| qwen_note0          | 0.389          | 0.439         | 0.393       | 0.316      | 0.378       | 0.475      |
| qwen_note2          | 0.389          | 0.441         | 0.395       | 0.317      | 0.372       | 0.480      |
| qwen_note5          | 0.390          | 0.441         | 0.395       | 0.317      | 0.376       | 0.479      |
| gpt_note            | 0.407          | 0.445         | 0.420       | 0.335      | 0.376       | 0.469      |

![[Pasted image 20260326155930.png]]
Structured event memory outperforms note memory in most models in decision making. ✅
-> 清洗之后这个现象被略微放大，Δ (event - note) = 0.001166 -> 0.005919，但还是不明显

gpt-5-mini performs best. ❌
-> T3-A top-1: deepseek_event; T3-N top-1: gpt_note.

Increasing the context length makes the agent perform worse. ❌
-> 和之前相比不明显（qwen_event2）

Notably, for deepseek-v3.2, thinking patterns did not significantly improve decision performance. ✅
-> 确实是想多了，这个现象在上下文较短的配置下也出现过

As shown in Table 5, although the difference is subtle, close to that under full memory
conditions, increasing the context length makes the agent perform worse. 
-> T3-N ✅；T3-D❌

The performance differences between Mem0 and RAG are minimal, both close to the baseline performance ❌
-> Rag起到负面效果；Mem0_event表现和基线接近，Mem0_note表现差很多。

## Memory Length $N\in\{0,2,5\}$
T3-N
```json
{  
    "memory_type": "event",  
    "qid": "P000003-V16-T0009-T3-N-0",  
    "qtype": "T3-N",  
    "question": "Which of the next step is most appropriate?",  
    "gt": [  
        "medication"  
    ],  
    "baseline": [  
        "order_labs"  
    ],  
    "baseline_reason": "The patient is undergoing R-ICE chemotherapy and has abnormal lab results, indicating the need for further laboratory testing to monitor for potential complications.",  
    "preds": {  
        "ctx0": [  
            "medication"  
        ],  
        "ctx2": [  
            "medication"  
        ],  
        "ctx5": [  
            "order_labs"  
        ]  
    },  
    "reasons": {  
        "ctx0": "The patient has abnormal lab results and is receiving chemotherapy, so ordering furosemide is necessary to manage potential fluid overload or related complications.",  
        "ctx2": "The patient has abnormal lab results and is receiving chemotherapy, so ordering furosemide is necessary to manage potential fluid overload or related complications.",  
        "ctx5": "The patient is undergoing R-ICE chemotherapy and has abnormal lab results, indicating the need for further laboratory testing to monitor for potential complications."  
    }  
}
```
T3-A
```json
{  
    "memory_type": "event",  
    "qid": "P000003-V19-T0008-T3-M-0",  
    "qtype": "T3-M",  
    "question": "Which 2 medications are most appropriate next?",  
    "gt": [  
        "0.9% Sodium Chloride",  
        "5% Dextrose",  
    ],  
    "baseline": [  
        "0.9% Sodium Chloride",  
        "5% Dextrose"  
    ],  
    "baseline_reason": "Based on the patient's admission record, the most appropriate next medications are 0.9% Sodium Chloride and 5% Dextrose, which are commonly used for hydration and IV fluid support during chemotherapy.",  
    "preds": {  
        "ctx0": [  
            "0.9% Sodium Chloride",  
            "5% Dextrose"  
        ],  
        "ctx2": [  
            "0.9% Sodium Chloride",  
            "Heparin Flush (10 units/ml)"  
        ],  
        "ctx5": [  
            "0.9% Sodium Chloride",  
            "5% Dextrose"  
        ]  
    },  
    "reasons": {  
        "ctx0": "The patient is receiving multiple medications, including IV fluids and chemotherapy agents. Based on the admission record, 0.9% Sodium Chloride and 5% Dextrose are appropriate for hydration and support during chemotherapy.",  
        "ctx2": "The patient is receiving multiple medications as part of his treatment plan, including 0.9% Sodium Chloride and Heparin Flush (10 units/ml) for maintaining IV access and preventing clotting.",  
        "ctx5": "The patient is receiving multiple medications as part of his treatment plan, including 0.9% Sodium Chloride and 5% Dextrose for hydration and support. These are appropriate next steps based on the ongoing treatment regimen."  
    }  
}
```
# Memory Type ($M_N, M_E$)
T3-N
```json
{  
    "agent": "agentic_decision",  
    "qid": "P000002-V15-T0031-T3-N-0",  
    "qtype": "T3-N",  
    "question": "Which of the next step is most appropriate?",  
    "event_pred": [  
        "medication"  
    ],  
    "note_pred": [  
        "order_labs"  
    ],  
    "event_reason": "The patient has abnormal lab results and a positive respiratory culture, requiring targeted antibiotic therapy based on susceptibility results. Therefore, the most appropriate next step is to administer medication.",  
    "note_reason": "The patient has multiple abnormal lab results and a positive respiratory culture, indicating the need for further diagnostic evaluation and targeted treatment. Ordering labs will help guide management.",  
    "gt": [  
        "medication"  
    ],  
}
```
T3-A
```json
{  
    "agent": "agentic_decision",  
    "qid": "P000005-V12-T0013-T3-A-0",  
    "qtype": "T3-A",  
    "question": "Which 3 lab panels are most appropriate next?",  
    "event_pred": [  
        "CBC",  
        "Coagulation Panel",  
        "LFT"  
    ],  
    "note_pred": [  
        "C-Reactive Protein",  
        "CBC",  
        "LFT"  
    ],  
    "event_reason": "The patient has abnormal CBC, LFT, and coagulation parameters, suggesting ongoing infection or inflammation. A Coagulation Panel, LFT, and CBC are the most appropriate next lab panels to further evaluate the patient's condition.",  
    "note_reason": "The patient has abnormal CBC, LFT, and other labs indicating possible infection or inflammation. Next appropriate labs include CBC, LFT, and C-Reactive Protein to assess for ongoing issues.",  
    "gt": [  
        "CBC",  
        "FBS",  
        "LFT"  
    ],  
}
```
在上面两种设定中，Agent看的更多的是现有的病人状态，没有显式的在推理中用到历史记忆，因为Reasoning trace都相似度极高。问题准确率更多与模型本身的医疗知识储备相关，这解释了为什么增加上下文长度不会明显影响结果准确度——因为问题本身与历史visit的相关性不高。
-> The agent performance depends more on the model’s clinical reasoning capacity within the immediate context. ✅
# RAG/Mem0
方差过小是问题设计的原因？如果我们只给相关的内容会做出什么判断
T3-A
```json
{  
    "memory_type": "note",  
    "qid": "P000001-V12-T0002-T3-A-1",  
    "qtype": "T3-A",  
    "question": "Which imaging order is most appropriate next?",  
    "gt": [  
        "XRAY abdomen"  
    ],  
    "baseline": [  
        "XRAY abdomen"  
    ],  
    "baseline_reason": "The patient has bilateral lower extremity edema, which is more consistent with a systemic condition such as liver disease or heart failure rather than a localized issue in the extremities. Therefore, an imaging study of the abdomen would be more appropriate to evaluate for underlying causes such as ascites or liver pathology.",  
    "preds": {  
        "llm_agent": [  
            "XRAY abdomen"  
        ],  
        "mem0_agent": [  
            "CT head"  
        ],  
        "rag_agent": [  
            "CT chest"  
        ]  
    },  
    "reasons": {  
        "llm_agent": "The patient has bilateral lower extremity edema, which is more consistent with a systemic condition such as liver disease or heart failure rather than a localized issue in the extremities. Therefore, an imaging study of the abdomen would be more appropriate to evaluate for underlying causes such as ascites or liver pathology.",  
        "mem0_agent": "Patient has a reported headache during this admission and a noncontrast CT head is the most appropriate initial, rapid study to evaluate acute intracranial causes. Other abdominal and chest imaging have recently been performed and are less relevant to the new symptom.",  
        "rag_agent": "The patient has chest pain with a limited/negative chest x‑ray and a nondiagnostic/limited lower‑extremity Doppler; the next appropriate study to evaluate for pulmonary embolism or other intrathoracic causes is a CT chest."  
    },  
}
```
即使在检索返回信息中体现了显式的时间戳，这些历史信息还是有可能误导Agent。尤其是Note的情况下，agent会把返回的note和现有的笔记混淆。这解释了Rag/Mem0会带来更低回答准确度。

T3-N
```json
{  
    "memory_type": "note",  
    "qid": "P000001-V20-T0002-T3-N-0",  
    "qtype": "T3-N",  
    "question": "Which of the next step is most appropriate?",  
    "gt": [  
        "order_labs"  
    ],  
    "baseline": [  
        "order_imaging"  
    ],  
    "baseline_reason": "The patient has a history of HCC and cirrhosis, and the current presentation includes right upper quadrant pain and abdominal distention. An ultrasound of the right upper quadrant is needed to evaluate for possible complications.",  
    "preds": {  
        "llm_agent": [  
            "order_imaging"  
        ],  
        "mem0_agent": [  
            "order_labs"  
        ],  
        "rag_agent": [  
            "order_labs"  
        ]  
    },  
    "reasons": {  
        "llm_agent": "The patient has a history of HCC and cirrhosis, and the current presentation includes right upper quadrant pain and abdominal distention. An ultrasound of the right upper quadrant is needed to evaluate for possible complications.",  
        "mem0_agent": "RUQ ultrasound excluded biliary obstruction but the patient has cirrhosis, ascites, and was being evaluated for infection; laboratory studies (CBC, CMP, LFTs, coagulation) are needed to assess for leukocytosis, renal/hepatic function, and guide further management.",  
        "rag_agent": "Right upper quadrant ultrasound excluded biliary obstruction/choledocholithiasis. The patient with cirrhosis, splenomegaly, and perihepatic ascites needs laboratory evaluation (CBC, CMP/LFTs, coagulation, blood cultures as indicated) to assess for infection, hepatic decompensation, and guide need for paracentesis or other interventions."  
    },  
}
```
这里体现出了历史信息对回答的辅助作用。
## Note vs Event
```json
{  
    "agent": "mem0_agent",  
    "qid": "P000007-V11-T0033-T3-N-0",  
    "qtype": "T3-N",  
    "question": "Which of the next step is most appropriate?",  
    "event_pred": [  
        "order_labs"  
    ],  
    "note_pred": [  
        "medication"  
    ],  
    "event_reason": "The patient has persistent severe electrolyte abnormalities (hypokalemia, hypocalcemia, hypomagnesemia), metabolic acidosis and rising creatinine despite ongoing replacement—repeating labs/ABG and electrolytes is the next appropriate step to assess response and guide further therapy.",  
    "note_reason": "The patient has a stool culture positive for Salmonella species and ongoing severe electrolyte abnormalities (hypokalemia, hypomagnesemia, hypocalcemia, metabolic acidosis) requiring continued IV/PO repletion and initiation/adjustment of targeted antimicrobial therapy in an immunocompromised host. Thus the immediate next step is medication management.",  
    "baseline": {  
        "event": [  
            "order_labs"  
        ],  
        "note": [  
            "order_labs"  
        ]  
    },  
    "baseline_reason": {  
        "event": "The patient has a fecal culture positive for Salmonella species, and there are ongoing electrolyte abnormalities and metabolic acidosis. The most appropriate next step is to order additional labs to monitor the patient's condition and guide treatment.",  
        "note": "The patient has a fecal culture positive for Salmonella species, indicating an infectious cause of his symptoms. Additional labs are needed to monitor electrolyte imbalances and guide treatment."  
    },  
    "gt": [  
        "order_labs"  
    ],  
    "event_hit_gt": true,  
    "note_hit_gt": false,  
    "event_file": "agents/mem0_agent/agentic_decision/results/P000007.event.20260218-210633-338e2d15.mem0.pred.jsonl",  
    "note_file": "agents/mem0_agent/agentic_decision/results/P000007.note.20260218-201837-4c4441bb.mem0.pred.jsonl"  
}
```

# 类似的prediction相关工作
## AGENTEHR: Advancing Autonomous Clinical Decision-Making via Retrospective Summarization
![[Pasted image 20260326165952.png]]
3.3 Evolving Strategy
### Experience Generation
For each training instance, the agent generates a full interaction trajectory $H_K$, a final retrospective summary $S_{\text{final}}$, and a predicted answer set $Y$. By comparing these outputs against the ground truth $Y^*$, we derive domain-specific experiences using the reflection module $R_\theta$, which shares parameters with the agent’s policy:
$E_{\text{act}} = R_\theta(H_K, Y, Y^*)$ 
$E_{\text{sum}} = R_\theta(H_K, S_{\text{final}}, Y, Y^*)$ 
Here, $R_\theta$ is prompted to extract procedural heuristics for the Actor (e.g., optimal tool selection) and information salience guidelines for the Summarizer (e.g., critical evidence retained vs. noise compressed).
### Memory Construction
We organize these insights into an Experience Memory Bank $B$. Each entry is stored as a triplet:
$B = \{(e, E_{\text{act}}, E_{\text{sum}})_i\}_{i=1}^{M}$ 
where $M$ is the size of the memory and $e$ is the representation of the EHR. $e$ is obtained by encoding the most recent clinical events from each patient’s EHR tables using a pre-trained encoder. This ensures that the retrieved experience is contextually aligned with the patient’s current status.
### Evolved Inference

During inference, for a new patient context, we compute its embedding and retrieve the most similar memory items from $B$. These retrieved experiences are injected into the framework to guide both modules:
$S_i = M_\theta(H_{\text{rec}}, S_{i-w}, H_{\text{dist}}, X, E_{\text{sum}})$ 
$a_{i+1} \sim \pi_\theta(a_{i+1} \mid \hat{H}_i, X, E_{\text{act}})$ 
By explicitly conditioning on the experience, the summarizer improves its ability to filter noise, and the actor adopts proven reasoning strategies, thereby enhancing overall robustness.
![[Pasted image 20260326170017.png]]
![[Pasted image 20260326171009.png]]
### MIMIC-IV Construction
For each clinical task, we first identify valid event occurrences for each patient. We define a sample instance by setting the prediction timestamp $t$ to one minute prior to the event time and identifying the ground-truth label set $Y^*$. Subsequently, we temporally censor the patient’s history by removing any clinical observations recorded after the prediction time $t$.

To ensure the benchmark covers both prevalent conditions and long-tail rare cases, we implement a **Label-wise Weighted Sampling** strategy. We calculate a sampling weight $w_S$ for each candidate sample $S$, inversely proportional to the frequency of its constituent labels. Formally, for a sample $S$ with a ground-truth label set $Y^* = \{y_1, y_2, \ldots, y_N\}$, the weight is defined as:
$w_S = \frac{1}{|Y^*|} \sum_{y \in Y^*} \frac{1}{\text{Count}(y)}$ 
where $\text{Count}(y)$ represents the global frequency of label entity $y$ within the specific task type.

We then compute the mean weight $\bar{w}$ across all candidates to stratify the dataset. Samples with $w_S \le \bar{w}$ are categorized into the **Common pool** (high-frequency labels), while those with $w_S > \bar{w}$ are assigned to the **Rare pool** (low-frequency labels).

Within each pool, we perform weighted random sampling using $w_S$ as the probability distribution. This ensures that even within the Common or Rare subsets, the selected samples are uniformly distributed across the label space, maximizing diversity.

Finally, we sample 600 samples for each type of task and filter out samples lacking admission records to ensure data completeness, resulting in the final **MIMIC-IV-Common** and **MIMIC-IV-Rare** benchmarks.