# Dynamic Attractor Network — Memory Formation, Reinforcement & Forgetting

Computational-neuroscience seminar project reproducing and extending
**Boscaglia et al. (2023)**, *PLOS Computational Biology*
([paper](https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1011727)).

We implement a **rate-based recurrent network of 100 neurons** that forms memory *assemblies*
through online Hebbian learning, and use it to study when two memories **fuse**, **stay separate**,
or settle into a **stable partial overlap** ("semantic bridge").

The model includes neural adaptation, online Hebbian learning with a forgetting term, Gaussian
background noise, and two normalization mechanisms — synaptic normalization (**SW**) and divisive
normalization (**SR**) — following the paper.

Reference implementation: [MartaBoscaglia/DynamicAttractorNetworkModel_2023](https://github.com/MartaBoscaglia/DynamicAttractorNetworkModel_2023).

---

## Repo contents

| File | What it is |
|---|---|
| `dynamic_attractor_network_colab.ipynb` | Main notebook — all code and experiments (Weeks 1–6). |
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
(Or with conda: `conda env create -f environment.yaml && conda activate dynamic-attractor`.)

**Run the cells top to bottom.** The **Setup & base model** block *must* run first — it defines the
parameters, the core simulation loop, and the helper functions every later week reuses.

Figures render **inline** as each plotting cell runs. A few cells also save `.png`/`.npy` artifacts
into `./attractor_network_results/`.

---

## Author Contributions

!!!
**TODO**
!!!

---

## Reproducibility

`RANDOM_SEED = 42` is fixed in the parameter cell, so a clean top-to-bottom run reproduces the
figures in the report and presentation. Set it to `None` for a fresh random run.

Some full-length runs (Weeks 4–6) use large `total_time` values and take several minutes. Each is
preceded by a small **sanity-check** run; lower `total_time` there for a faster preview.

---

## How the results / figures were generated

Every figure comes straight from a notebook cell — run the section and the plot appears inline.

| Notebook section | Produces | Corresponds to (paper) |
|---|---|---|
| **Setup & base model (Weeks 1–3)** | Firing-rate heatmap + weight-evolution summary of a single assembly | Fig. 2 |
| **Week 2 milestone** | Intra-assembly weight over time, sampled at each stimulation onset | Fig. 4A |
| **Week 3 milestone** | Stability check (bounded weights/rates + forgetting), with pass/fail summary | Fig. 2B |
| **Week 4** | Frequency-dependent assembly growth across onset frequencies | Fig. 4 |
| **Week 5** | Memory competition: reinforced vs. forgotten assemblies | Fig. 6 |
| **Week 5 (two-phase)** | Formation (Phase 1) then competition (Phase 2), plotted separately and combined | Fig. 6 |
| **Week 6 — overlapping memories** | Cross-assembly overlap from co-presentation | Fig. 9 |
| **Week 6 — functional bridge** | `factor_SW` × co-presentation sweeps + scalar overlap metric ("semantic bridge") | Suppl. Fig. S5 |
| **Week 6 — staggered onsets** | Bridge tuning via a small stagger delay between assembly onsets | Suppl. Fig. S5 |

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

We used Claude (Model Opus 4.8), Gemini (Model **XX**  ) and DeepSeek (Model **XX**  ) to aid us in producing the code, assist in the roadmap for the project and how to organize the presentation.
---

## Citation

Boscaglia, M. et al. (2023). *Dynamic attractor networks for memory formation, reinforcement and
forgetting.* PLOS Computational Biology.
