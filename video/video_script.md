# Video Script (about 11 minutes, within the 10 to 15 minute limit)

Read at a relaxed pace, about 130 words per minute. Each section matches one slide in `slides/MTS_Imputation_Survey_Anshita_Shah.pptx`; the same text is in the speaker notes of each slide. Record with Zoom or OBS: share the slides full screen, camera in the corner.

**Tip:** do not read word for word. Glance at a section, then say it in your own way. It will sound far more natural.

## Slide 1: Title

Hi everyone, I'm Anshita Shah, and this is my short story for CMPE 255, Data Mining. The paper I chose is "Deep Learning for Multivariate Time Series Imputation: A Survey" by Jun Wang, Wenjie Du and their colleagues, published in the IJCAI 2025 survey track. In the next twelve or so minutes I'll explain what problem this paper tackles, how the authors organise a very crowded field of deep learning methods, what they concluded, and finally what I personally took away from it, given that I work with clinical data that is full of gaps.

## Slide 2: Why old fixes fall short

Let me start with the problem. Almost every real dataset recorded over time has holes in it. In a hospital, vitals like heart rate are captured often, but lab values like lactate show up only a few times a day. In the PhysioNet 2012 ICU dataset, which the survey cites, more than eighty percent of the values are missing. Sensors fail, networks drop packets, and privacy rules hide fields. Most machine learning models cannot accept a table with holes in it, so somebody has to fill those holes first. That step is called imputation. The survey's goal is to organise the many deep learning methods for doing this, compare their trade-offs, and point to fair benchmarks.

The traditional remedies are easy to reach for. Mean filling replaces every gap with the column average. Last observation carried forward just copies the previous value. KNN borrows from similar rows, and MICE runs chained regressions. They are fast and cheap, which is why everyone uses them. The trouble is that they are myopic. They treat each variable mostly in isolation and ignore how values evolve through time and how variables move together. A heart rate spike and a falling blood pressure tell a story together, and these methods cannot hear it. The result is data that looks tidy but quietly misleads whatever model you train on top of it.

## Slide 3: Why is the value missing?

Before any model, the paper asks a deceptively simple question: why is this value missing? It uses Rubin's three classic categories. MCAR, missing completely at random, means the gap has nothing to do with any value, like a sensor lead slipping off by chance. MAR, missing at random, means the gap depends on things you can observe, for example fewer readings at night when time is recorded. MNAR, missing not at random, is the tricky one. The gap depends on the hidden value itself. A doctor orders a lactate test because the patient already looks unwell. MCAR and MAR are called ignorable, so a model can learn from what is observed. MNAR is non-ignorable, and if you ignore it your estimates become biased in a way your metrics may never reveal.

## Slide 4: Taxonomy

Now the heart of the survey: how to organise more than thirty deep imputers. The authors use two axes instead of one long list. The columns are about what the model outputs. Predictive methods give one best guess per missing value. Generative methods learn a whole distribution, so you can sample several plausible fills and measure uncertainty. Large model methods reuse pre-trained foundation models or language models. The rows are about the network architecture: RNNs, CNNs, graph networks, attention, VAEs, GANs, diffusion models and large models. You can see the clustering. Recurrent, convolutional, graph and attention models sit mostly on the predictive side, while VAEs, GANs and diffusion models are generative by nature.

## Slide 5: Families compared

This table summarises what the survey says about each family. RNNs fit sequences naturally, but they process step by step, so they are slow on long series. CNNs catch local patterns but are limited by kernel size. Graph networks model how sensors relate to each other, at a high computational cost on big graphs. Attention models are generally the strongest for imputation because they see long-range dependencies and train in parallel. VAEs give explicit probabilities but limited generative power. GANs produce realistic outputs but are notoriously unstable to train. Diffusion models capture the most complex patterns but are expensive and can produce fills that clash with neighbouring observed values. Large models bring transfer from pre-training, but this area is still young.

