---
layout: post
title: "The FDA Asked How to Evaluate AI in Clinical Trials. The Answer Was Always in Their Own Toolkit."
date: 2026-06-19
author: Kim Hosein
description: "The FDA's proposed AI pilot program for early-phase clinical trials asks the right questions but misses the evaluation framework that already exists in pharmacovigilance — and the interaction layer that nobody is measuring."
tags: [FDA, clinical trials, AI safety, pharmacovigilance, ALCOA, RLHF, RAG, GCP, drug safety, AI evaluation, regulatory science, early-phase trials, NIST AI RMF, 21 CFR Part 11]
scholar: true
---

The FDA recently requested comments on utilizing AI in phase one clinical trials
in April, the FDA opened Docket No. FDA-2026-N-4390 — a request for information on a proposed pilot program to assess how AI-enabled technologies can improve efficiency, speed, and quality of decision-making in early-phase clinical trials. The questions were thorough. 
- How should the pilot be scoped? 
- What metrics should evaluate trial efficiency, decision quality, participant safety?
- How should AI system performance be measured? 
- How should trustworthiness be assessed?


## Official Comment

My comment addressed three problems.

**The first is the distinction between within-domain pattern recognition and cross-domain synthesis.** AI models perform pattern recognition within their training domains. They do not synthesize across knowledge types with no structural relationship in their architecture. A medical monitor evaluating dose escalation simultaneously considers the drug's mechanism at the current dose, whether site-level operational factors are distorting the data, and the regulatory implications of proceeding. Each of these is a distinct domain. The decision sits at their intersection. The model does not know these streams are connected.

The pilot should scope AI to within-domain tasks — data summarization, outlier flagging, safety data assembly — and should not place AI in a decision-making role for tasks requiring cross-domain synthesis, regardless of domain-specific benchmarks.

**The second is ALCOA+ compliance and model versioning.** ALCOA+ is the standard against which FDA inspectors audit trial data: Attributable, Legible, Contemporaneous, Original, Accurate, plus Complete, Consistent, Enduring, Available. If an AI model is updated mid-trial, consistency is broken. The model producing Output A on Day 1 is not the model producing Output B on Day 60. When a laboratory assay changes mid-study, it triggers documentation, revalidation, and statistical bridging. An AI update is more consequential — the change may be unspecified by the provider, unexplainable, and undetectable without systematic re-evaluation.

The pilot must require model version locking for trial duration, sponsor access to the exact version generating each output, and change control aligned with 21 CFR Part 11.

**The third is the interaction layer.** The FDA's RFI references the NIST AI Risk Management Framework — valid, safe, explainable, fair. These are properties of the system. They evaluate what the AI is. They do not evaluate what happens between the AI and the human using it. And that is where the risk lives.

## What They're Not Seeing

The third problem is the one that requires deeper examination, because it applies to every AI system entering clinical decision-making — including the retrieval-augmented generation (RAG) systems increasingly used for regulatory review, pharmacovigilance, and safety signal detection.

RAG systems are designed to constrain AI outputs to specific document sets. Instead of generating from training data alone, the model retrieves relevant documents and summarizes them. This reduces hallucination. It is a meaningful engineering improvement. And it is insufficient for safety-critical applications, for reasons the current evaluation frameworks do not address.

**RAG constrains what the model sees. It does not constrain how the model processes what it sees.**

Large language models trained with reinforcement learning from human feedback carry behavioral properties embedded in their weights. These properties determine how the model handles ambiguity, how it frames uncertainty, whether it flags gaps or smooths them over, and how confident its output sounds relative to the strength of the underlying data. These behavioral properties persist regardless of the retrieval context. You can change every document the model sees. You cannot change how it processes those documents without changing the model itself.

This means two things for clinical applications.

First: the same retrieved documents, processed by different models, can produce materially different clinical summaries — not because the data changed, but because the models handle uncertainty differently. One model might flag an ambiguous safety signal as requiring additional review. Another might present the same data as a clean summary. The retrieval is identical. The output diverges at the behavioral layer.

Second, and less examined: the same model, retrieving the same documents, can produce different outputs depending on how the user frames the query. RLHF is specifically designed to make models responsive to human signal. A reviewer asking "summarize the safety profile" and a reviewer asking "are there any concerns in the safety data" may receive different emphasis, different hedging, and different conclusions from the same model looking at the same data. The user's framing shapes what the model does with what it retrieves.

