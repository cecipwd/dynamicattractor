# Dynamic Attractor Network — Memory Formation, Reinforcement & Forgetting

This project is a computational-neuroscience reproduction and extension of **Boscaglia et al. (2023)**, *PLOS Computational Biology*
([paper](https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1011727)).

It investigates how memory assemblies dynamically evolve based on stimulus presentation frequency, and what specific conditions allow them to overlap to represent associated memories. In particular, we hypothesize that the creation of these overlapping assemblies is driven by the interplay between interleaved co-stimulation and either synaptic normalization fine-tuning or staggered delays.

To test this, we develop a 100-neuron attractor network across five milestones, sequentially integrating and evaluating dynamic mechanisms like Hebbian learning with forgetting term, neural adaptation, synaptic & divisive normalization, and background noise.

Reference implementation: [MartaBoscaglia/DynamicAttractorNetworkModel_2023](https://github.com/MartaBoscaglia/DynamicAttractorNetworkModel_2023).


---

## Repo contents

| File | What it is |
|---|---|
| `dynamic_attractor_network.ipynb` | Main notebook — all code and experiments (Milestones 1–5). |
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

While a fixed seed is used to generate the exact figures seen in the report, the actual hypothesis testing (such as the parameter sweeps in Milestone 5) calculates the Intersection over Union by averaging the results across 3 distinct random seeds. This ensures our core findings are statistically robust and not artifacts of a single network initialization.

Some full-length runs use large `total_time` values and take several minutes. Each is
preceded by a small **sanity-check** run; lower `total_time` there for a faster preview.



---

## How the results / figures were generated

Every figure comes straight from a notebook cell — run the section and the plot appears inline.

| Notebook section | Produces | Corresponds to (paper) |
|---|---|---|
| **Setup** | Parameters, stimulation schedule, helper functions and the core simulation loop — must run first | - |
| **Milestone 1 — Baseline model** | Firing-rate trace showing the assembly returns to baseline after stimulation, plus mean-weight evolution | Fig. 2 |
| **Milestone 2 — Full model** | Intra-assembly mean weight over time, weight growth sampled at each stimulation onset, and a firing-rate heatmap around the stimulation period | Fig. 4A |
| **Milestone 2 → Stability check** | Three-panel check: weight std plateaus, firing rates stay ≤ `r_max`, and the SR/SW normalization factors dip below 1 — with an automated pass/fail summary | Fig. 2B |
| **Milestone 3 — Frequency-dependent assembly growth** | Assembly growth across onset frequencies (1/30, 1/40, 1/60, 1/120), with a summary table | Fig. 6A |
| **Milestone 4 → Phase 1 — Assembly formation** | Growth of the three assemblies (P1–P3) from a blank weight matrix | Fig. 9A, top |
| **Milestone 4 → Phase 2 — Assembly competition** | Stitched Phase 1 + Phase 2 timeline of assembly sizes, plus mean incoming synaptic weight from P1 (synaptic recruitment) | Fig. 9A, Fig. 9B |
| **Milestone 5 — Overlapping assemblies: hypotheses 1** | Overlap metric (IoU) vs. co-presentation interval, swept across `factor_SW` values (0.85, 1.0, 1.5) over 3 seeds | - |
| **Milestone 5 — Overlapping assemblies: hypotheses 2** | Heatmap of the memory-overlap phase space: co-presentation interval × stagger delay | - |

---

## Main findings 

- **Relevance of temporal separation**: forming orthogonal memories strictly requires a temporal delay between stimuli. If distinct patterns are stimulated simultaneously, the network's Hebbian learning forces them to fuse into a single "super-assembly".
- **Memory competition**: frequently recalled memories expand by recruiting free background neurons and abandoned, forgotten memories' neurons. However, even rarely stimulated memories maintain enough synaptic strength to shield themselves from being overwritten by dominant ones.
- **Binary phase transition of overlaps**: it seems that the model's physics may prevent the formation of overlapping assemblies through co-stimulation. Modulating temporal co-presentation and synaptic normalization does not create overlaps; instead, it triggers a strict, all-or-nothing phase transition where memories either perfectly segregate or completely fuse.
- **Ineffectiveness of staggered delays**: we hypothesized that introducing a slight temporal stagger during co-presentation might overcome this binary limitation and form a memory bridge. However, a comprehensive 2D parameter sweep comparing co-presentation intervals against stagger delays produced no measurable partial overlap. This confirms that within the current constraints of the model, temporal manipulation alone cannot foster overlapping memories.

Full method, results, and limitations are in the technical note.

---

## Documentation of LLM Usage

We used Claude (Model Opus 4.8), Gemini (Model 3.1 Pro) and DeepSeek (Model 3) to aid us in producing the code, assist in the roadmap for the project and how to organize the presentation.

---

## Citation

Boscaglia, M. et al. (2023). *Dynamic attractor networks for memory formation, reinforcement and
forgetting.* PLOS Computational Biology.