## Slide 6: Four models worth knowing

Let me spotlight four models that come up again and again. GRU-D, from 2018, modifies a recurrent unit so that the influence of an old reading decays based on how long ago it was seen. That idea of using time gaps as an input is very intuitive for clinical data. BRITS treats missing values as variables inside a bidirectional recurrent graph, and it can do classification at the same time. SAITS uses diagonally masked self-attention and trains on two objectives together, masked imputation and reconstruction. And CSDI was the first diffusion model designed specifically for time series imputation. It starts from noise and gradually denoises toward plausible values, conditioned on what was observed.

## Slide 7: How the field evolved

Putting these on a timeline shows the arc of the field. Around 2018 it was recurrent networks and the first GANs. 2020 brought VAEs with Gaussian process priors. 2021 was the arrival of diffusion with CSDI. 2022 saw graph networks like GRIN for sensor networks. By 2023 attention models like SAITS and TimesNet were strong, and language models such as GPT4TS were being adapted. And in 2024, time series foundation models like MOMENT and Timer joined in. So the field has moved from hand-crafted recurrent tricks toward large, general, pre-trained models.

## Slide 8: What the survey concludes

What did the authors conclude? Since this is a survey, these are judgments across the whole field rather than one experiment. One: attention-based models generally beat RNNs and CNNs. Two: generative models, especially diffusion, are the most expressive and give uncertainty, but they cost a lot to run. Three: most methods quietly assume MCAR or MAR, while real data, especially clinical data, is often MNAR. That mismatch is the biggest gap in the field. Four: the usual approach of imputing first and predicting second throws information away. And five: comparisons across papers were inconsistent, which is why they built shared tools.

## Slide 9: Impute then predict, or encode and predict?

This diagram explains point four. In the two-stage pipeline, you fill the gaps, hand the now complete data to a classifier, and the mask that told you what was missing is thrown away. In the end-to-end approach, the model receives the data together with the mask and the time gaps, encodes them jointly, and predicts directly. The authors argue the second path is more promising, especially when the pattern of missingness itself carries meaning. In a hospital, the fact that a doctor ordered a test is information.

## Slide 10: Tools and benchmark

To make comparisons fair, the authors point to two resources from their own group. PyPOTS is a Python toolbox for partially observed time series, with thirty-seven imputation models at the time of writing. TSI-Bench is a benchmark suite built on it, covering 34,804 experiments, 28 algorithms and 8 datasets with varied missingness patterns. For anyone wanting to try these models, PyPOTS is the obvious starting point, and that is what I would use for a small experiment.

## Slide 11: Open challenges

The survey closes with open problems. Handling MNAR data properly is first. Scaling deep imputers to very large datasets is second, since they are much heavier than statistical methods. Third is judging imputation by how well downstream tasks perform, not only by reconstruction error. And fourth is using large language models that carry domain knowledge, priors about missingness and multimodal inputs like clinical notes.

## Slide 12: My take

Here is my own take. I work on early sepsis detection with Transformers on the PhysioNet 2019 dataset, so this problem is personal. In that data, lab values are mostly missing, and they are not missing at random. Clinicians order labs when they are worried. That makes the encode-and-predict argument very convincing to me. Throwing away the mask is throwing away a clinician's suspicion. One thing I wish the survey did more of is clinical evaluation: reporting how imputation choices change real decisions like early alerts, not just error on held-out values. And I think the biggest practical wins will come from methods that treat MNAR as the default rather than the exception.

## Slide 13: Takeaways

To wrap up. Missing data is the norm, not the exception. Always ask why a value is missing before choosing a method. Attention models are a strong default, and diffusion models are powerful when you need uncertainty and can afford the compute. And whenever missingness carries meaning, keep the mask as a feature. The paper link, my Medium article and the slides are all in my GitHub repository. Thank you for watching.

---
_Total: about 1404 words, roughly 11 minutes at a natural pace._