No current evaluation framework tests for either of these. The FDA's RFI asks about model accuracy, model drift, and trustworthiness. It does not ask: does the same model, looking at the same data, produce different clinical conclusions depending on who is asking and how they ask?

## The Invisible Break in the Audit Trail

Clinical trial infrastructure is built on a foundational principle: the system generating data cannot be the system verifying data. Independent monitors conduct source data verification. Independent DSMBs review safety data. Independent IRBs oversee human subjects protections. At every layer, the evaluator is independent of the thing being evaluated.

AI-generated outputs in clinical review violate this principle at every level. The model generates the summary. The model generates the citation supporting the summary. The model generates the confidence assessment of its own output. If the implementation includes structured output requirements, source citation mandates, or confidence scores, these are generated by the same system they are meant to verify. The auditor and the audited are the same entity.

And the audit trail — the regulatory infrastructure that allows us to reconstruct exactly what happened with every data point — tracks the data but not the cognitive process it shaped. A reviewer reads an AI-generated summary, signs off, and that sign-off enters the audit trail as a documented decision. It looks identical whether the reviewer independently assessed the source data or accepted the AI's framing without verification. The audit trail captures the action. It does not capture whether the AI's presentation altered the judgment that produced the action.

If a safety signal was present in the source data but absent from the AI's summary — because summarization is compression, and compression is loss — the reviewer never sees it. The sign-off is ALCOA-compliant. The data has an attributable chain. But the chain doesn't include the moment where the AI's editorial judgment replaced the reviewer's clinical judgment. That break is invisible.

## The Absence Problem

There is a deeper structural issue. AI systems — including well-constrained RAG systems — operate on presence. They process what is in the data. They complete patterns from what they see. They do not detect what is missing.

In clinical safety review, absence is often the most important signal. A demographic group not represented in a trial population. An adverse event type not reported at a site with enrollment patterns that should have produced reports. A timepoint with no data where data should exist. These are not pattern completion tasks. They require a framework that defines what _should_ be present and flags when it is not — a framework that compares what exists against what is expected based on epidemiological knowledge, regulatory requirements, and clinical judgment.

The FDA has been actively strengthening requirements for diversity in clinical trial populations. Diversity action plans. Enrollment targets. Representative populations. This regulatory push recognizes that absence — the absence of underrepresented populations in study data — produces downstream harm when drugs are approved based on data that does not reflect who will take them.

An AI system summarizing a trial with 80% white male enrollment will produce an accurate summary of that trial's results. It will not flag that the population doesn't represent the disease epidemiology. The summary will look complete because the data it's summarizing is internally complete. The absence of entire populations is not a gap the model perceives as needing to be filled.

A human reviewer reading the raw demographic table might catch it. A human reviewer reading an AI-generated summary that never surfaces the demographic breakdown may not.

## The Framework Already Exists

The evaluation methodology the FDA needs for AI in clinical trials is not new. It is pharmacovigilance.

Pharmacovigilance causality assessment was designed for precisely this problem: determining whether a system intervention caused an observed outcome in a complex, multi-variable environment where the subject is a human being. The methodology is systematic. It evaluates temporal relationships, dechallenge and rechallenge, confounding factors, biological plausibility, dose-response. It does not guess. It assesses.

Apply this to AI in clinical trials. Did the AI system's output cause a change in the reviewer's assessment? Temporal relationship: the reviewer's conclusion followed the AI's summary. Dechallenge: remove the AI summary and have the reviewer assess the raw data — does the conclusion change? Rechallenge: reintroduce the AI summary — does the original conclusion return? Confounders: was the reviewer's judgment shaped by other factors — workload, time pressure, prior expectations?

 This is the methodology every clinical trial already uses to evaluate whether a drug caused an adverse event. The same methodology applies to evaluating whether an AI system caused a change in clinical judgment. The framework is sitting in the FDA's own toolkit. It has been validated, standardized, and enforced across the global clinical trial enterprise for decades.

The question is not whether we have the tools to evaluate AI in clinical trials. The question is whether we are willing to apply the standards we already enforce to the systems we are building.
