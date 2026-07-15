# Dynamic Attractor Network — Memory Formation, Reinforcement & Forgetting

Computational-neuroscience seminar project reproducing and extending
**Boscaglia et al. (2023)**, *PLOS Computational Biology*
([paper](https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1011727)).

The reproduction and extension are addressed by two main research questions, respectively:
1. How do memory assemblies dynamically evolve based on the frequency of stimulus presentation?
2. Under what conditions can we support the creation of overlapping assemblies to represent associated memories?

To investigate these research questions, we develop five milestones:
- Milestone 1 & 2 implement a static **rate-based recurrent network of 100 neurons**, adding step-by-step the dynamic mechanisms (Hebbian learning with forgetting term, background noise, neural adaptation, synaptic & divisive normalization) and testing their effectiveness.
- Milestone 3 & 4 address the first research question. In particular, Milestone 3 tests the relationship between assembly growth and frequency of stimulation, whereas Milestone 4 investigates the formation, reinforcement and forgetting of 3 different assemblies with different stimulation frequency.
- Milestone 5 addresses the second research question. In particular, we develop 2 different hypotheses, investigating the interplay between interleaved co-stimulation and synaptic normalization firstly, and secondly the interplay between interleaved co-stimulation and "stagger delays".


Reference implementation: [MartaBoscaglia/DynamicAttractorNetworkModel_2023](https://github.com/MartaBoscaglia/DynamicAttractorNetworkModel_2023).


---

## Repo contents

| File | What it is |
|---|---|
| `dynamic_attractor_network.ipynb` | Main notebook — all code and experiments (Milestones 1–6). |
| `requirements.txt` | pip dependencies. |
| `technical_note.` | Method + results + limitations write-up. |

---

## How to run

**Option A — Google Colab (recommended).** Open the notebook via the *Open in Colab* badge at the
top and run all cells (`Runtime → Run all`). No install needed; Colab already ships `numpy` and
`matplotlib`.

**Option B — locally.**
```bash
python -m venv .venv
source .venv/bin/activate          # Windows: .venv\Scripts\activate
pip install -r requirements.txt
pip install jupyter                # if not already installed
jupyter notebook dynamic_attractor_network_colab.ipynb
```
(Or with conda: `conda env create -f environment.yaml && conda activate dynamic-attractor`.).

**Run the cells top to bottom.** The **Setup & base model** block *must* run first — it defines the
parameters, the core simulation loop, and the helper functions every later milestone reuses.

Figures render **inline** as each plotting cell runs. A few cells also save `.npy` artifacts
into `./attractor_network_results/`.

---

## Author Contributions

Alejandra Sagastume wrote the code for the milestones 1-2 and created the code repository.

Edith Aguado programmed milestone 3 and contributed to milestone 5.

Cecilia Impagliatelli wrote the code for milestone 4-5.

All authors contributed to project planning, the design of the final presentation and the
organization of the final code repository.

---

## Reproducibility

`RANDOM_SEED = 42` is fixed in the parameter cell, so a clean top-to-bottom run reproduces the
figures in the report and presentation. Set it to `None` for a fresh random run.

Some full-length runs use large `total_time` values and take several minutes. Each is
preceded by a small **sanity-check** run; lower `total_time` there for a faster preview.

---

## How the results / figures were generated

Every figure comes straight from a notebook cell — run the section and the plot appears inline.

| Notebook section | Produces | Corresponds to (paper) |
|---|---|---|
| **Setup** | Parameters, stimulation schedule, helper functions and the core simulation loop — must run first | - |
| **Milestone 1 — Baseline model** | Firing-rate trace showing the assembly returns to baseline after stimulation, plus mean-weight evolution | Fig. 2 |
| **Milestone 2 — Full Model** | Intra-assembly mean weight over time, weight growth sampled at each stimulation onset, and a firing-rate heatmap around the stimulation period | Fig. 4A |
| **Milestone 2 → Stability Check** | Three-panel check: weight std plateaus, firing rates stay ≤ `r_max`, and the SR/SW normalization factors dip below 1 — with an automated pass/fail summary | Fig. 2B |
| **Milestone 3 — Frequency-dependent assembly growth** | Assembly growth across onset frequencies (1/30, 1/40, 1/60, 1/120), with a summary table | Fig. 4 |
| **Milestone 4 → Phase 1 — Assembly formation** | Growth of the three assemblies (P1–P3) from a blank weight matrix | Fig. 6 |
| **Milestone 4 → Phase 2 — Assembly competition** | Stitched Phase 1 + Phase 2 timeline of assembly sizes, plus mean incoming synaptic weight from P1 (synaptic recruitment) | Fig. 6, Fig. 9B |
| **Milestone 5 — Overlapping assemblies** | Overlap metric (IoU) vs. co-presentation interval — the memory phase transition — swept across `factor_SW` values (0.85, 1.0, 1.5) over 3 seeds | - |
| **Milestone 5 → Functional bridge with staggered onsets** | Heatmap of the memory-overlap phase space: co-presentation interval × stagger delay | - |

---

## Main findings 

- Fusion is driven by **direct Hebbian potentiation of cross-assembly weights during external
  co-stimulation**, not by internally driven recruitment.
- The synaptic-normalization parameter (`factor_SW`) does **not** prevent fusion across its tested
  range — it targets internal recurrent activity, not externally co-driven co-firing. Divisive
  normalization (`Factor_SR`) is a distinct but similarly indirect lever; pushing either too hard
  destroys assemblies rather than partitioning them.
- Overlap metrics show **bistable** (all-or-nothing) fusion/separation; the most promising levers for
  a *graded* partial overlap are **co-presentation frequency** and the **forgetting rate `beta`**.

Full method, results, and limitations are in the technical note.

---

## Documentation of LLM Usage

We used Claude (Model Opus 4.8), Gemini (Model 3.1 Pro) and DeepSeek (Model 3) to aid us in producing the code, assist in the roadmap for the project and how to organize the presentation.

---

## Citation

Boscaglia, M. et al. (2023). *Dynamic attractor networks for memory formation, reinforcement and
forgetting.* PLOS Computational Biology.
