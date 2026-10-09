<img src="./assets/ua_logo_green_rgb.png" alt="University of Alberta Logo" width="50%" />

# Autonomous Researcher

[![Python 3](https://img.shields.io/badge/Python-3-3776AB.svg)](https://www.python.org/)
[![SLURM](https://img.shields.io/badge/SLURM-Workflow-4EAA25.svg)](https://slurm.schedmd.com/)
[![Corpus 1950–1980](https://img.shields.io/badge/Corpus-1950--1980-76B900.svg)](https://ntrs.nasa.gov/)

> **Autonomous computational archaeology — re-running 1950–1980 federal research that its authors never could.**

*Built on the [University of Alberta](https://www.ualberta.ca/en/information-services-and-technology/research-computing/index.html)/[Alberta Machine Intelligence Institute](https://www.amii.ca/) Vulcan Cluster*

**Maintained by:** Rahim Khoja ([khoja1@ualberta.ca](mailto:khoja1@ualberta.ca))

---

## 📖 Description

**Autonomous Researcher** is an autonomous pipeline that harvests older (1950–1980) US federal research papers and re-simulates their analysis on modern HPC hardware.

This is **not a reproducibility project**. The target is work whose computation was *infeasible or un-conceived in its era*: hand-integrated equations, graphical solutions, relaxation methods, analyses that stopped at "a digital computer could evaluate this." We run what they couldn't.

The pipeline: **harvest → relevance filter → OCR → AI review → re-simulate → publish.** A 937 GB corpus of 96,321 papers (NASA NTRS/NACA 1950–1980, OSTI/NIST/DTIC planned) feeds a GLM-OCR stage served through vLLM on fractional GPUs. A relevance filter scores papers up for theoretical/analytic/proposed-but-uncomputed work and down for "we ran X" — wind-tunnel data doesn't re-simulate; Orr-Sommerfeld spectra do.

The corpus itself lives on Vulcan project storage, not in this repo — the manifest is the source of truth: one JSON line per paper with `doc_id`, `year`, `sha256`, `bytes`, `pages`, and `category`. This repo holds the pipeline that operates on it.

The re-simulation stage follows the pattern proven in [example-using-agenticai](https://github.com/ualberta-rcg/example-using-agenticai) — one solver + one `sbatch` + one results dir per paper. The era cap at 1980 is deliberate: later years increasingly had the compute to simulate their own ideas. Older is richer.

## ✨ Features

- **Full-Era Harvest** — complete NTRS sweep 1950–1980, all research types, sha256-verified manifest as single source of truth
- **Era-Aware Relevance Filter** — scores analytic/theoretical work over empirical test reports before compute is spent on OCR
- **GLM-OCR via vLLM** — scanned typewriter-era pages to clean Markdown, equations rendered as LaTeX
- **SLURM-Native** — self-chaining harvest jobs, page-sharded OCR arrays, fractional L40S slices
- **Simulation-First Layout** — every paper's solver, job script, results, and AI review live side-by-side
- **Autonomous Stages** — each stage gates the next; nothing runs before its filter passes
- **Framework Agnostic** — scipy-stack solvers today; any cluster-runnable analysis per paper
- **Publish-Ready** — new results and AI reviews land in a citable per-paper bundle

## 🚀 Quickstart

This repo holds the pipeline; the corpus lives on Vulcan project storage.

### The Pipeline

```bash
git clone git@github.com:ualberta-rcg/autonomous-researcher.git
# pipeline stages land here as they come online — see STATUS.md
```

### Re-Simulating a Paper

```bash
# One paper = one solver + one sbatch + one results dir
# (pattern proven in example-using-agenticai/naca-papers)
sbatch <doc_id>.sh   # runs ../sim/<doc_id>.py, writes results/<doc_id>/
```

## 📚 Documentation

| Document | Description |
|----------|-------------|
| [example-using-agenticai](https://github.com/ualberta-rcg/example-using-agenticai) | Proven re-simulation pattern (`naca-papers/`) this pipeline builds on |
| [Vulcan Docs](https://ualberta-rcg.github.io/ragflow-wiki-data/) | Cluster documentation |

## 🔗 References

- [University of Alberta Research Computing](https://www.ualberta.ca/en/information-services-and-technology/research-computing/index.html)
- [Alberta Machine Intelligence Institute (AMII)](https://www.amii.ca/)
- [Digital Research Alliance of Canada](https://alliancecan.ca/)
- [NASA Technical Reports Server](https://ntrs.nasa.gov/)
- [Vulcan Docs](https://ualberta-rcg.github.io/ragflow-wiki-data/)

---

## 🤝 Support

Many Bothans died to bring us this information. This project is provided as-is, but reasonable questions may be answered based on my coffee intake or mood. ;)

Feel free to open an issue or email **[khoja1@ualberta.ca](mailto:khoja1@ualberta.ca)** or **[kali2@ualberta.ca](mailto:kali2@ualberta.ca)** for U of A related deployments.

## 🧠 About University of Alberta Research Computing

The [Research Computing Group](https://www.ualberta.ca/en/information-services-and-technology/research-computing/index.html) supports high-performance computing, data-intensive research, and advanced infrastructure for researchers at the University of Alberta and across Canada.

We help design and operate compute environments that power innovation — from AI training clusters to national research infrastructure.
