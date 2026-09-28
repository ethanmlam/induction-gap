# Six sessions, 16 hours + 3-5 hours to debug

| Session | Hours | Finish line |
| --- | ---: | --- |
| Paper + prediction | 2 | Read the narrow Olsson result and TransformerLens induction demo. Write and date your prediction before held-out runs. |
| Dataset + head discovery | 3 | Inspect the synthetic prompts, indexing and attention stripe; select the head using only discovery data. |
| Ablations + controls | 3 | Verify intervention affects exactly one head. Compare clean, selected-head ablation, control-head ablation and non-repeat prompts. |
| Gap sweep + plots | 3 | Run held-out seeds across 16/64/128/256 gaps; inspect raw data, paired trends and uncertainty. |
| Your writeup | 3 | Draft 800-1,200 words: hypothesis, method, results, controls, caveats, next question. A null result is reportable. |
| Repo polish | 2 | Save executed notebook, exact versions, raw CSV and figures; independently rerun a small batch from clean Colab. |

Budget another 3-5 hours for runtime/debugging. Do not declare the study finished from an unexecuted scaffold. Avoid changing the prediction or analysis criteria after seeing held-out results without a labeled post-hoc note.
