# SymPO: Consistency by Construction via Permutation Symmetry for Preference Optimization

**MRAC '26 @ ACM Multimedia 2026 — Oral** · November 10–14, 2026 · Rio de Janeiro, Brazil
Paper: to appear in the ACM Digital Library (DOI `10.1145/3840474.3840539`, active once the proceedings are published) · [Code](https://github.com/xinyou1220/MER_2026_TRACK3)

Meng-Ching Chen\*, Xin-You Song\*, Sheng-Bi Chen\*, Chia-Yen Lee†
National Yang Ming Chiao Tung University, Hsinchu, Taiwan
<sub>\* equal contribution · † corresponding author</sub>

## Overview

Pairwise preference judges should give the same answer when the two candidates are shown in the other order, yet learned judges routinely break this, and reinforcement learning amplifies it. **SymPO** enforces candidate-swap symmetry by construction at four stages:

1. **Rollouts** — every pair is rolled out in both orders in the same batch (balanced swap augmentation).
2. **Reward** — a ±1 correctness signal plus a bounded auxiliary envelope `τ·tanh(·)` holding a label-free majority-vote swap-consistency term, an out-of-fold Bradley–Terry margin and a length penalty, with a hard invalid-output floor.
3. **Update** — critic-free REINFORCE++ with a group-mean baseline and global advantage normalization.
4. **Inference** — constrained scoring averaged over both orders, i.e. the exact antisymmetric projection ½ + A(c₀, c₁), so the deployed decision is order-invariant.

Evaluated on **MER2026 Track 3 (MER-Prefer)**, an audio–visual emotion-preference benchmark, with LoRA fine-tuning of Qwen2.5-Omni-7B.

## Main results (test WAF-2, from Table 1)

| Method | WAF-2 |
|---|---:|
| One-shot direct prompting | 84.71 |
| SFT (swap augmentation) | 90.51 ± 0.45 |
| Naive RL + antisym. inference | 90.24 ± 1.47 |
| **SymPO-Bounded** | **90.59 ± 1.19** |
| SymPO-Bounded, Qwen3-Omni-30B-A3B backbone | 91.56 |

Mean ± s.d. over three seeds. The paper's central finding is that the deployed score says little about the model's intrinsic order dependence (rank correlation ρ = −0.11 between pre-projection position bias and score); see §5.4 and Table 3.

## Code

- **Training and evaluation code (MER 2026 Track 3):** https://github.com/xinyou1220/MER_2026_TRACK3
  - Quick start: see "Setup" and "Reproducing the results" in its README.
  - Run SymPO: `python scripts/run_experiment.py --experiment sympo --seed 42`
  - Paper table rows and experiment names: the "Baselines and ablations" table in its README.

## Citation

```bibtex
@inproceedings{chen2026sympo,
  title     = {SymPO: Consistency by Construction via Permutation Symmetry for Preference Optimization},
  author    = {Chen, Meng-Ching and Song, Xin-You and Chen, Sheng-Bi and Lee, Chia-Yen},
  booktitle = {Proceedings of the 4th International Workshop on Multimodal, Generative and Responsible Affective Computing (MRAC '26)},
  year      = {2026},
  address   = {Rio de Janeiro, Brazil},
  publisher = {ACM},
  doi       = {10.1145/3840474.3840539}
}
```

The paper is licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/).
