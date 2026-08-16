# Research-paper replication labs

Runnable experiments that adapt a paper's method to accessible open models while clearly separating new measurements from the paper's reported evidence.

## Lessons

- [01 — Chain-of-Thought Prompting Elicits Reasoning in Large Language Models](01-chain-of-thought-prompting.ipynb) — Wei et al. (NeurIPS 2022). An experimental replication lab using an open Qwen instruct model and a deterministic GSM8K sample: direct vs CoT prompts, paper-inspired ablations, checkpointed batched inference, paired uncertainty, cost metrics, and error inspection. Allow roughly 60–90 minutes on a free Colab T4; Python and basic prompting knowledge are enough.

  [![Open replication lab 01 in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/webpug/aiML/blob/main/research-notebooks/01-chain-of-thought-prompting.ipynb)

The lab requires a GPU and public Hugging Face downloads but no API key, gated model, or paid service. It faithfully replicates the experimental intervention on an accessible open model; it cannot exactly reproduce the proprietary LaMDA, PaLM, or GPT-3 runs from the paper.
