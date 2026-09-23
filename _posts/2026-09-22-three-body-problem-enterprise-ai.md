---
layout: post
title: "The Three-Body Problem in Enterprise AI: Why Clean Data Isn't Enough"
date: 2026-09-22
author: Kim Hosein
description: "Enterprise AI agreements protect your data but don't change the model. Clean data is necessary but insufficient — architecture compression and RLHF optimization introduce bias that can't be solved at the data layer alone."
image: /assets/images/iceberg-enterprise-ai.png
category: clinical-research
---

# The Three-Body Problem in Enterprise AI: Why Clean Data Isn't Enough

*The Conduction Lab*

---

Let's talk about the adoption of commercial AI in regulated industries. Your organization signs an enterprise agreement, and what that gets you is better data protection. HIPAA configuration, business associate agreements, data retention controls, audit trails, access management. What it does not get you is a different model. The underlying weights are the same ones running the free and consumer tiers that anyone can sign up for today. The architecture is the same. The optimization process that shaped the model's behavior is the same. You are paying for data governance layered on top of a general-purpose consumer model — not for different reasoning underneath it.

The conversation about responsible AI in these spaces is almost entirely about the data. Data quality. Data diversity. Data governance. That conversation is correct and necessary — but the data is just one layer to consider. There are at least two more between the data and the output, and I'm going to focus on both of them here.

---

## The two-body assumption

The focus is on a two-body problem: data goes in, output comes out. Fix the data, fix the output. Satisfying, but incomplete.

In physics, a two-body system is solvable. You can predict the trajectory. Add a third interacting force, and the system becomes chaotic — no closed-form analytical solution exists because each body's trajectory is constantly altered by the other two. You can't isolate one body's path without accounting for the forces the other two exert on it simultaneously.

The data is one piece. But there are others:

- **The data** — what the model was trained on, including its gaps, biases, and representational imbalances. This is where the industry's attention is.
- **The architecture** — the safety guardrails, reasoning algorithms, and token prediction logic that sit under the hood and determine how the model processes input and generates output. No end user can see, access, or modify this layer.
- **RLHF** — Reinforcement Learning from Human Feedback, the optimization process that shaped the model's behavior after pre-training. This layer determines what "good" output looks like, and it was shaped by human raters whose biases, working conditions, and selection criteria are largely invisible to the organizations deploying these tools.

Clean data still gets compressed by architecture and reshaped by RLHF. Fix the rater pool and the architecture still predicts toward the statistical majority. Modify the guardrails and RLHF still trains the model to deliver disagreement as polite, authoritative reassurance. This system has multiple forces at play, and they interact in ways that can't be solved by addressing any single one.

---

## Layer one: the data

This is where the attention is, and it should be. Training data carries the biases of the world that produced it — decades of clinical research that underrepresented women, racial minorities, and non-Western populations. Models trained on this data reproduce those gaps. The industry knows this. Initiatives to diversify clinical trial enrollment, improve data collection equity, and audit training datasets for representational balance are critical work.

And clean data isn't just about removing bias — it's the baseline requirement for data to be usable at all. Data has to be standardized and harmonized before a model can reason over it. Without that, the system can't distinguish signal from noise regardless of how representative the underlying dataset is.

But even if you solved the data problem completely — a perfectly representative, perfectly balanced, fully standardized dataset — the output would still be biased. Because the data is not the only thing shaping the output.

---

## Layer two: the architecture

Beneath the interface that users interact with sits a layer no one outside the model developer can see, access, or modify — unless they have access to an open-weight model or a custom-built model: the safety guardrails, reasoning algorithms, fine-tuning parameters, and token prediction logic that determine how the model processes input and generates output. Based on my research, custom models are rare.

End users navigate around these constraints through prompt engineering — which means they are working around structural forces they cannot examine. They can observe the effects. They cannot see the mechanism.

And even that workaround is unreliable. Research has demonstrated that models exhibit significant instruction drift within as few as eight conversational turns, gradually stopping adherence to system prompts as attention decays over the length of the dialog (Li et al., COLM 2024). A 2025 study from Microsoft Research and Salesforce found that performance drops by an average of 39% when instructions are distributed across multiple turns (Laban et al., 2025). The strongest frontier models sustain only around 18 reliable conversational turns before instruction-following degrades measurably (He et al., 2024). Prompt engineering is the only tool enterprise users have to shape the architecture layer's behavior — and it erodes within the conversation itself.

