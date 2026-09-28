# Induction across a gap

A small, deliberately narrow mechanistic-interpretability experiment on pretrained GPT-2-small. The notebook identifies heads that attend to the token *after* an earlier occurrence of the current token in a repeated random-token sequence, then tests whether zeroing a selected head's output changes the repeated token's logit advantage as the distance to that earlier occurrence grows.

This is **scaffolding, not a completed study**. No model results have been run or endorsed here. Ethan supplies the prediction, executes the notebook, inspects the evidence, writes the analysis, and decides whether the results merit publication.

## Run on free Colab

1. Open `induction_gap.ipynb` in Google Colab. Runtime > Change runtime type > T4 GPU if available. A GPU is strongly recommended; no model training or paid compute is required.
2. Run all cells in order. The first cell installs exact direct-package versions. Restart the runtime if Colab asks, then rerun from the top. The notebook can also run as a standalone upload without cloning this folder.
3. First use `N_PER_SEED = 8`, `SEEDS = (11,)` as a smoke test. Then restore the declared settings for the actual run. Do not use smoke-test plots as results.
4. The notebook saves `results.csv`, `head_scores.csv`, `gap_plot.png`, and `run_metadata.json` locally. Download them from Colab's Files pane. Keep the raw data, output notebook, code version, runtime package versions, and failed runs together when making a public repo.
5. On CPU, lower batch size and expect a slower run. On free Colab, a GPU is subject to availability and session limits; the exact wall time has not been tested here.

## Prediction - Ethan fills this in before seeing held-out results

**Predicted direction and shape as gap grows:** [write your own falsifiable prediction]

**Primary metric and what result would change your mind:** [fill in]

**Date and saved version of the prediction:** [fill in before running the held-out gap sweep]

Do not edit a prediction after inspecting held-out results; add an explicitly dated post-hoc note instead.

## What the notebook measures

Prompt layout: `variable-length random prefix | fixed 32-token A | gap filler | repeat A`. The repeat starts at the same absolute position for every gap, so compared predictions share target position and total sequence length. The earlier source position varies by design. For repeat position `j`, the model predicts `A[j+1]`; the intended attention source is the *first* `A[j+1]`, not `A[j]`. The same A and foil token are paired across gap conditions within each seed/sample.

The discovery set chooses the head with the highest mean attention to this source, using gap 16. Disjoint held-out seeds are used for all reported effects. For each held-out example, the metric is the correct-token logit minus a fixed sampled foil-token logit. `clean - zero-ablation` is the selected head's contribution under that intervention. The notebook also measures median-attention control-head ablation, and a non-repeat prompt in which the earlier A is replaced while the repeat/target stays fixed. Repeated examples across gap conditions are paired, not independent. Uncertainty is a descriptive bootstrap across held-out prompts within each seed; this is not uncertainty across models.

## Limitations to fill after running

- [ ] Did performance differ on non-repeat controls? Does the selected head's effect actually differ from the control head's effect?
- [ ] Did attention and causal effect disagree? Attention-to-source is a discovery heuristic, not proof of a circuit.
- [ ] Could zero ablation be off-distribution? Consider a replacement/patching follow-up rather than overclaiming.
- [ ] Random synthetic tokens and a single model may not generalize to ordinary language or newer models.
- [ ] Is gap confounded with source position, length of intervening content, or token collisions? Target positions are matched, but source positions necessarily move.
- [ ] Check prior work before claiming novelty for this gap manipulation.

This does **not** reproduce the training-time phase change, the particular 2-layer attention-only model, or the broad in-context-learning claims in Olsson et al. Head discovery is exploratory. A positive ablation difference is evidence under this exact prompt distribution and intervention, not complete circuit faithfulness.

## Next steps, owned by Ethan

Read the paper and the TransformerLens induction-head demo; save a prediction. Run and debug the notebook. Examine plots and raw observations, check close prior art, then write an 800-1,200-word explanation in your own words. Publish a reproducible repo and, if it earns it, a LessWrong post. Do not claim Alignment Forum placement as automatic.

## Sources

- Olsson et al., [In-context Learning and Induction Heads](https://transformer-circuits.pub/2022/in-context-learning-and-induction-heads/index.html).
- [TransformerLens Main Demo: induction heads, attention hooks](https://transformerlensorg.github.io/TransformerLens/generated/demos/Main_Demo.html).
- [TransformerLens PyPI](https://pypi.org/project/transformer-lens/) (direct dependency pinned below).

## Files

- `induction_gap.ipynb`: self-contained Colab notebook and machinery.
- `requirements.txt`: pinned direct dependencies; PyTorch is supplied by Colab's GPU runtime and checked in the notebook.
- `SESSION_PLAN.md`: 16-hour session breakdown plus debugging buffer.
