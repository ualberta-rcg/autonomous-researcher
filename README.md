<img src="./assets/ua_logo_green_rgb.png" alt="University of Alberta Logo" width="50%" />

# Autonomous Researcher

[![Python 3](https://img.shields.io/badge/Python-3-3776AB.svg)](https://www.python.org/)
[![SLURM](https://img.shields.io/badge/SLURM-Workflow-4EAA25.svg)](https://slurm.schedmd.com/)
[![vLLM](https://img.shields.io/badge/vLLM-GLM--OCR-8B5CF6.svg)](https://docs.vllm.ai/)
[![Corpus 1950–1980](https://img.shields.io/badge/Corpus-1950--1980-76B900.svg)](https://ntrs.nasa.gov/)

> **Computational archaeology — re-running 1950–1980 federal research that its authors never could.**

*Built on the [University of Alberta](https://www.ualberta.ca/en/information-services-and-technology/research-computing/index.html)/[Alberta Machine Intelligence Institute](https://www.amii.ca/) Vulcan Cluster*

**Maintained by:** Rahim Khoja ([khoja1@ualberta.ca](mailto:khoja1@ualberta.ca))

---

## 📖 Description

**Autonomous Researcher** harvests older (1950–1980) US federal research papers — NASA NTRS/NACA today, OSTI/NIST/DTIC planned — and re-simulates the analysis on modern HPC hardware.

This is **not a reproducibility project**. The target is work whose computation was *infeasible or un-conceived in its era*: hand-integrated equations, graphical solutions, relaxation methods, analyses that stopped at "a digital computer could evaluate this." We run what they couldn't.

The pipeline: **harvest → relevance filter → OCR → AI review → re-simulate → publish.** A 937 GB corpus of 96,321 NTRS papers (1950–1980, every year, sha256-verified in a manifest) feeds a GLM-OCR stage served through vLLM on fractional GPUs. A relevance filter scores papers up for theoretical/analytic/proposed-but-uncomputed work and down for "we ran X." Re-simulation follows the pattern proven in [example-using-agenticai](https://github.com/ualberta-rcg/example-using-agenticai) — one solver + one `sbatch` + one results dir per paper (see `naca-papers/` there for the Lessen 1950 Orr-Sommerfeld example).

The era cap at 1980 is deliberate: later years increasingly had the compute to simulate their own ideas. Older is richer.

## ✨ Features

- **96,321-paper corpus** — complete NTRS sweep 1950–1980, all research types, manifest with per-file `sha256`, page counts, and categories
- **Era-aware relevance filter** — prioritizes analytic/theoretical work over empirical test reports (wind-tunnel data doesn't re-simulate; Orr-Sommerfeld spectra do)
- **GLM-OCR via vLLM** — scanned typewriter-era pages to clean Markdown, equations rendered as LaTeX
- **SLURM-native** — self-chaining harvest jobs, page-sharded OCR arrays, fractional L40S slices; nothing runs where it shouldn't
- **Simulation-first layout** — every paper's solver, job script, results, and AI review live side-by-side in the corpus tree

## 📊 Pipeline Status

| Stage | State |
|-------|-------|
| NTRS harvest | ✅ done — 96,277 PDFs / 937 GB |
| Relevance filter | 🔜 next |
| OCR | 🔄 3,226 / 96k done (1950–53; vLLM pass timed out at 24 h, reshard pending) |
| OSTI / NIST / DTIC harvests | ⏳ planned |
| AI review | ⏳ planned |
| Re-simulation & publish | 🔄 pattern proven ([naca-papers](https://github.com/ualberta-rcg/example-using-agenticai)) |

## 🚀 Quickstart

The corpus (937 GB) lives on Vulcan project storage, not in this repo. On Vulcan:

```bash
git clone git@github.com:ualberta-rcg/autonomous-researcher.git
```

Live pipeline state: [`STATUS.md`](STATUS.md). The manifest is the source of truth for the corpus: one JSON line per paper with `doc_id`, `year`, `sha256`, `bytes`, `pages`, and `category`.

## 🔗 References

- [University of Alberta Research Computing](https://www.ualberta.ca/en/information-services-and-technology/research-computing/index.html)
- [Alberta Machine Intelligence Institute (AMII)](https://www.amii.ca/)
- [Digital Research Alliance of Canada](https://alliancecan.ca/)
- [NASA Technical Reports Server](https://ntrs.nasa.gov/)
- [Vulcan Docs](https://ualberta-rcg.github.io/ragflow-wiki-data/)

---

## 🤝 Support

"I am an instrument of God. I'm also one hell of a research computing tool." — somebody, probably.

This project is provided as-is. Feel free to open an issue or email **[khoja1@ualberta.ca](mailto:khoja1@ualberta.ca)**.

## 🧠 About University of Alberta Research Computing

The [Research Computing Group](https://www.ualberta.ca/en/information-services-and-technology/research-computing/index.html) supports high-performance computing, data-intensive research, and advanced infrastructure for researchers at the University of Alberta and across Canada.

We help design and operate compute environments that power innovation — from AI training clusters to national research infrastructure.