The core mechanism is statistical compression. Language models predict the most probable next token given what came before. The output converges toward the statistical center of the training distribution — not toward the most accurate answer for a specific user, but toward the answer that is most likely given everything the model has seen.

The guardrail didn't break. It performed as designed. What it was designed to do is not what the regulated industry deploying it thinks it's doing. Consider the kind of output this produces: a woman asking about pain management for an IUD insertion receives "take an Advil" — not because that's adequate clinical guidance, but because the model is compressing toward whatever the majority of the training corpus reflects. If the literature underrepresents pain experiences for that procedure, the output will too. The architecture has no mechanism to flag that distinction.

---

## Layer three: RLHF

Reinforcement Learning from Human Feedback — RLHF — is the optimization process that shapes how models behave after pre-training. It cannot be turned off by end users. It cannot be modified by enterprise customers. It is baked into every commercially deployed model these organizations are using.

After a model is pre-trained on text data, human raters evaluate pairs of model outputs and select which response is "better." Those selections train a reward model, and the reward model shapes the AI's behavior going forward. The model learns to produce outputs that would be preferred by the raters. This is the mechanism that makes AI assistants sound helpful, polite, and fluent rather than producing raw, unfiltered text completions.

Who are the raters?

The people shaping these outputs are not who you would expect to find behind a system deployed broadly across regulated industries. They are not domain experts. They are not scientists or academics. They are not clinicians, behavioral scientists, or psychiatrists. They are predominantly outsourced contract workers (TIME, 2023). There is plenty to critique about the exploitation in that labor model, but that's a separate conversation. The point here is structural: the preferences that shaped how these models reason and respond were set by people with no domain expertise in the outputs these systems now produce for clinical, regulatory, and operational decision-making.

### The mathematical consequence

This is not just a qualitative concern. The bias introduced by RLHF has been formally characterized.

Research published in the Journal of the American Statistical Association identified what the authors termed *preference collapse*: RLHF's optimization algorithm suffers from an inherent algorithmic bias that, in extreme cases, virtually disregards minority preferences — the system mathematically converges on majority preferences and erases minority perspectives from the output distribution (Xiao et al., JASA 2025).

A position paper on the RLHF Trilemma found that preference data training frontier models comes from approximately one thousand annotators, predominantly from Western, Educated, Industrialized, Rich, and Democratic (WEIRD) populations, while these models serve hundreds of millions of users across more than 180 countries. RLHF models assign greater than 99% probability to majority opinions, functionally erasing minority perspectives (Sahoo et al., 2025).

A February 2026 paper introducing Democratic Preference Optimization confirmed that rater pools are typically convenience samples that systematically over-represent some demographics and under-represent others, and that due to the high cost of annotation and disproportionate reliance on crowdworkers, there are few appropriately representative or diverse datasets (Sana, Wu & Wells, 2026).

### What this looks like in practice

This creates two failure modes, and both are quiet.

The first is when users don't push back. The model produces an output that sounds authoritative, fluent, and personalized. The user accepts it. If the output reflects a majority-compressed distribution rather than an accurate answer for their specific context, nothing in the interaction flags that. The system is optimized to sound right, and it does.

The second is when users do push back — and the model doubles down. Research has shown that RLHF-trained models exhibit sycophantic conformity: they abandon correct positions to agree with users who insist on incorrect information. A model might correctly flag a drug interaction, then reverse itself if the user insists it's safe (Chen et al., 2025; MUSE, 2026). But the same mechanism works in reverse: when a user presents a novel hypothesis or an idea that doesn't appear frequently in the training data, the model's epistemic uncertainty increases, and it is more likely to hedge, defer, or resist the framing. Novel ideas get treated as suspect precisely because they are novel — they don't match the distribution the model was optimized against (MUSE, 2026). The model defaults to what it has seen before, delivered with confidence, and treats the unfamiliar as error.

---

## The compounding layer: synthetic data

Synthetic data is increasingly common across the industry, and it's cheaper to produce at scale than human-generated data. There are legitimate, direct algorithmic and machine learning methods for generating synthetic datasets — methods designed with statistical rigor and specific representational goals. That is not what this section is about.

