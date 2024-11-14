---
layout: main
title: "Introduction"
---

---

The nf-core/scflow pipeline enables fully automated, reproducible analyses of single-cell data at scale in the Cloud (GCP/AWS), in a high-performance computing (HPC) environment, or on a local workstation using the scFlow toolkit.

nf-core/scflow is a NextFlow (http://www.nextflow.io) pipeline targeted for curation with nf-core (https://nf-co.re/). Key features include: -

- Single-line execution of a complete analysis following best-practices

- Analytical steps are parallelized and distributed efficiently at scale on a High Performance Computing (HPC) environment, in the Cloud (e.g. GCP, AWS), or on a local workstation.

- Smart cache allows parameters to be fine-tuned and analyses resumed.

- Input parameters are standardized and documented.

- The pipeline is version controlled for reproducibility.

- The analysis environment is provided by version controlled Docker/Singularity images.

- Pipeline outputs are standardized and include publication-quality plots, tables, and interactive reports.

[<img src="{{ "/assets/img/nf-core-scflow_logo_light.png" | relative_url }}" width="340">](https://github.com/combiz/nf-core-scflow)
