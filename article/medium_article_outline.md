# Medium Article Kit: write it in your own words

The rubric requires the Medium article to be written from scratch, without AI, and Turnitin is used. This kit gives you the skeleton, the facts to cover, and where each figure and table goes. **Write every paragraph yourself.** Target length: 1,500 to 2,000 words (about 7 to 9 minutes of reading on Medium).

---

## Suggested title and subtitle (pick or rewrite)

- Title: *Filling the Gaps: How Deep Learning Learned to Handle Missing Data in Time Series*
- Subtitle: *A tour of the IJCAI 2025 survey on multivariate time series imputation, and why the missing values themselves might be the most useful signal*

Tags for Medium: Data Mining, Machine Learning, Time Series, Healthcare AI, Deep Learning

---

## Section 1. Hook (150 to 200 words)

**Image:** `figures/fig1_missing_data_problem.png`
Caption idea: "A toy ICU record. Filled circles were recorded; hollow ones are missing."

Points to cover:
- Open with a concrete scene: a patient chart where lab values appear only a few times a day.
- Fact: in PhysioNet 2012, more than 80% of values are missing.
- Other domains: IoT sensors, finance, traffic.
- Define imputation in one sentence.

## Section 2. Why the easy fixes fail (150 words)

Points to cover:
- Mean filling, last observation carried forward, KNN, MICE.
- They ignore time dynamics and cross-variable links.
- One example sentence of your own about why that matters (for example, BP and heart rate moving together).

## Section 3. About the paper (100 words)

Facts:
- *Deep Learning for Multivariate Time Series Imputation: A Survey*, Jun Wang, Wenjie Du et al.
- Published at IJCAI 2025, survey track. arXiv: https://arxiv.org/abs/2402.04059
- Authors are from PyPOTS Research, HKUST, Oxford, King's College London and Huawei Noah's Ark Lab.
- Purpose: organise deep imputation methods, compare trade-offs, point to fair benchmarks, list open problems.

## Section 4. The question before any model: why is it missing? (250 words)

**Image:** `figures/fig2_missingness_mechanisms.png`

Points to cover:
- MCAR, MAR, MNAR, each with your own everyday example.
- Ignorable vs non-ignorable.
- Why MNAR is dangerous: biased estimates that normal metrics will not reveal.

## Section 5. The two-axis taxonomy (250 words)

**Image:** `figures/fig3_taxonomy.png`

Points to cover:
- Axis 1: predictive vs generative vs large model. Explain "uncertainty" plainly.
- Axis 2: architecture family.
- Observation: RNN/CNN/GNN/attention sit on the predictive side; VAE/GAN/diffusion are generative by nature.

## Section 6. Families at a glance (table + 150 words)

Paste this table into Medium (Medium has no native tables, so either screenshot slide 7 of the deck or use a GitHub Gist with this markdown):

| Family | Example models | Strength | Weakness |
|---|---|---|---|
| RNN | GRU-D, BRITS | Natural fit for sequences | Slow on long series |
| CNN | TimesNet | Local patterns | Kernel size limits range |
| GNN | GRIN, SPIN | Links between sensors | Costly on large graphs |
| Attention | SAITS, ImputeFormer | Long-range, parallel; usually best | Data hungry |
| VAE | GP-VAE, V-RIN | Explicit probabilities | Limited generative power |
| GAN | E2GAN, NAOMI | Realistic outputs | Unstable training |
| Diffusion | CSDI, SSSD, PriSTI | Richest patterns plus uncertainty | Expensive; boundary clashes |
| Large models | MOMENT, GPT4TS | Transfer from pre-training | Still early stage |

Then briefly describe 3 or 4 models in your own words: GRU-D (time decay), BRITS (bidirectional, gaps as variables), SAITS (masked self-attention, two training tasks), CSDI (first diffusion imputer).

**Optional image:** `figures/fig5_timeline.png` to show how the field moved from RNNs to foundation models.

## Section 7. What the survey concludes (250 words)

Five findings to explain:
1. Attention generally beats RNN and CNN.
2. Diffusion is the most expressive and gives uncertainty, but it is costly.
3. Most methods assume MCAR/MAR; real data is often MNAR.
4. Impute-then-predict throws away information; encode-and-predict is more promising.
5. Comparisons were inconsistent, so PyPOTS (37 models) and TSI-Bench (34,804 experiments, 28 algorithms, 8 datasets) exist.

**Image:** `figures/fig4_two_pipelines.png` to illustrate point 4.

## Section 8. Open problems (100 words)

MNAR handling, scalability, downstream evaluation, LLMs with domain knowledge and multimodal inputs.

## Section 9. My two cents (250 to 300 words) ← the most important section for marks

Write this entirely from your own experience. Prompts:
- In your sepsis work on PhysioNet 2019, how sparse were the labs? What did you notice?
- Why "missingness is a clinical signal": a lab is ordered when a clinician is worried.
- Where you agree with the authors (encode-and-predict).
- What you wish the survey did more of (for example, evaluation on real decisions like alert timing, not only reconstruction error).
- One idea you would try next.

## Section 10. Wrap-up (100 words)

Three takeaways, link to your GitHub repo, slides and video.

---

## References to list at the end

1. J. Wang, W. Du, et al. *Deep Learning for Multivariate Time Series Imputation: A Survey.* IJCAI 2025. https://arxiv.org/abs/2402.04059
2. W. Du et al. *TSI-Bench: Benchmarking Time Series Imputation.* https://arxiv.org/abs/2406.12747
3. PyPOTS library: https://github.com/WenjieDu/PyPOTS

## Before you publish: checklist

- [ ] Every paragraph written by you (rubric: no AI, Turnitin is used)
- [ ] At least 4 images, each with a caption
- [ ] The families table included
- [ ] "My two cents" section is personal and specific
- [ ] Links to the paper, GitHub repo, slides and video
- [ ] Paste the Medium URL into `README.md`
