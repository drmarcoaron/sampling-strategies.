# Sampling Strategies in Large Language Models

Welcome to **sampling-strategies**, an educational repository by [@marcoharuni](https://github.com/marcoharuni) dedicated to decoding strategies—the algorithms that turn probability distributions from large language models (LLMs) into actual words. This resource is inspired by the [YouTube explainer](#youtube-explainer) on how LLMs like ChatGPT decide which word to say next, and is intended for both learners and practitioners seeking a deeper, research-driven understanding of sampling in generative AI.

## Table of Contents

- [Introduction](#introduction)
- [Why Sampling Matters](#why-sampling-matters)
- [Core Decoding Strategies](#core-decoding-strategies)
    - [Greedy Decoding](#greedy-decoding)
    - [Random Sampling](#random-sampling)
    - [Top-K Sampling](#top-k-sampling)
    - [Top-P (Nucleus) Sampling](#top-p-nucleus-sampling)
    - [Temperature Scaling](#temperature-scaling)
    - [Min-P Sampling](#min-p-sampling)
    - [Repetition and Frequency Penalties](#repetition-and-frequency-penalties)
    - [Beam Search](#beam-search)
- [Practical Usage and Trade-offs](#practical-usage-and-trade-offs)
- [References](#references)
- [YouTube Explainer](#youtube-explainer)

---

## Introduction

Every time a large language model (LLM) like GPT-2, GPT-3, or ChatGPT generates text, it does so one token at a time, assigning a probability to each possible next token in its vocabulary. The model must then select one token to continue the sequence, and this choice is governed by a *decoding strategy* or *sampling method*. The configuration of this step profoundly affects the creativity, coherence, and diversity of generated text.

## Why Sampling Matters

If LLMs always picked the most probable next token, outputs would be repetitive and dull. Yet, pure randomness leads to incoherence. The goal is to balance predictability with creativity, and this is achieved via sampling strategies. These algorithms have a strong foundation in both statistical language modeling and contemporary deep learning literature.

## Core Decoding Strategies

### Greedy Decoding

- **Description:** Always selects the token with the highest probability.
- **Pros:** Fast, deterministic, and simple.
- **Cons:** Outputs are often repetitive and lack diversity, prone to loops (e.g., "I'm sorry, I'm sorry...").
- **Reference:** [Holtzman et al., 2020](https://arxiv.org/abs/1904.09751)

### Random Sampling

- **Description:** Selects the next token by sampling from the probability distribution as predicted by the model.
- **Pros:** High diversity and creativity, especially for short outputs.
- **Cons:** May produce incoherent or off-topic sequences; not robust for controlled generation.
- **Reference:** [Radford et al., 2019](https://cdn.openai.com/better-language-models/language_models_are_unsupervised_multitask_learners.pdf)

### Top-K Sampling

- **Description:** Considers only the top K most probable tokens, then samples from them.
- **Pros:** Balances randomness and control, reduces risk of low-probability (nonsensical) tokens.
- **Cons:** Fixed K may not adapt well to different contexts.
- **Reference:** [Fan et al., 2018](https://arxiv.org/abs/1805.04833)

### Top-P (Nucleus) Sampling

- **Description:** Instead of a fixed K, gathers the smallest set of tokens whose cumulative probability exceeds a threshold P (e.g., 0.9), then samples from this "nucleus."
- **Pros:** Adaptive, context-aware, and often yields more natural language.
- **Cons:** Can still be sensitive to the chosen threshold P.
- **Reference:** [Holtzman et al., 2020](https://arxiv.org/abs/1904.09751)

### Temperature Scaling

- **Description:** Adjusts the "sharpness" of the probability distribution before sampling. Lower temperature (<1) sharpens, higher temperature (>1) flattens.
- **Pros:** Intensifies or dampens randomness; can be combined with other methods.
- **Cons:** Extreme values can produce either repetitive or completely random outputs.
- **Reference:** [Ackley et al., 1985](https://www.sciencedirect.com/science/article/pii/0167278985900871)

### Min-P Sampling

- **Description:** A newer strategy that keeps all tokens whose probability is at least a fraction (e.g., 10%) of the top token's probability, adapting to the model's confidence.
- **Pros:** Dynamically adjusts the sampling pool, balancing coherence and diversity, especially effective at high temperatures.
- **Cons:** Parameter tuning can be less intuitive.
- **Reference:** [Goel et al., 2022](https://arxiv.org/abs/2206.06946), [implemented in Hugging Face Transformers](https://huggingface.co/docs/transformers/main/en/main_classes/text_generation#transformers.MinLengthLogitsProcessor)

### Repetition and Frequency Penalties

- **Description:** Apply penalties to tokens already generated, reducing the chance of repetition.
- **Pros:** Improves diversity in longer texts, avoids loops.
- **Reference:** [Keskar et al., 2019](https://arxiv.org/abs/1909.05858)

### Beam Search

- **Description:** Keeps multiple candidate sequences (beams) at each step, expanding and scoring them in parallel.
- **Pros:** More exhaustive, improves results in precision-demanding tasks (e.g., translation, summarization).
- **Cons:** Computationally expensive, less creative, can favor safe but bland outputs.
- **Reference:** [Sutskever et al., 2014](https://papers.nips.cc/paper/2014/hash/a14ac55a4f27472c5d894ec1c3c743d2-Abstract.html)

## Practical Usage and Trade-offs

- **Consistency:** Use greedy or low-temperature sampling.
- **Creativity:** Use top-P or min-P with higher temperature.
- **Precision (e.g., translation):** Use beam search.
- **Diversity:** Combine temperature scaling and repetition penalties.

> **Tip:** There's no universal best method; choice depends on your application and desired output characteristics.

## References

- Holtzman, A., Buys, J., Du, L., Forbes, M., & Choi, Y. (2020). [The Curious Case of Neural Text Degeneration](https://arxiv.org/abs/1904.09751)
- Fan, A., Lewis, M., & Dauphin, Y. (2018). [Hierarchical Neural Story Generation](https://arxiv.org/abs/1805.04833)
- Goel, K., Kulshreshtha, D., Wang, Y., & others. (2022). [Minimum Bayes Risk Decoding with Neural Metrics](https://arxiv.org/abs/2206.06946)
- Keskar, N. S., McCann, B., Varshney, L. R., Xiong, C., & Socher, R. (2019). [CTRL: A Conditional Transformer Language Model for Controllable Generation](https://arxiv.org/abs/1909.05858)
- Sutskever, I., Vinyals, O., & Le, Q. V. (2014). [Sequence to Sequence Learning with Neural Networks](https://papers.nips.cc/paper/2014/hash/a14ac55a4f27472c5d894ec1c3c743d2-Abstract.html)
- Ackley, D. H., Hinton, G. E., & Sejnowski, T. J. (1985). [A learning algorithm for Boltzmann machines](https://www.sciencedirect.com/science/article/pii/0167278985900871)
- Radford, A., Wu, J., Child, R., et al. (2019). [Language Models are Unsupervised Multitask Learners](https://cdn.openai.com/better-language-models/language_models_are_unsupervised_multitask_learners.pdf)

## YouTube Explainer

This repository is inspired by the [YouTube video explainer](https://www.youtube.com/) (replace with actual link), which demystifies LLM decoding strategies for a broad audience. The code and notebooks here allow you to experiment with these sampling techniques yourself—try them out and see how simple changes in decoding can transform the personality of your language model!

---

> **Author:** [Marco Haruni](https://github.com/marcoharuni)  
> *If you found this helpful, star the repo, share it, or leave a comment with your favorite sampling strategy!*
