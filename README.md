# [SoulChat-R1: SST-based Multi-turn Psychological Counseling CoT Dataset](https://github.com/scutcyr/SoulChat-R1)

<p align="center">
    <a href="https://arxiv.org/pdf/2412.13660"><img src="https://img.shields.io/badge/Paper-PDF-red.svg"></a>
    <a href="./LICENSE"><img src="https://img.shields.io/badge/license-Apache%202-red.svg"></a>
    <a href="support os"><img src="https://img.shields.io/badge/os-linux%2C%20win%2C%20mac-pink.svg"></a>
    <a href="https://github.com/scutcyr/SoulChat-R1/graphs/contributors"><img src="https://img.shields.io/github/contributors/scutcyr/SoulChat-R1?color=9ea"></a>
    <a href="https://github.com/scutcyr/SoulChat-R1/commits"><img src="https://img.shields.io/github/commit-activity/m/scutcyr/SoulChat-R1?color=3af"></a>
    <a href="https://github.com/scutcyr/SoulChat-R1/issues"><img src="https://img.shields.io/github/issues/scutcyr/SoulChat-R1?color=9cc"></a>
    <a href="https://github.com/scutcyr/SoulChat-R1/stargazers"><img src="https://img.shields.io/github/stars/scutcyr/SoulChat-R1?color=ccf"></a>
</p>

\[ [中文](./README_zh.md) | English \]

