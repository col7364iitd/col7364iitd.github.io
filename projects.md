---
layout: page
title: Term Projects
nav_exclude: true
description: Tentative list of projects from the Instructor.
---

# Tentative Term Projects

## COL764: Information Retrieval and Web Search — Semester I, 2026

### All projects are to be done individually

---

**Status:** Tentative project list; the final scope will be agreed upon after an initial literature review.

## Purpose and expectations

Each of these projects is intended to be a small --doable within 2 months-- but serious research work in information retrieval (IR). 
It is not an exhaustive list of projects. **If you would like to propose a project yourself** please bring me a proposal. 

Each of these projects below have

- a concrete research question -- not just an implementation goal;
- recent published work that can serve as a starting point;
- one or more public benchmarks to measure your progress on;
- a scope compatible with modest computing resources ("modest" may still require significant GPU usage); and
- a plausible route to a workshop, ECIR, or SIGIR submission if the results are sufficiently strong.

**There is also a small preliminary document for each project that was generated with the help of a GenAI tool by the instructor. Once the project selection is finalised, instructor will share the file with respective students**

**Publication is not guaranteed**. 
A successful course project should nevertheless be designed and evaluated with **publication-quality care**. 

### Common requirements

Every student will be expected to:

1. Formulate a precise, falsifiable research hypothesis.
2. Reproduce at least one strong baseline and compare against one classical and at least two recent neural or LLM-based methods where applicable.
3. Evaluate on at least two datasets or two meaningfully different domains, unless the project specifies otherwise.
4. Report both effectiveness and efficiency: for example, nDCG/Recall together with latency, tokens, index size, or GPU time.
5. Include at least two substantive ablations and statistical significance or confidence analysis at the query level.
6. Produce an error taxonomy based on manual analysis of at least 50 failures.
7. Release reproducible code, configurations, random seeds, cached candidate sets, and model/version details. All of these should ideally be on a git repository (can be on GitHub, BitBucket, or IITD gitlab)
8. Write the final report in the structure of a short ECIR/SIGIR paper, including limitations and a transparent statement of how generative-AI tools were used.

**Merely replacing one LLM with another is not a sufficient contribution**. 
A good project should change or carefully study at least one of the following: the learning objective, supervision signal, retrieval policy, representation, evaluation methodology, or effectiveness–efficiency trade-off.

### Compute assumptions

All core experiments should be feasible on approximately **one 16 GB GPU**. Suitable methods include frozen-model inference, training a small prediction head or adapter, fine-tuning BERT-sized encoders, and 4-bit LoRA/QLoRA tuning of a 1.5B–3B parameter language model. A larger model may be used for limited teacher-label generation, but the project must not depend on sustained access to a large GPU cluster or an expensive proprietary API.

## Submission stages: 

### Stage 1: Proposal Checklist (deadline: between 20th September to 23rd September) 
(This is the stage where the instructor can suggest reformulating your project scope). 
**Weightage: 25%**

A detailed report that contains: 

1. one primary hypothesis and one backoff formulation;

2. the closest three papers and a paragraph explaining the novelty of the proposed hypothesis;

3. a benchmark and split plan, (be careful of leakage/contamination issues while choosing benchmarks);

4. the strongest classical, neural, and recent baseline that will be reproduced;

### Stage 2: Initial Pilot (deadline: between 5th October to 12th October)

**Weightage: 25%*** 

5. a preliminary pilot -- you can decide what this pilot should be in accordance with the report that you had submitted above;

6. the exact GPU, API, annotation, and indexing budget -- this is beyond the one you need for initial pilot;

7. a plan for statistical testing and at least 50 manually analyzed failures.

8. A GitHub repository link that contains all the code & reports and maintained regularly.

### Stage 3 and Final: End of the project report (deadline: between 10th November to 18th November)
**Weightage: 50%** 

1. Full report of all the work done, SIGIR-style paper draft, Github repository, AI usage statement, and shared data artefacts. 
Make sure at this stage you maintain all the final experiment logs, details of the statistical significant tests conducted.

2. A demo + viva (as needed)

---

## Project overview