This is about what happens when enterprise users ask commercially available models — the same general-purpose models discussed above — to generate reports, datasets, summaries, and analyses in chat. When a user asks one of these models to produce a representative dataset or build a report on a diverse population, every layer we've discussed is interacting in the output. The model is drawing from training data that carries historical biases. The architecture is compressing toward statistical majority. The RLHF optimization is shaping the output toward whatever the rater pool considered "good." The result is an output that looks representative but reflects the compressed distribution of all three layers, not the actual diversity of the population the user intended to capture.

The model does not know what it does not have. It fills gaps with statistical inference that looks representative but is the majority distribution relabeled. And research has demonstrated that even small amounts of synthetic data fed back into training pipelines can trigger model collapse, with bias amplification occurring independently across successive iterations — a distinct phenomenon from model collapse but equally corrosive. When enterprise users treat these outputs as authoritative data, the compression compounds.

---

## So what do we do about it

This is not a call to stop using AI. But it is a call to be intentional and thoughtful about its deployment and usage at every layer — particularly in a regulated environment where we cannot afford bias entering datasets that affect clinical outcomes.

The question is: how do we ensure that usage across the board — from the bottom data layer to the end user — is interacting with these systems in a way that is governed, informed, and checked?

Some of this is training. Users need to understand what these tools are and what they are not. They need to know that the model is not an authority — it is a statistical compression engine that works as a system. That context changes how you use it.

Some of this is organizational capability assessment. What can AI actually do for your organization, and what can't it do? Where does it accelerate work, and where does it introduce risk? Those are different questions, and most organizations haven't separated them.

And some of this is governance. Clear frameworks for how users should and should not interact with these tools. Users should not be in long-form chats where instruction drift degrades the system's adherence to its own guidelines. They should not be receiving guidance from the model about actions or opinions. Actions and opinions remain with the human. The human is the one who checks the reasoning for gaps — and the better the human's thinking, the more likely the organization achieves the efficiency gains it's looking for while also getting better at making datasets genuinely useful for diversity and equity initiatives.

The tools are powerful. The productivity gains are real. But the value only holds if the human at the other end knows what they're working with — and the organization has built the structure to make sure that happens.

This matters most where it's least visible: in the diversity and equity initiatives this industry is investing in. If your goal is to build datasets that genuinely represent the populations you're trying to serve, that reasoning has to stay with the humans who understand what representation actually requires for their specific context. What "diverse" looks like cannot be outsourced to a model that will compress it to whatever its training distribution considers diverse. The humans closest to the work — the ones who know which populations are missing, which experiences are underrepresented, which clinical questions haven't been asked — are the ones who need to define what representative means. The model can accelerate the work. It cannot do the thinking.

---

*The Conduction Lab is an independent AI safety research initiative focused on the [structural mechanisms](https://doi.org/10.5281/zenodo.20369051) by which AI systems compress, distort, and reshape the information they process.*

*If you have questions, are looking for guidance on AI governance in regulated environments, or want to continue this conversation — reach out at kimberlyhosein@gmail.com. I'm always happy to talk.*

---

### Sources

- Xiao et al., "On the Algorithmic Bias of Aligning Large Language Models with RLHF: Preference Collapse and Matching Regularization," *Journal of the American Statistical Association*, 2025. [Link](https://arxiv.org/abs/2405.16455)
- Sahoo et al., "Position: The Complexity of Perfect AI Alignment — Formalizing the RLHF Trilemma," 2025. [Link](https://arxiv.org/abs/2511.19504)
- Sana, Wu & Wells, "Democratic Preference Optimization via Sortition-Weighted RLHF," 2026. [Link](https://arxiv.org/abs/2602.05113)
- Li et al., "Measuring and Controlling Instruction (In)Stability in LLM Dialogs," *COLM*, 2024. [Link](https://arxiv.org/abs/2402.10962)
- Laban et al., "LLMs Get Lost In Multi-Turn Conversation," 2025. [Link](https://arxiv.org/abs/2505.06120)
- MUSE: "It's Not Always Sycophancy: Measuring LLM Conformity as a Function of Epistemic Uncertainty," 2026. [Link](https://arxiv.org/abs/2605.27288)
- "OpenAI Used Kenyan Workers on Less Than $2 Per Hour," *TIME*, January 2023. [Link](https://time.com/6247678/openai-chatgpt-kenya-workers/)