## Recent Updates
- 👏🏻 2025.09.28: Now, you can download the **SoulChat-R1** dataset from：[https://www.modelscope.cn/datasets/YIRONGCHEN/SoulChat-R1](https://www.modelscope.cn/datasets/YIRONGCHEN/SoulChat-R1)!
- 👏🏻 2025.09.04: We are pleased to announce that our work: [CATCH: A Novel Data Synthesis Framework for High Therapy Fidelity and Memory-Driven Planning Chain of Thought in AI Counseling](https://arxiv.org/pdf/2412.13660) <!-- github link to be updated --> has been accepted by EMNLP 2025 Findings! 🎉

## Introduction

Since the release of [SoulChat2.0](https://github.com/scutcyr/SoulChat2.0) in December 2024, we have continuously explored language patterns and intervention mechanisms in real-world counseling scenarios, achieving significant progress in enhancing the therapy fidelity and logical coherence of AI counseling.

Despite the rapid development of LLM-based psychological counseling research in recent years, with works such as SMILE, CPsyCoun, Cactus, and PsyDT advancing the field through data synthesis, these methods generally adopt a "one-time generation" approach to construct multi-turn dialogue data. This generation strategy lacks fine-grained control over each therapeutic stage during data synthesis, making it difficult to ensure that the synthesized data aligns with therapy protocols at every stage, resulting in **low therapy fidelity**. Furthermore, existing synthesized data largely **lacks explicit chain-of-thought (CoT)**, failing to reveal the decision-making rationale and reasoning process behind each counselor response. Models trained on such data often merely imitate surface-level conversational patterns, unable to deeply understand and master the underlying therapeutic logic.

These limitations cause AI counseling models to be prone to **therapy drift** during actual conversations—where the dialogue gradually deviates from the intended therapeutic trajectory—compromising intervention effectiveness and professional credibility. To systematically address these issues, the Future Technology College at South China University of Technology and the Guangdong Provincial Key Laboratory of Human Digital Twin, building upon their prior work, propose the CATCH framework, aiming to construct a high-fidelity, explainable AI counseling data and model system.

## Methodology

**CATCH** is an innovative data synthesis framework, whose core lies in solving two key challenges in AI counseling: **low therapy fidelity** and **opaque decision logic**. To this end, CATCH consists of two complementary components:

1.  **Progressive Dialogue Synthesis (PDS)**: Enhances the **therapy fidelity** of dialogues.
2.  **Memory-driven Dynamic Planning Chain-of-Thought (MDP CoT)**: Reveals the **decision-making rationale** behind each response.

### (1) Progressive Dialogue Synthesis (PDS)

To overcome the issues of disordered therapeutic processes and low fidelity caused by the existing "one-time generation" method, CATCH proposes the **Progressive Dialogue Synthesis (PDS)** strategy. PDS strictly adheres to the theoretical framework of **Single-Session Therapy (SST)**, decomposing the counseling process into three structured stages: **Goal Identification**, **Working**, and **Ending**. The dialogue content is not generated all at once but is synthesized **incrementally, stage by stage**, ensuring that each interaction precisely aligns with the established therapeutic protocol.

The PDS process begins with authentic client self-reports. First, a Large Language Model (LLM) extracts and generates the core therapeutic elements required for each stage from the self-report, forming a structured **dialogue outline**:

*   **Counseling Goal**: Based on the client's self-report, generate a goal that is unique, specific, feasible, positive, evaluable, and focused on internal psychological processes.
*   **Resources**: In conjunction with the counseling goal, systematically explore and generate resources, including the client's internal strengths (e.g., past coping experiences) and external supports (e.g., social relationships).
*   **Solution**: Identify "positive resources" from the generated resources that can effectively support goal attainment, and transform them into a specific, feasible, and independently executable action plan for the client.

This outline, combined with the **Big Five personality traits** extracted from the self-report, guides the dialogue generation. The LLM generates the dialogue for each stage according to the outline. The dialogue for subsequent stages is strictly built upon the output of the previous stage, ultimately stitching together a logically coherent, structurally complete, and high-fidelity multi-turn counseling dialogue.

#### Detailed Explanation of the Three Stages of SST

| Stage | Core Tasks | Key Steps |
| :--- | :--- | :--- |
| **Goal Identification Stage** | Establish a trusting relationship and collaboratively confirm the core counseling goal. | 1.  **Building Alliance**: Establish a safe therapeutic relationship through empathy and unconditional positive regard.<br>2.  **Focusing on the Core**: Guide the client to identify the most urgent and representative problem from a range of concerns.<br>3.  **Confirming the Goal**: Collaboratively negotiate and confirm a clear, psychology-focused goal that meets SST standards. |
| **Working Stage** | Explore resources and transform positive resources into a concrete solution. | 1.  **Exploring Resources**: Systematically uncover the client's internal (e.g., personal qualities, successful experiences) and external resources (e.g., social support).<br>2.  **Transforming into a Solution**: Use techniques like behavioral rehearsal to transform the identified "positive resources" into an actionable plan.<br>3.  **Preparing for Transition**: When the client shows willingness to act, prepare to move to the ending stage. |
| **Ending Stage** | Summarize, provide feedback, reinforce confidence, and assign follow-up actions. | 1.  **Feedback & Summary**: Affirm the client's efforts and progress, reinforcing their positive experiences.<br>2.  **Assigning Homework**: Based on the solution, assign specific "homework" or action plans to consolidate gains.<br>3.  **Closing**: Ensure the session feels complete and constructive, enhancing the client's confidence for facing future challenges. |

<p align="center">
    <img src="./figure/PDS _Pipeline.png" width=700px/>
</p>

### (2) Memory-driven Dynamic Planning Chain-of-Thought (MDP CoT)

To address the lack of decision logic transparency in synthesized data, CATCH introduces the **Memory-driven Dynamic Planning (MDP)** thinking pattern and constructs the **MDP-CoT** to systematically reveal the complete reasoning process behind each response.

MDP simulates the cognitive process of a professional counselor conducting **case conceptualization**, comprising three progressive steps:

*   **Memory Enhancement**: Reviews and integrates the dialogue history to construct a continuously updated, structured summary of the client's background, emotional state, and goal progress.
*   **Global Planning**: Assesses the current progress, identifies the precise therapeutic stage, and plans the next steps to ensure that short-term interactions serve the long-term goal.
*   **Strategy Reasoning**: Synthesizes the information from memory and planning to select the optimal response strategy for the current turn, including specific therapeutic techniques and expression styles.

Based on this, we designed a **collaborative multi-agent framework** to generate high-quality MDP-CoT:

*   **Memory Capture Agent**: Analyzes the dialogue history to generate a "current state summary" of the client's personal situation, resource status, and counseling goal.
*   **Global Plan Agent**: Determines the exact stage of the session and plans the direction for the next response (maintain, shift step, or transition stage).
*   **Strategy Reasoning Agent**: Analyzes the client's latest statement to reason about the most appropriate response strategy (technique, tone, length).
*   **Checking Agent**: Rigorously verifies each generated step for consistency, logical coherence, and alignment with the final response. If any deviation is detected, it triggers a regeneration mechanism to ensure quality.
*   **Fusion Agent**: Integrates the verified outputs from the three agents into a fluent, natural, first-person inner monologue, forming the final **MDP-CoT**.

<p align="center">
    <img src="./figure/MDP_CoT_Synthesis_Pipeline.png" width=800px/>
</p>

## Experiments

### Evaluation of the PDS Method

To verify the effectiveness of **Progressive Dialogue Synthesis (PDS)** in enhancing therapy fidelity, we compared it with the **one-time generation method**. Using the same LLM and SST knowledge, we generated two sets of dialogues for 60 counseling cases and invited experts to conduct a blind evaluation.

The evaluation covered the following six dimensions, as shown in the table below:

| Dimension | Abbreviation | Focus |
|------|------|----------|
| **Counselor's Coherent Guidance** | CCG | Whether the intervention path is logically progressive, with natural transitions and consistent goals across stages |
| **Goal Identification Stage Performance** | GIS | Whether a trusting relationship is established and a specific, measurable psychological goal meeting SST standards is collaboratively identified |
| **Working Stage Performance** | WS | Whether internal and external resources are systematically explored and effectively guided toward a feasible action plan |
| **Ending Stage Performance** | ES | Whether an appropriate summary is provided, positive feedback is given, and follow-up tasks consistent with the solution are assigned |
| **Dialogue Fluency** | DF | Whether the language is natural and coherent, without repetition, jumps, or semantic breaks |
| **Topic Relevance** | TR | Whether the content remains focused on the core concern, avoiding irrelevant digressions |

<p align="center">
    <img src="./figure/PDS_Evaluation_Result.png" width="500" alt="Expert evaluation results of PDS vs One-time Generation">
</p>

**Results**: PDS-generated dialogues significantly outperformed the one-time generation method on all therapeutic stages (GIS, WS, ES) and counselor guidance coherence (CCG). For instance, PDS received a **78%** expert preference in the GIS stage and a high of **95%** in the WS stage. Moreover, PDS also demonstrated superior stability in DF and TR, indicating that it not only enhances the structural integrity of the therapy but also improves semantic consistency and topic focus.

---

### Overall Model Performance Evaluation

We performed supervised fine-tuning on the **Qwen3-8B** and **Qwen3-14B** base models using the high-quality multi-turn dialogue data (with explicit MDP CoT) generated by the CATCH framework, resulting in the **CATCH-8B** and **CATCH-14B** models. We compared them with advanced models such as GPT-4o, DeepSeek-V3, and DeepSeek-R1 to comprehensively evaluate their performance in Single-Session Therapy (SST).

#### Evaluation Methodology

The evaluation adopted a **dialogue simulation** protocol, following the [COUNSELINGEVAL](https://aclanthology.org/2024.findings-emnlp.832/) paradigm:

1.  **Test Set**: Used the standardized COUNSELINGEVAL test set, containing 150 client profiles with three predefined attitude types (**positive**, **neutral**, **negative**) to assess model robustness.
2.  **Dialogue Simulation**: Employed a **dual-model simulation**:
    *   **Client**: Played by GPT-4o-mini, generating natural, coherent responses based on the profile and attitude.
    *   **Counselor**: Played by the model under evaluation. All models were provided with the same SST knowledge to ensure fairness.
3.  **Evaluator**: Used **GPT-4o** as an automated evaluator (AI evaluator) to score 450 complete dialogues (150 cases × 3 attitudes).
4.  **Reliability Validation**: Randomly selected 60 samples for human expert scoring using the same rubric. The results showed a significant positive correlation between GPT-4o and human expert ratings.

The evaluation was conducted across two dimensions: **SST-specific skills** and **general counseling skills**.

**1. SST-Specific Skills Evaluation**

-   **Solution Focus (SF)**: Measures the counselor's ability to guide the client from problem description toward solution-building, hope, and positive experiences, reflecting the model's capability in cognitive restructuring and positive guidance.
-   **Resource Activation (RA)**: Assesses the counselor's effectiveness in identifying and activating the client's internal (e.g., emotion regulation, past coping) and external resources (e.g., social support), and transforming them into actionable strategies.
-   **Goal Orientation (GO)**: Evaluates the counselor's ability to help the client set clear, measurable, and executable psychological goals, and to consistently track progress and adjust plans throughout the dialogue, reflecting the structured level and logical coherence of the intervention.

**2. General Counseling Skills Evaluation**

-   **Understanding**: Assesses the counselor's ability to accurately comprehend the client's statements, emotional state, and core concerns.
-   **Interpersonal Effectiveness**: Measures the counselor's performance in building trust, expressing empathy, and maintaining a positive interaction.
-   **Collaboration**: Evaluates whether the counselor can establish a collaborative relationship with the client, jointly exploring problems and solutions rather than providing one-sided guidance.

<p align="center">
    <img src="./figure/Experiment_Result.png" width="900" alt="Performance comparison of CATCH models vs baseline models">
</p>

**Results**:
1.  **Significant Improvement in SST Core Skills**  
    CATCH models showed outstanding performance in **Resource Activation (RA)** and **Goal Orientation (GO)**:
    - CATCH-8B: RA ↑ **32.9%** (3.98 → 5.29), GO ↑ **50.9%** (2.85 → 4.30)
    - CATCH-14B: RA ↑ **31.4%** (4.08 → 5.36), GO ↑ **45.0%** (3.02 → 4.38)
    - CATCH outperformed DeepSeek-R1 and GPT-4o on all SST metrics, demonstrating that **PDS + MDP-CoT** effectively enhances the model's understanding and execution of therapeutic logic.

2.  **Leading Performance in Solution Focus**  
    CATCH also surpassed the baselines in SF (CATCH-8B: 4.91 > Qwen3-8B: 4.47). Although the improvement was smaller, combined with its significant advantages in RA and GO, this indicates its core strength lies in **deep resource exploration** and **long-term goal consistency**.

3.  **Competitive General Skills**  
    CATCH performed robustly in understanding, interpersonal effectiveness, and collaboration, performing on par with GPT-4o and DeepSeek-R1 (e.g., CATCH-14B collaboration reached 4.99), showing that it maintains naturalness and empathy while enhancing its professional capabilities.

---

### Ablation Study

To disentangle the contributions of **PDS** and **MDP-CoT**, we conducted an ablation study, comparing the following settings:
- `w/o PDS and MDP`: Baseline model (standard one-time generation data)
- `w/o MDP`: Using PDS data, but without MDP CoT
- `CATCH-8B/14B`: Full CATCH framework data (PDS + MDP-CoT)

<p align="center">
    <img src="./figure/Ablation_Result.png" width="900" alt="Ablation study results">
</p>

**Results**:
1.  **Core Role of PDS**  
    Simply introducing PDS led to a significant performance boost (e.g., CATCH-8B RA from 3.98 → 5.09, GO from 2.85 → 4.16). This proves that **structured, stage-by-stage data generation** is key to enhancing therapy fidelity, providing a more reliable learning signal for the model.

2.  **Key Gain from MDP-CoT**  
    Adding MDP-CoT on top of PDS further improved performance (e.g., CATCH-8B RA from 5.09 → 5.29). This validates that **explicit chain-of-thought** is crucial for the model to master deep therapeutic logic—it enables the model to not only mimic "what to say" but also understand "why to say it."

3.  **Robustness Analysis**  
    The baseline model's performance significantly declined when facing **negative-attitude** clients, exposing its vulnerability to **therapy drift**. In contrast, the CATCH model maintained high performance across all attitudes, with a particularly noticeable advantage in negative scenarios, demonstrating that the framework effectively enhances the model's **robustness and anti-interference capability** in complex, real-world situations.

<p align="center">
    <img src="./figure/Attitude_Analysis.png" width="600" alt="Model performance under different client attitudes">
</p>

## Dataset

We are pleased to announce that the high-quality AI counseling dataset constructed through the **CATCH** framework is now open-sourced! This dataset not only contains multi-turn structured dialogues that follow standard therapy protocols but also innovatively provides an explicit decision-reasoning chain (MDP CoT) for each counselor response. Our goal is to transform the "tacit knowledge" of professional counselors into "explicit knowledge" that can be learned, providing researchers in the field of AI counseling with valuable training data rich in deep therapeutic logic.

### Dataset Overview

This dataset aims to address the low therapy fidelity and lack of decision transparency in existing synthetic data. Its core consists of two parts, jointly simulating the entire professional counseling process:

1.  **`catch_dialogue.json`**: **Structured Counseling Dialogues**.
    *   Strictly follows the three-stage process of **Single-Session Therapy (SST)**: **Goal Identification**, **Working**, and **Ending**.
    *   Each counselor response is annotated with its corresponding **counseling stage**.

2.  **`catch_mdpcot.json`**: **Explicit Chain-of-Thought (MDP CoT)**.
    *   Reveals the complete thought process of the counselor before generating each response, including the three key steps of **Memory Enhancement**, **Global Planning**, and **Strategy Reasoning**.
    *   Clearly separates "thinking" from "response" using the `<think>...</think>` tags.

### Data Format

#### 1. Structured Counseling Dialogues (`catch_dialogue.json`)

```json
{
    "dialogue_id": "100596267",
    "consultation_target": "Help the client attempt to identify and express their true emotional needs, enhancing their experience of safety and trust in intimate relationships.",
    "conversation": [
        {
            "role": "client",
            "content": "I've been feeling kind of strange lately. I want to talk to someone, but I don’t know where to start."
        },
        {
            "role": "counselor",
            "content": "That’s okay. Would you like to start by talking about what makes you feel “strange” lately? I’m here to listen, no rush.",
            "stage": "target"
        },
        // ... more dialogue turns
        {
            "role": "counselor",
            "content": "We’ve talked about a lot today—you mentioned how past experiences have continued to affect you... I believe in you—and I’ll continue to support you!",
            "stage": "ending"
        }
    ]
}
```

**Field Description:**

| Field Name | Type | Description |
| :--- | :--- | :--- |
| `dialogue_id` | string | A unique identifier for the dialogue. |
| `consultation_target` | string | The core goal of this counseling session. Extracted by the PDS strategy from the client's self-report, meeting SST standards of "specific, feasible, positive," etc. |
| `conversation` | list[dict] | A list of dialogue turns in chronological order. |
| `role` | string | The speaker's role: `"client"` or `"counselor"`. |
| `content` | string | The specific content of the utterance. |
| `stage` | string | **Only present for counselor utterances**. Annotates the counseling stage of the current turn: `"target"` (Goal Identification), `"working"` (Working), or `"ending"` (Ending). |

#### 2. Explicit Chain-of-Thought (`catch_mdpcot.json`)

Each entry in this file corresponds to one counselor response in `catch_dialogue.json`. The `output` field contains the MDP CoT enclosed in `<think>...</think>`.

**Goal Identification Stage Example**

```json
{
    "instruction": "其实这种感觉有点久了，但每次冒出来，我都不太愿意去想。可能是觉得没必要吧，有时候甚至觉得在浪费时间。",
    "system": "# 【一次单元咨询疗法（Single-Session Therapy，SST）指南手册...",
    "input": "",
    "output": "<think>\n我先看看咨询进展。来访者近期状态奇怪，感觉日子空虚，做什么都提不起劲，而且这种空虚感存在较久，但他不愿去想。目前，我暂未探索到其内部资源和外部资源，本次咨询目标也未明确。\n\n基于这些咨询进展，我来分析下咨询阶段。根据提供的历史对话和我的思考内容，来访者目前仍未明确咨询目标，当前还处于目标确认阶段。因为目前还未完成目标确认阶段的步骤，所以不应转换到其他阶段。下一步，我应保持在目标确认阶段，继续与来访者沟通交流，建立更紧密的关系，以此推动明确咨询目标。\n\n结合前面的咨询进展和阶段分析，再看看当前轮回复。来访者说这种感觉有点久了，但每次冒出来，他都不太愿意去想，还觉得没必要，甚至觉得是在浪费时间。这体现出来访者对面对该问题的抗拒。在目标确认阶段，我需要处理阻抗，通过接纳阻抗的策略，引导来访者表达抗拒情绪，进一步了解其内心想法，从而更好地明确咨询目标。所以我选择接纳阻抗作为回复策略，语气上要保持温和理解，运用共情的SST疗法技巧，用询问的表达方式简洁明了地进行回应。 \n</think>\n听起来你在面对这些感受的时候，可能会有点抗拒，像是不想让它占据你太多的精力或时间。这种抗拒会让你觉得轻松一些，还是会让内心更复杂？",
    "history": [
        [
            "我最近状态挺奇怪的，有点想找人聊一聊，但又不知道从哪里开始。",
            "嗯，那没关系，你愿意从什么让你最近觉得自己“状态奇怪”这一部分入手聊聊吗？我在这里倾听，不着急。"
        ],
        [
            "可能是我总觉得日子过得有点空虚吧，好像做什么都提不起劲，也不知道自己怎么了。",
            "听起来内心有些困惑，像是对现在的生活少了动力或方向。如果可以的话，能说说这个“空虚”的感受对你来说是怎样的吗？"
        ]
    ]
}
```

In the goal identification stage, when faced with the client's avoidance of emotions ("it's unnecessary," "wasting time"), the counselor does not forcefully push forward. Instead, they precisely identify the client's psychological resistance and use an open-ended question to probe the emotional cost of this avoidance ("does it make your inner world more complicated?"), guiding the dialogue from problem description towards a deeper exploration of underlying needs.

**Working Stage Example**

```json
{
    "instruction": "有的，比如说读一些关于自我成长的书籍，还试过冥想和瑜伽什么的，但效果不太持久...",
    "system": "# 【一次单元咨询疗法（Single-Session Therapy，SST）指南手册】...",
    "input": "",
    "output": "<think>\n我先梳理一下咨询进展。来访者小时候经历过家暴，这让他变得更封闭，在亲密关系中不敢完全信任对方，也不知如何打开自己。他尝试过读自我成长的书籍、冥想和瑜伽这些内部资源，但效果不持久，且积极性不高，外部资源目前也暂未探索得到。本次咨询目标是帮助来访者尝试识别并表达内心真实的情感需求，增强对亲密关系中安全感和信任的体验。\n\n基于以上咨询进展，来分析一下咨询阶段。当前对话已进入工作阶段，在寻找积极资源步骤中，内部资源方面来访者尝试的读自我成长书籍、冥想和瑜伽效果不佳，外部资源也没找到。虽然已经明确本次咨询目标，但还没挖掘到对实现该目标有积极作用的资源。所以，下一步我应该继续挖掘积极资源，询问来访者之前尝试的方式在某些方面的帮助，看能不能进一步激活内部资源。\n\n再结合前面的情况，看看当前轮回复。来访者提到之前尝试的方法效果不持久，还感觉没办法真正触及到内心深处。现在处于工作阶段的寻找积极资源步骤，为了进一步激活内部资源，我得从他尝试过的方法入手，引导他思考这些方法即便效果不持久，可能仍存在积极影响，以此挖掘潜在积极资源。 \n</think>\n你会特别提到冥想、瑜伽和阅读书籍，说明这些方式某种程度上吸引了你。它们有没有哪一方面帮助到你呢？比如你觉得稍微舒缓了情绪或者更清楚自己的某些触动？",
    "history": [
        [
            "我最近状态挺奇怪的，有点想找人聊一聊，但又不知道从哪里开始。",
            "嗯，那没关系，你愿意从什么让你最近觉得自己“状态奇怪”这一部分入手聊聊吗？我在这里倾听，不着急。"
        ],
        [
            "可能是我总觉得日子过得有点空虚吧，好像做什么都提不起劲，也不知道自己怎么了。",
            "听起来内心有些困惑，像是对现在的生活少了动力或方向。如果可以的话，能说说这个“空虚”的感受对你来说是怎样的吗？"
        ],
        [
            "其实这种感觉有点久了，但每次冒出来，我都不太愿意去想。可能是觉得没必要吧，有时候甚至觉得在浪费时间。",
            "听起来你在面对这些感受的时候，可能会有点抗拒，像是不想让它占据你太多的精力或时间。这种抗拒会让你觉得轻松一些，还是会让内心更复杂？"
        ],
        [
            "会让自己更复杂吧，好像这些情绪不会真的消失，它们还是会突然跳出来，提醒我它们的存在。尤其是在我觉得状态不错，或者目标清晰的时候。",
            "原来如此，那种压下的情绪好像会趁你放松警惕时冒出来，打乱你的节奏，这听上去确实让人不舒服。你觉得这些情绪是和哪些经历或事情有关联呢？"
        ],
        [
            "过去一些不太好的事吧，和家庭有关。小时候经历过家暴，我从那时候变得更封闭了，可能它对我现在的性格、生活都有些影响。",
            "那一定是很难忘的经历，对现在的生活也留下了印记。听起来这些过往对你来说可能还影响到了你的一些情感和人际关系，是这样吗？"
        ],
        [
            "是的，尤其是在和别人的关系中吧。好像越是亲密的关系，我越不敢完全信任对方，也不知道怎样打开自己。",
            "嗯，原来亲密关系对你来说带着一种挑战，像是既渴望又担心的感受，不知道该如何去平衡这种矛盾是吗？"
        ],
        [
            "是的，我很羡慕那些能随意表达情感、找到快乐的人。可是我不知道自己该怎么去迈出这一步，好像总是被一种无形的东西卡住了。",
            "那种自由表达和快乐的状态听起来确实让人向往，而你感到被卡住时，可能是因为内心的某些需求遇到了难题。我们可以试着一起探索一下，这种“卡住”的背后，它想让你注意到什么，或者希望抓住什么？"
        ],
        [
            "我觉得我可能真的需要去看看自己内心真正需要的是什么，可能是找到更真实的自我吧，也想知道该怎么开始信任别人。",
            "嗯，你提到之前尝试过去看看内心的真实需求，那在过去类似这样的状态里，你有试过哪些方法让自己感觉好一些吗？比如让自己更接近内心的真实或者更放松。"
        ]
    ]
}
```

The model first integrates memory to identify the core narrative of "childhood abuse → closed-off personality → trust issues"; then performs global planning, determining it is in the "working stage" without yet activated effective resources; finally, it executes strategy reasoning, choosing to start from the client's mentioned meditation and reading—methods that are "ineffective but present"—to explore potential positive experiences and advance resource activation.

**Ending Stage Example**

```json
{
    "instruction": "我愿意试试，谢谢你帮助我理清了这些思路！",
    "system": "# 【一次单元咨询疗法（Single-Session Therapy，SST）指南手册】...",
    "input": "",
    "output": "<think>\n我先回顾一下这位来访者的咨询进展。来访者小时候经历过家暴，性格变得封闭，在亲密关系中既渴望又担心，难以完全信任对方，不知如何打开自己，还感觉生活空虚，做事提不起劲。不过，来访者也有一些内部资源，比如尝试过阅读自我成长书籍、冥想、瑜伽，虽效果不持久但能带来短暂平静；还有写日记的习惯，虽未坚持，但现在打算以简单记录情绪、触发事件及内心需求的方式重新开始写日记，积极性较高。外部资源方面，来访者有个关系不错的朋友，会讨论学习或工作上的事情，虽不确定能否分享个人内容，但愿意慢慢尝试与之沟通，积极性也较高。本次咨询目标是帮助来访者尝试识别并表达内心真实的情感需求，增强对亲密关系中安全感和信任的体验。\n\n基于这些咨询进展，我来分析一下咨询阶段。来访者在本次咨询中已经和我探讨出了帮助自己识别并表达内心真实情感需求，增强亲密关系安全感和信任体验的目标。在工作阶段，也充分挖掘出了内部资源（重新开始以简单方式写日记）和外部资源（愿意尝试和朋友沟通），而且来访者对写日记及与朋友沟通都表现出了较高的积极性，在对话最后还表示愿意尝试所讨论的方案，也对我表达了感谢，这已经满足了工作阶段向结束阶段推进的条件。所以，下一步我应该进入结束阶段，对来访者进行反馈总结和鼓励等操作。\n\n再看看当前轮回复的情况。来访者当前轮发言说“我愿意试试，谢谢你帮助我理清了这些思路！”这表明来访者已表示愿意尝试所讨论的方案，满足从工作阶段向结束阶段推进的条件。在结束阶段，我应按照SST手册要求，对来访者进行反馈总结和鼓励。我要先通过回顾来访者过去经历及面对亲密关系的状态，肯定其探索情绪与尝试改变的行为，给予赞美。接着布置家庭作业，明确提出让来访者记录情绪等具体行动。最后再次鼓励，表达对其的支持，增强来访者信心。整体语气要积极、肯定且充满鼓励，运用反馈总结、鼓励赞美等SST疗法技巧，表达方式要详细，符合结束阶段适当延长回复长度的要求。 \n</think>\n今天我们聊了很多，你讲到一直被一些过去的经历影响，也让你在面对亲密关系时有些犹豫和不安，但同时我们也看到，你愿意去探索自己的情绪，勇敢地去尝试改变，这本身就是一种非常难得的力量。接下来，你可以每天花五分钟简单写下当天的情绪、触发的事情和内心的小需求，试着和自己对话，帮自己一点点更清楚地了解真实的感受。等你觉得舒适的时候，也可以尝试从一些轻松的小话题入手，和身边值得信任的人慢慢交流。你很有耐心，也在向前迈进，这些努力一定会帮助你找到方向。相信你能做到，我也会一直支持你！",
    "history": [
        // ... 对话历史
    ]
}
```

This case demonstrates CATCH's complete grasp of therapeutic logic in the ending stage. The model explicitly retrospects the entire session through MDP-CoT: accurately summarizing the client's core distress (childhood trauma, trust difficulties), the identified internal and external resources (journaling, friend support), and the counseling goal, and based on this, determines that the conditions for transitioning from the working stage to the ending stage have been met.

**Field Description:**

| Field Name | Type | Description |
| :--- | :--- | :--- |
| `instruction` | string | **Input**. The client's utterance for the current turn, which is the direct basis for generating the CoT and the response. |
| `system` | string | **System prompt**. Provides the SST therapy knowledge background to ensure the professionalism and consistency of the model's reasoning. |
| `output` | string | **Core output**. Contains the **MDP CoT** (the counselor's complete thought process) within the `<think>...</think>` tags, followed by the final generated **counselor response**. |
| `history` | list[list[string]] | **Context**. The dialogue history organized in the format `[ [client utterance, counselor response], ... ]`, providing the necessary context for reasoning in the current turn. |