| No. | Tentative project | Main theme | GPU level |
|---:|---|---|---|
| 1 | Query-adaptive reformulation for retrieval | Query understanding, hybrid IR | Light–moderate |
| 2 | Instruction-following retrieval with counterfactual constraints | Dense retrieval, representation learning | Moderate |
| 3 | Small reasoning models for difficult retrieval | Reasoning, RL/preference learning | Moderate |
| 4 | Preference-trained listwise reranking | Reranking, DPO/GRPO | Moderate |
| 5 | Query-adaptive sparse neural retrieval | Sparse retrieval, efficiency | Light–moderate |
| 6 | Learning query-specific ANN search budgets | Vector search, systems | Light |
| 7 | Learning when to retrieve, decompose, and stop | Multi-hop retrieval, sequential decisions | Moderate |
| 8 | Evidence-utility reranking for grounded generation | RAG evaluation, evidence selection | Light–moderate |
| 9 | Calibrated and selective LLM relevance assessment | Evaluation, uncertainty | Moderate |
| 10 | Query-dependent passage granularity | Indexing, long documents | Light–moderate |
| 11 | Value-of-clarification in conversational search | Interactive IR, decision theory | Light–moderate |
| 12 | Verifier-guided, budgeted knowledge-graph retrieval | Graph retrieval, reasoning | Moderate |
| 13 | Negation- and exclusion-aware multimodal retrieval | Multimodal IR, compositionality | Moderate |

---

## 1. Query-adaptive reformulation for retrieval

### Objective

Different queries benefit from different reformulation strategies: pseudo-relevance feedback, hypothetical document generation, direct query expansion, decomposition, or no reformulation at all. Build a lightweight policy that predicts which strategy—and what expansion budget—to use for each query.

### Research opportunity

Most published systems apply one reformulation method uniformly. A publishable contribution could show that query properties such as ambiguity, specificity, predicted difficulty, or initial-result disagreement can support a better effectiveness–latency trade-off. Possible methods include a supervised router, a contextual bandit, or a small policy optimized with an explicit cost-aware reward.

### Suggested evaluation

- **Benchmarks:** BEIR and TREC Deep Learning; optionally a domain-shift subset such as SciFact or FiQA.
- **Baselines:** BM25; dense retrieval; pseudo-relevance feedback; always-use-HyDE; always-use-Query2doc.
- **Metrics:** nDCG@10, Recall@100, reformulation cost, latency, query drift, and routing regret relative to an oracle.
- **Useful ablations:** query features, router type, reformulation budget, and teacher-label source.

### Starter readings

