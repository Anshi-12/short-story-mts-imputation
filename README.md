# Filling the Gaps: Deep Learning for Multivariate Time Series Imputation

**CMPE 255 Data Mining | Short Story Assignment | Fall 2026**
**Author:** Anshita Shah, San José State University

This short story explains the survey paper:

> J. Wang, W. Du, Y. Yang, L. Qian, W. Cao, K. Zhang, W. Wang, Y. Liang, Q. Wen.
> **Deep Learning for Multivariate Time Series Imputation: A Survey.** IJCAI 2025 (Survey Track).
> Paper: [arXiv 2402.04059](https://arxiv.org/abs/2402.04059) | [PDF](https://arxiv.org/pdf/2402.04059) | [IJCAI proceedings](https://www.ijcai.org/proceedings/2025/1187)

---

## Deliverables

| Item | Link |
|---|---|
| Medium article | [Read on Medium](https://medium.com/@anshita.apps/deep-dive-into-deep-learning-for-time-series-imputation-6f7dad5b4554) |
| Slides (Google Slides) | [View slides](https://docs.google.com/presentation/d/1oF8cKZ2pXDeH4CORTRU-R7TJv-2ZOiTZZp-9wozYAxs/edit?usp=sharing) |
| Video walkthrough | [Watch on YouTube](https://youtu.be/zfN-qqSvUz0) |
## Repository structure

```
.
├── README.md
├── slides/
│   └── MTS_Imputation_Survey_Anshita_Shah.pptx   13 slides, speaker notes included
├── figures/
│   ├── fig1_missing_data_problem.png             what missing data looks like
│   ├── fig2_missingness_mechanisms.png           MCAR, MAR, MNAR
│   ├── fig3_taxonomy.png                         the survey's two-axis taxonomy
│   ├── fig4_two_pipelines.png                    impute-then-predict vs encode-and-predict
│   └── fig5_timeline.png                         how the field evolved
├── video/
│   └── video_script.md                           narration for the recorded talk
└── article/
    └── medium_article_outline.md                 structure and sources for the article
```

## The paper in one minute

**Problem.** Multivariate time series from hospitals, sensors and finance are full of gaps. In the PhysioNet 2012 ICU dataset, more than 80% of values are missing. Most models need complete input, so the gaps must be filled first (imputation). Classic fixes like mean filling or KNN ignore time dynamics and links between variables.

**Key concept.** Why a value is missing matters. Rubin's three mechanisms are MCAR (pure chance), MAR (depends on observed values) and MNAR (depends on the hidden value itself). MNAR is common in clinical data and is the hardest case.


**Taxonomy.** The survey organises more than 30 deep imputers on two axes: what they output (predictive, generative, large model) and which network they use (RNN, CNN, GNN, attention, VAE, GAN, diffusion, large models).


| Family | Example models | Strength | Weakness |
|---|---|---|---|
| RNN | GRU-D, BRITS | Natural fit for sequences | Slow on long series |
| CNN | TimesNet | Local patterns | Kernel size limits range |
| GNN | GRIN, SPIN | Links between sensors | Costly on large graphs |
| Attention | SAITS, ImputeFormer | Long-range, parallel; usually best | Data hungry |
| VAE | GP-VAE, V-RIN | Explicit probabilities | Limited generative power |
| GAN | E2GAN, NAOMI | Realistic outputs | Unstable training |
| Diffusion | CSDI, SSSD, PriSTI | Richest patterns plus uncertainty | Expensive |
| Large models | MOMENT, GPT4TS | Transfer from pre-training | Still early stage |

**Findings.**
1. Attention-based models generally outperform RNN and CNN models.
2. Diffusion models are the most expressive and give uncertainty, but they are computationally costly.
3. Most methods assume MCAR or MAR, while real data is often MNAR.
4. Impute-then-predict discards the missingness mask; end-to-end encode-and-predict is more promising.
5. Fair comparison needs shared tools: PyPOTS (37 imputation models) and TSI-Bench (34,804 experiments, 28 algorithms, 8 datasets).


**My take.** From my work on early sepsis detection with PhysioNet 2019, lab values are missing because clinicians order them only when worried. Missingness is a clinical signal, which makes the encode-and-predict argument especially convincing.

## References

1. J. Wang, W. Du, et al. *Deep Learning for Multivariate Time Series Imputation: A Survey.* IJCAI 2025. https://arxiv.org/abs/2402.04059
2. W. Du et al. *TSI-Bench: Benchmarking Time Series Imputation.* https://arxiv.org/abs/2406.12747
3. PyPOTS: https://github.com/WenjieDu/PyPOTS

All figures in this repository are original illustrations made for this assignment, based on the content of the survey.
