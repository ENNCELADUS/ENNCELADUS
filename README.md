# Hi, I'm Anrui Wang 👋

I study **Computer Science and Technology (AI Honors Program)** at **ShanghaiTech University**. My research applies machine learning to biology, with a focus on protein interaction networks, virtual cell models, context-specific synthetic lethality, and scientific agents.

🔗 **Homepage:** https://ENNCELADUS.github.io · **CV:** [Download](cv/cv.pdf)

## Education

- **ShanghaiTech University**, Shanghai, China · Sep 2023 – Jul 2027 (expected)
- B.S. in Computer Science and Technology (AI Honors Program)
- Overall GPA: **3.58/4.00**
- Relevant coursework: Machine Learning, Deep Learning, Bioinformatics

## Research

### Deep Learning Framework for Inductive Protein–Protein Interaction (PPI) Prediction

**Advisors:** Prof. Xuming He (SIST, ShanghaiTech) · Dec 2025 – Sep 2026

We proposed GraSPPI, a motif graph prompt tuning framework for inductive PPI prediction. A triadic-closure motif template infers shared interaction partners from intrinsic protein features and uses them as prompts for pair predictions. Structural losses jointly regularize predictions on sampled training subgraphs to preserve network topology. GraSPPI had the best average rank across multiple metrics among the compared methods on held-out PPI networks.

### Context-Specific Synthetic-Lethality (SL) Prediction with Pretrained Virtual Cell Foundation Models

**Advisors:** Prof. Jie Zheng (SIST, ShanghaiTech) and Prof. Lichun Jiang (SIAIS, ShanghaiTech) · Nov 2025 – present

I develop a framework that uses pretrained AI Virtual Cell foundation models and multi-omics data to predict gene essentiality across cancer cell lines. I extend the predicted essentiality representations to predict context-specific synthetic-lethal partners in held-out cell lines. I also build a cell-line-specific benchmark from multiple SL screening studies and compare the model with baselines and prior methods.

### Deep Learning and Scientific Agent Framework for Membrane PPI Prediction

**Advisors:** Prof. Xuming He (SIST, ShanghaiTech) and Prof. Wenqing Shui (iHuman Institute, ShanghaiTech) · Jun 2025 – present

I developed a deep learning model for endogenous transmembrane PPI prediction, including the training algorithm, architecture optimization, and benchmarking. I also developed an interactive research website to explore the tissue-wide transmembrane PPI network. Current work combines wet-lab experiments with model development: experimental feedback helps an LLM scientific agent improve its workflow and predictor, and the agent guides the next experiments.

## Publications & Manuscripts

- Wang, A., He, X. **“From Pairs to Networks: Motif Graph Prompt Tuning for Inductive Protein–Protein Interaction Prediction.”** Manuscript under review at the International Conference on Learning Representations (ICLR), 2027.
- Yang, L., Dang, T., Luo, H., Tan, Q., Li, A., Lang, X., Yang, X., Shi, S., Liu, X., Wang, A., et al. **“Proteomics-Informed Prediction of a Tissue-Wide Endogenous Transmembrane Protein Interaction Network.”** Manuscript under review at Nature Biotechnology.

## Selected Projects

### CS290S: Official-Source RAG for ShanghaiTech/SIST QA

May 2026 – Jun 2026

Combined BM25 and dense retrieval with reciprocal-rank fusion, source diversity constraints, and reranking. Expected-source hit@5 increased from 0.74 to 0.89 on a controlled benchmark. Integrated a self-hosted Qwen generator with evidence-aware context selection, numbered citations, answer validation, and abstention. Correct answers in manual review increased from 59/100 to 69/100, while abstentions fell from 18/100 to 14/100. Used controlled ablations and failure analysis to evaluate retrieval and generation.

### CS181: Hybrid Logic-DQN Minesweeper Solver

Apr 2025 – Jun 2025

Combined logical mine constraints, exact enumeration, and probabilistic sampling with a policy that selects guaranteed-safe moves first and uses DQN as a fallback. The solver achieved win rates of 80.90% on 6×6 boards and 74.90% on 9×9 boards in controlled evaluations against constraint programming, probabilistic, pure-DQN, and hybrid strategies.

## Internship Experience

### Huida (Shanghai) Biotechnology Co., Ltd.

AI Drug Data Retrieval Assistant R&D Intern · Shanghai, China · Jul 2025 – Sep 2025

Built a pipeline to parse, clean, extract, chunk, deduplicate, and normalize heterogeneous animal-study reports. Combined LanceDB vector search, BM25, weighted score fusion, metadata filtering, reranking, and citation constraints for traceable answers. Integrated the pipeline into the bilingual ToxiRAG assistant with a Streamlit interface and command-line ingestion and evaluation tools. Evaluated the system with benchmarks and failure-case tests.

## Awards & Competitions

### Creating a Moon Colony Using a Space Elevator System

**MCM · Honorable Mention · Jan 2026 – Feb 2026**

Modeled delivery of 100 million metric tons for a 100,000-person Moon colony. Compared space-elevator, rocket, and hybrid systems with a time-dependent multi-commodity network-flow MILP in Pyomo. Used scenario experiments and 1,000-run Monte Carlo simulations to evaluate schedule, cost, reliability, water supply, and environmental impact.

### NIPT Timing Optimization & Fetal Abnormality Determination

**CUMCM · Second Prize, Shanghai Division · Sep 2025**

Used spline mixed-effects models, survival models, and Turnbull estimates to derive BMI testing windows. Compared Elastic-Net logistic regression and XGBoost using chromosome, GC-content, and sequencing-quality features. Evaluated performance with patient-grouped cross-validation, held-out tests, calibration, and Monte Carlo error propagation.

## Skills

Proficient in using AI agents.

## GitHub Stats

<div align="center">
  <img src="https://github-readme-stats.vercel.app/api?username=ENNCELADUS&show_icons=true&theme=default" alt="GitHub Stats" />
</div>

## Connect

- Email: [arwang@shanghaitech.edu.cn](mailto:arwang@shanghaitech.edu.cn)
- Homepage: https://ENNCELADUS.github.io
- GitHub: [@ENNCELADUS](https://github.com/ENNCELADUS)

<div align="center">
  <sub>📍 Shanghai · © 2026 Anrui Wang</sub>
</div>