- Gao et al., [Precise Zero-Shot Dense Retrieval without Relevance Labels (HyDE)](https://aclanthology.org/2023.acl-long.99/), ACL 2023.
- Wang et al., [Query2doc: Query Expansion with Large Language Models](https://aclanthology.org/2023.emnlp-main.585/), EMNLP 2023.
- Feng et al., [Synergistic Interplay between Search and Large Language Models for
Information Retrieval](https://aclanthology.org/2024.acl-long.517/), ACL 2024.

**Compute profile:** Mostly frozen retrieval and LLM inference; train only a small router or adapter.

---

## 2. Instruction-following retrieval with counterfactual constraints

### Objective

Train a retriever that follows natural-language search instructions, including inclusion, exclusion, audience, style, temporal, and source constraints—not merely topical similarity.

### Research opportunity

Instruction-following retrievers can appear successful by exploiting topic overlap while ignoring the instruction. Construct counterfactual training or evaluation examples in which the query topic is held constant but the instruction changes. Explore contrastive losses, hard-negative mining, or parameter-efficient fine-tuning that explicitly penalizes instruction violations.

### Suggested evaluation

- **Benchmarks:** FollowIR and InstructIR-style tasks; augment with controlled counterfactual pairs.
- **Baselines:** BM25, a general dense retriever such as E5/BGE, and Promptriever.
- **Metrics:** nDCG@10, instruction-following delta, pairwise constraint accuracy, and performance on unseen instruction types.
- **Useful ablations:** synthetic versus human-written instructions, negative construction, instruction position, and frozen versus adapted encoder.

### Starter readings

- Weller et al., [Promptriever: Instruction-Trained Retrievers Can Be Prompted Like Language Models](https://openreview.net/forum?id=odvSjn416y), ICLR 2025.
- Weller et al., [FollowIR: Evaluating and Teaching Information Retrieval Models to Follow Instructions](https://aclanthology.org/2025.naacl-long.597/), NAACL 2025.
- Feng et al., [Don’t Reinvent the Wheel: Efficient Instruction-Following Text Embedding based on Guided Space Transformation](https://aclanthology.org/2025.acl-long.1196/), ACL 2025.

**Compute profile:** Fine-tune a BERT-sized dual encoder using LoRA or standard contrastive learning.

---

## 3. Small reasoning models for difficult retrieval

### Objective

Train a compact model to generate a short query rationale, decomposition, or search plan before retrieval for questions whose relevant documents do not share obvious lexical cues with the query.

### Research opportunity

Reasoning-intensive retrieval remains difficult even for strong embedding models. Investigate whether a 1.5B–3B model, trained with QLoRA and preference or reinforcement learning, can improve retrieval without producing long and expensive chains of thought. The reward could combine retrieval gain, factual consistency, rationale length, and latency.

### Suggested evaluation

- **Benchmarks:** BRIGHT; selected BEIR datasets as an out-of-domain control.
- **Baselines:** BM25, a strong dense retriever, zero-shot LLM query reasoning, and supervised fine-tuning.
- **Metrics:** nDCG@10, Recall@100, tokens generated, latency, and gain conditional on predicted query difficulty.
- **Useful ablations:** SFT versus DPO/GRPO, rationale length penalty, reward components, and retrieval model.

### Starter readings

- Su et al., [BRIGHT: A Realistic and Challenging Benchmark for Reasoning-Intensive Retrieval](https://openreview.net/forum?id=ykuc5q381b), ICLR 2025.
- Qin et al., [Reinforced Query Reasoners for Reasoning-Intensive Retrieval](https://aclanthology.org/2025.emnlp-main.1078/), EMNLP 2025.
- Das et al., [RaDeR: Retrieval-Augmented Dense Retrieval with Reasoning](https://aclanthology.org/2025.emnlp-main.1011/), EMNLP 2025.

**Compute profile:** 4-bit LoRA/QLoRA on a 1.5B–3B model; cache all retrieval candidates and rewards.

---

## 4. Preference-trained listwise reranking

### Objective

Develop a small listwise reranker trained from ranking preferences rather than only pointwise relevance labels.

### Research opportunity

LLM rerankers are often sensitive to document order, prompt format, and list length. Generate preference pairs from relevance judgments or a stronger teacher, then compare supervised fine-tuning with DPO or a small-policy GRPO variant. A strong contribution would improve permutation robustness, calibration, or cost while preserving ranking quality.

### Suggested evaluation

- **Benchmarks:** TREC Deep Learning 2019/2020 and BRIGHT; optionally one BEIR transfer collection.
- **Baselines:** BM25 or dense first-stage retrieval, a cross-encoder, RankGPT-style prompting, and FIRST-style reranking.
- **Metrics:** nDCG@10, MRR@10, permutation sensitivity, latency, tokens per query, and pairwise preference accuracy.
- **Useful ablations:** preference construction, list size, position randomization, loss function, and teacher size.

### Starter readings

- Sun et al., [Is ChatGPT Good at Search? Investigating Large Language Models as Re-Ranking Agents](https://aclanthology.org/2023.emnlp-main.923/), EMNLP 2023.
- Reddy et al., [FIRST: Faster Improved Listwise Reranking with Single Token Decoding](https://aclanthology.org/2024.emnlp-main.491/), EMNLP 2024.
- Zhuang et al., [Rank-R1: Enhancing Reasoning in LLM-based Document Rerankers via Reinforcement Learning](https://dl.acm.org/doi/10.1145/3805712.3809961), SIGIR 2026.

**Compute profile:** QLoRA on a compact language model; rerank only a cached top-20 or top-50 candidate set.

---

## 5. Query-adaptive sparse neural retrieval

### Objective

Learn to allocate a different sparse-retrieval budget to each query—for example, the number of expansion terms, active dimensions, or postings traversed.

### Research opportunity

Sparse neural retrievers usually apply a global pruning or regularization setting. Easy queries may need very few terms, while difficult queries benefit from expansion. Predicting the required budget from the query could improve the Pareto frontier between effectiveness and efficiency. The project should include real index measurements rather than FLOP proxies alone.

### Suggested evaluation

- **Benchmarks:** MS MARCO passage ranking and several BEIR collections.
- **Baselines:** BM25, a SPLADE-family model, global top-*k* pruning, and static posting-budget methods.
- **Metrics:** MRR@10/nDCG@10, Recall@100, index size, postings visited, CPU latency, and tail latency.
- **Useful ablations:** difficulty signal, per-query top-*k*, regularizer, and index pruning threshold.

### Starter readings

- Formal et al., [Towards Effective and Efficient Sparse Neural Information Retrieval](https://dl.acm.org/doi/10.1145/3634912), ACM TOIS 2024.
- Nardini et al., [Effective Inference-Free Retrieval for Learned Sparse Representations](https://dl.acm.org/doi/10.1145/3726302.3730185), SIGIR 2025.
- Xu et al., [LACONIC: Dense-Level Effectiveness for Scalable Sparse Retrieval via a Two-Phase Training Curriculum](https://dl.acm.org/doi/10.1145/3805712.3809869), SIGIR 2026.

**Compute profile:** Use released sparse encoders and train only a budget predictor or small adapter; indexing and retrieval are primarily CPU tasks.

---

## 6. Learning query-specific ANN search budgets

### Objective

Predict the approximate-nearest-neighbour (ANN) search effort required for each query, such as HNSW `efSearch` or IVF `nprobe`, rather than using one global setting.

### Research opportunity

Queries vary in local density, ambiguity, and first-stage score distribution. Build an early-exit or budget-selection model using cheap pre-search or partial-search features. The research goal is a calibrated policy that meets a recall or latency target under distribution shift.

### Suggested evaluation

- **Benchmarks:** MS MARCO and selected BEIR collections using a fixed dense encoder and FAISS/HNSW index.
- **Baselines:** fixed-budget ANN, exhaustive dense retrieval on a manageable subset, LADR, and score-threshold early exit.
- **Metrics:** ANN recall, end-to-end nDCG@10, mean and p95 latency, distance computations, calibration error, and constraint violations.
- **Useful ablations:** query embedding features, partial-search features, target recall, and in-domain versus shifted queries.

### Starter readings

- Kulkarni et al., [Lexically-Accelerated Dense Retrieval](https://dl.acm.org/doi/10.1145/3539618.3591715), SIGIR 2023.
- Zhang et al., [Hybrid Inverted Index Is a Robust Accelerator for Dense Retrieval](https://aclanthology.org/2023.emnlp-main.116/), EMNLP 2023.
- Busolin et al., [Early Exit Strategies for Approximate k-NN Search in Dense Retrieval](https://dl.acm.org/doi/10.1145/3627673.3679903), CIKM 2024.
- Zhang and Miller, [Distribution-Aware Exploration for Adaptive HNSW Search](https://dl.acm.org/doi/10.1145/3786639), SIGMOD 2026.

**Compute profile:** One frozen dense encoder; train a small regressor or classifier. The main experiments run on CPU after embeddings are cached.

---

## 7. Learning when to retrieve, decompose, and stop

### Objective

Build a controller for multi-hop question answering that decides whether to answer, issue another search, decompose the question, or stop.

### Research opportunity

Fixed multi-hop pipelines waste retrieval calls on easy questions and stop too early on difficult ones. Treat retrieval as a sequential decision problem with a reward for answer quality and penalties for search calls, generated tokens, and unsupported conclusions. Compare supervised imitation with an offline contextual-bandit or lightweight PPO/GRPO formulation.

### Suggested evaluation

- **Benchmarks:** HotpotQA, 2WikiMultihopQA, and MuSiQue.
- **Baselines:** fixed one-hop and fixed multi-hop retrieval, IRCoT, FLARE, and Adaptive-RAG.
- **Metrics:** answer EM/F1, supporting-fact recall, retrieval calls, tokens, latency, and quality under a fixed budget.
- **Useful ablations:** action space, reward terms, uncertainty signal, maximum hops, and answer verifier.

### Starter readings

- Trivedi et al., [Interleaving Retrieval with Chain-of-Thought Reasoning for Knowledge-Intensive Multi-Step Questions](https://aclanthology.org/2023.acl-long.557/), ACL 2023.
- Jiang et al., [Active Retrieval Augmented Generation](https://aclanthology.org/2023.emnlp-main.495/), EMNLP 2023.
- Jeong et al., [Adaptive-RAG: Learning to Adapt Retrieval-Augmented Large Language Models through Question Complexity](https://aclanthology.org/2024.naacl-long.389/), NAACL 2024.
- Fang et al., [KiRAG: Knowledge-Driven Iterative Retriever for Enhancing Retrieval-Augmented Generation](https://aclanthology.org/2025.acl-long.929/), ACL 2025.

**Compute profile:** Freeze the reader and retriever; train only the controller or a LoRA adapter on a small policy model.

---

## 8. Evidence-utility reranking for grounded generation

### Objective

Rerank passages by the additional answer claims they support, not only by their independent relevance to the query.

### Research opportunity

A top-*k* set can contain individually relevant but redundant passages. Define a marginal evidence-utility score that rewards new claim coverage and penalizes contradiction or redundancy. Possible approaches include submodular selection, a small cross-encoder, or preference learning from answer-level utility labels generated with a fixed reader.

### Suggested evaluation

- **Benchmarks:** ASQA, QAMPARI, HotpotQA, and RAGTruth-derived settings.
- **Baselines:** relevance-only top-*k*, maximal marginal relevance, diversity clustering, and a standard cross-encoder reranker.
- **Metrics:** retrieval nDCG/Recall, citation or claim recall, faithfulness, answer correctness, redundancy, and latency.
- **Useful ablations:** utility-label source, set-aware versus independent scoring, contradiction penalty, and evidence-set size.

### Starter readings

- Saad-Falcon et al., [ARES: An Automated Evaluation Framework for Retrieval-Augmented Generation Systems](https://aclanthology.org/2024.naacl-long.20/), NAACL 2024.
- Ru et al., [RAGChecker: A Fine-grained Framework for Diagnosing Retrieval-Augmented Generation](https://proceedings.neurips.cc/paper_files/paper/2024/hash/27245589131d17368cccdfa990cbf16e-Abstract-Datasets_and_Benchmarks_Track.html), NeurIPS 2024.
- Niu et al., [RAGTruth: A Hallucination Corpus for Developing Trustworthy Retrieval-Augmented Language Models](https://aclanthology.org/2024.acl-long.585/), ACL 2024.
- Ju et al., [Controlled Retrieval-augmented Context Evaluation for Long-form RAG](https://aclanthology.org/2025.findings-emnlp.1151/), Findings of EMNLP 2025.

**Compute profile:** Use a fixed small generator and cached candidates; train only a compact utility scorer or adapter.

---

## 9. Calibrated and selective LLM relevance assessment

### Objective

Build a relevance assessor that can estimate its uncertainty and abstain on difficult query–document pairs instead of always producing a confident label.

### Research opportunity

LLM-generated relevance judgments can be inconsistent across prompts, batches, and query types. Compare verbalized probabilities, self-consistency, ensembles, and post-hoc calibration. Develop a selective assessment policy that sends only uncertain cases to a human or stronger model while preserving system-ranking fidelity.

### Suggested evaluation

- **Benchmarks:** LLMJudge and publicly judged TREC Deep Learning topics.
- **Baselines:** zero-shot prompting, few-shot prompting, batched self-consistency, and a fine-tuned compact classifier.
- **Metrics:** macro-F1, quadratic weighted kappa, Brier score, expected calibration error, risk–coverage curves, and Kendall correlation between system rankings.
- **Useful ablations:** prompt order, batch size, number of samples, calibration set size, and domain shift.

### Starter readings

- Faggioli et al., [Perspectives on Large Language Models for Relevance Judgment](https://dl.acm.org/doi/10.1145/3578337.3605136), ICTIR 2023.
- Ni et al., [DIRAS: Efficient LLM Annotation of Document Relevance for Retrieval Augmented Generation](https://aclanthology.org/2025.naacl-long.271/), NAACL 2025.
- Korikov et al., [Batched Self-Consistency Improves LLM Relevance Assessment and Ranking](https://aclanthology.org/2025.emnlp-main.1661/), EMNLP 2025.
- Rahmani et al., [LLMJudge: A Benchmark for LLM-based Relevance Judgments](https://llm4eval.github.io/LLMJudge-benchmark/).

**Compute profile:** Zero-/few-shot inference plus optional QLoRA of a 1.5B–3B judge; cache prompts and outputs.

---

## 10. Query-dependent passage granularity

### Objective

Index documents at multiple granularities—sentence/proposition, passage, document, and optionally summary-tree node—and learn which granularity or combination to search for each query.

### Research opportunity

Small chunks improve localization but lose context; large chunks preserve context but add noise. Instead of fixing one chunk size, learn a router using query and initial-retrieval features. A stronger extension is to retrieve coarse context first and selectively descend to finer units under a latency budget.

### Suggested evaluation

- **Benchmarks:** QASPER, QuALITY, HotpotQA, and MultiFieldQA-en or another long-document QA collection.
- **Baselines:** fixed-size chunking, document retrieval, Dense-X proposition retrieval, and RAPTOR-style hierarchical retrieval.
- **Metrics:** nDCG/Recall, answer EM/F1, evidence recall, index size, retrieved tokens, and latency.
- **Useful ablations:** granularity set, router features, hierarchical versus flat index, and token budget.

### Starter readings

- Chen et al., [Dense X Retrieval: What Retrieval Granularity Should We Use?](https://aclanthology.org/2024.emnlp-main.845/), EMNLP 2024.
- Sarthi et al., [RAPTOR: Recursive Abstractive Processing for Tree-Organized Retrieval](https://openreview.net/forum?id=GN921JHCRw), ICLR 2024.
- Zhao et al., [MoC: Mixtures of Text Chunking Learners for Retrieval-Augmented Generation System](https://aclanthology.org/2025.acl-long.258/), ACL 2025.

**Compute profile:** Precompute embeddings with a frozen encoder and train a small router; generation is optional and can use a compact model.

---

## 11. Value-of-clarification in conversational search

### Objective

Learn whether a search system should answer immediately, retrieve more results, or ask a clarifying question—and which question has the highest expected value.

### Research opportunity

Clarifying questions impose interaction cost and are useful only when the likely answer changes the ranking materially. Estimate the expected value of clarification from intent uncertainty and simulated or logged responses. Research challenges include simulator bias, safe offline evaluation, and policies robust to uncooperative or ambiguous answers.

### Suggested evaluation

- **Benchmarks:** TREC iKAT; CLAMBER- or PROCLARE-style clarification data; a carefully documented user simulator.
- **Baselines:** always answer, always clarify, uncertainty threshold, and a supervised clarification classifier.
- **Metrics:** retrieval nDCG after clarification, expected utility, number of turns, bad-question rate, and robustness across simulators.
- **Useful ablations:** response simulator, user-cost term, number of candidate questions, and uncertainty estimator.

### Starter readings

- Aliannejadi et al., [TREC iKAT 2023: The Interactive Knowledge Assistance Track](https://dl.acm.org/doi/10.1145/3626772.3657860), SIGIR 2024.
- Chen et al., [STYLE: Improving Domain Transferability of Asking Clarification Questions in Large Language Model Powered Conversational Agents](https://aclanthology.org/2024.findings-acl.632/), Findings of ACL 2024.
- Ye et al., [ProductAgent: Benchmarking Conversational Product Search Agent with Asking Clarification Questions](https://aclanthology.org/2025.emnlp-industry.25/), EMNLP Industry 2025.
- Abbasiantaeb et al., [Generating Multi-Aspect Queries for Conversational Search](https://aclanthology.org/2026.eacl-long.383/), EACL 2026.

**Compute profile:** Frozen retriever/LLM plus a small policy model; most experiments can be performed offline with cached interactions.

---

## 12. Verifier-guided, budgeted knowledge-graph retrieval

### Objective

Retrieve a small, executable subgraph or reasoning path that is sufficient to answer a question, while controlling graph-expansion cost.

### Research opportunity

Graph retrievers often expand too many nodes or produce plausible but invalid paths. Combine a learned path selector with a symbolic execution or consistency verifier. Preference learning can favor valid, minimal paths over invalid or unnecessarily long ones. The central research question is whether verification improves both answer quality and path faithfulness under a fixed expansion budget.

### Suggested evaluation

- **Benchmarks:** WebQSP, ComplexWebQuestions, GrailQA, or KQA Pro; select two according to implementation maturity.
- **Baselines:** shortest-path/heuristic search, Think-on-Graph, Reasoning on Graphs, and a text-only RAG baseline.
- **Metrics:** answer F1/Hits@1, path recall, execution validity, nodes expanded, latency, and faithfulness of the final explanation.
- **Useful ablations:** verifier type, maximum path length, preference objective, and graph-pruning strategy.

### Starter readings

- Sun et al., [Think-on-Graph: Deep and Responsible Reasoning of Large Language Model on Knowledge Graph](https://openreview.net/forum?id=nnVO1PvbTv), ICLR 2024.
- Luo et al., [Reasoning on Graphs: Faithful and Interpretable Large Language Model Reasoning](https://openreview.net/forum?id=ZGNWW7xZ6Q), ICLR 2024.
- Agarwal et al., [SymKGQA: Few-Shot Knowledge Graph Question Answering via Symbolic Program Generation](https://aclanthology.org/2024.acl-long.545/), ACL 2024.
- He et al., [G-Retriever: Retrieval-Augmented Generation for Textual Graph Understanding and Question Answering](https://papers.nips.cc/paper_files/paper/2024/hash/efaf1c9726648c8ba363a5c927440529-Abstract-Conference.html), NeurIPS 2024.

**Compute profile:** Keep the graph retriever and answer model mostly frozen; train a compact path scorer, verifier, or LoRA adapter.

---

## 13. Negation- and exclusion-aware multimodal retrieval

### Objective

Improve image–text retrieval for queries containing negation and exclusion constraints, such as “a street without cars” or “a red object but not a flower.”

### Research opportunity

Vision–language encoders often preserve topical similarity while ignoring negation. Construct hard negative pairs that differ only in an excluded concept, then train a lightweight adapter or compositional scoring function that preserves ordinary retrieval quality. A particularly valuable contribution would distinguish true visual absence from failure to detect an object.

### Suggested evaluation

- **Benchmarks:** SugarCrepe, NegBench-style tests, MS COCO/Conceptual Captions hard negatives, and optionally CIRCO.
- **Baselines:** frozen CLIP/SigLIP, prompt engineering, hard-negative contrastive tuning, and TripletCLIP.
- **Metrics:** Recall@K, AP@10, constraint-violation rate, performance on positive/non-negated queries, and robustness to paraphrase.
- **Useful ablations:** negative construction, adapter placement, synthetic versus human captions, and number/type of excluded concepts.

### Starter readings

- Hsieh et al., [SugarCrepe: Fixing Hackable Benchmarks for Vision-Language Compositionality](https://proceedings.neurips.cc/paper_files/paper/2023/hash/63461de0b4cb760fc498e85b18a7fe81-Abstract-Datasets_and_Benchmarks.html), NeurIPS 2023.
- Patel et al., [TripletCLIP: Improving Compositional Reasoning of CLIP via Synthetic Vision-Language Negatives](https://proceedings.neurips.cc/paper_files/paper/2024/hash/39781da4b5d05bc2908ce08e43bc6404-Abstract-Conference.html), NeurIPS 2024.
- Alhamoud et al., [Vision-Language Models Do Not Understand Negation](https://openaccess.thecvf.com/content/CVPR2025/html/Alhamoud_Vision-Language_Models_Do_Not_Understand_Negation_CVPR_2025_paper.html), CVPR 2025.
- Prachi et al., [Answering Multimodal Exclusion Queries with Lightweight Sparse Disentangled Representations](https://dl.acm.org/doi/10.1145/3731120.3744617), ICTIR 2025.
- Prachi et al., [Being Positive about Negative Queries: Exclusion Aware Multimodal Retrieval using Disentangled Representations](https://wacv.thecvf.com/virtual/2026/poster/362), WACV 2026.

**Compute profile:** Freeze a CLIP/SigLIP backbone and train small adapters or projection layers using mined hard negatives.

---

## How to express project preferences

Submit the following for your **top three** projects, in ranked order ((submission link)[https://forms.cloud.microsoft/r/GDe24ct28s])

1. project number and title;
2. a short explanation of why the problem interests you;
3. relevant background in IR, machine learning, NLP, vision, graphs, or systems;

You should not begin large-scale model training before the scope and evaluation plan have been approved. The first milestone for every project will be a short literature review -- although the deadline is nearly at the end of the month, **you are strongly advised to submit as early as possible**. 

## Scope note

These topics are intentionally broader than a single implementation recipe. Dataset choices, model sizes, and milestones may be adjusted according to team background, available compute, and developments in the literature. Students may also propose a closely related topic, provided it satisfies the research, reproducibility, benchmark, and compute requirements stated above.

