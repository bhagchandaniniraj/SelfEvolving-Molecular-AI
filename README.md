# MOSAIC

### Molecular Optimization through Scoring, Active Learning, and Iterative Consolidation

```{=html}
<p align="center">
```
`<strong>`{=html}A computational research framework for molecular
representation learning, candidate generation, uncertainty-aware
prioritization, and continual-learning evaluation.`</strong>`{=html}
```{=html}
</p>
```
```{=html}
<p align="center">
```
`<img alt="Python" src="https://img.shields.io/badge/Python-Jupyter-blue?logo=python">`{=html}
`<img alt="PyTorch" src="https://img.shields.io/badge/Deep%20Learning-PyTorch-ee4c2c?logo=pytorch">`{=html}
`<img alt="Chemistry" src="https://img.shields.io/badge/Chemistry-RDKit-2f855a">`{=html}
`<img alt="Workflow" src="https://img.shields.io/badge/Workflow-Phases%201%E2%80%9332-5b5bd6">`{=html}
`<img alt="Status" src="https://img.shields.io/badge/Status-Research%20Prototype-orange">`{=html}
`<img alt="Evidence" src="https://img.shields.io/badge/Evidence-Computational%20Only-blueviolet">`{=html}
```{=html}
</p>
```
> **Research status:** MOSAIC is an in-silico research prototype. Its
> recorded experiments demonstrate native XML data processing, molecular
> generation and ranking, experimental EWC adaptation, latent-space
> analysis, and computational evidence auditing. They do **not**
> establish clinical efficacy, experimentally measured binding affinity,
> a consistent EWC advantage, or successful promotion of an adapted
> model. The authoritative model in the recorded runs remains **K0**.

------------------------------------------------------------------------

## Contents

-   [Overview](#overview)
-   [Research goals](#research-goals)
-   [Architecture](#architecture)
-   [Recorded results](#recorded-results)
-   [Project phases](#project-phases)
-   [Repository layout](#repository-layout)
-   [Requirements](#requirements)
-   [Data access and licensing](#data-access-and-licensing)
-   [Installation](#installation)
-   [Configuration](#configuration)
-   [Running the notebook safely](#running-the-notebook-safely)
-   [Outputs and provenance](#outputs-and-provenance)
-   [Reproducibility](#reproducibility)
-   [Scientific limitations](#scientific-limitations)
-   [Troubleshooting](#troubleshooting)
-   [Responsible use](#responsible-use)
-   [License and citation](#license-and-citation)

------------------------------------------------------------------------

## Overview

**MOSAIC** stands for **Molecular Optimization through Scoring, Active
Learning, and Iterative Consolidation**.

It is a research implementation of a computer-based molecular-design
workflow that combines:

-   Native DrugBank XML processing.
-   Molecular structure extraction and chemical standardization.
-   Attributed molecular graph preparation with three-dimensional
    coordinates.
-   An EGNN-style geometry-aware encoder and a 32-dimensional latent
    representation.
-   A SELFIES-based molecular decoder.
-   In-silico chemical scoring and model-derived biological relevance.
-   Uncertainty-aware candidate prioritization and diversity-aware
    selection.
-   Experimental continual-learning evaluation using Elastic Weight
    Consolidation (EWC).
-   Source-integrity, provenance, and scientific-evidence audits.

The project is implemented as a phased Jupyter notebook. Earlier phases
build the data and model pipeline; later phases consolidate recorded
results and audit their provenance. The final audit phases are designed
to read existing outputs rather than repeat expensive experiments.

### What MOSAIC is---and is not

MOSAIC is a **computational research prototype**, not a clinically
validated drug-discovery platform. Its scores are used to prioritize
hypotheses for further investigation. They do not replace experimental
pharmacology, medicinal chemistry, toxicology, or regulatory assessment.

## Research goals

  -----------------------------------------------------------------------
  Goal                                Approach
  ----------------------------------- -----------------------------------
  Reduce ingestion complexity         Process native XML records without
                                      requiring an intermediate
                                      relational database

  Prepare molecular inputs            Extract structures, validate and
                                      standardize molecules, and
                                      construct graph representations

  Learn molecular representations     Encode attributed molecular graphs
                                      into a 32-dimensional latent space

  Generate candidates                 Decode latent samples into SELFIES
                                      molecular strings

  Prioritize candidates               Combine the historical
                                      multiobjective acquisition policy
                                      with uncertainty and
                                      diversity-aware selection

  Study knowledge retention           Evaluate experimental parameter
                                      drift, reference-prediction drift,
                                      latent displacement, and EWC
                                      penalties

  Preserve research evidence          Record source run IDs, hashes,
                                      manifests, metric populations, and
                                      scientific limitations
  -----------------------------------------------------------------------

## Architecture

``` mermaid
flowchart TD
    A[DrugBank XML] --> B[XML/XSD Structure Inspection]
    B --> C[Streaming Record Extraction]
    C --> D[Molecular Structures]
    C --> E[Available Biological Associations]
    D --> F[RDKit Validation and Standardization]
    E --> G[Structured Molecular Data]
    F --> G
    G --> H[Attributed 3D Molecular Graphs]
    H --> I[EGNN-style Variational Encoder]
    I --> J[32D Latent Representation]
    J --> K[SELFIES Molecular Decoder]
    K --> L[Candidate Molecules]
    L --> M[Validity, Novelty, and Chemical Scoring]
    M --> N[Utility and Uncertainty Estimation]
    N --> O[Historical Acquisition and Diversity-aware Selection]
    O --> P[Selected Feedback Batch]
    P --> Q[Experimental Adaptation with EWC]
    Q --> R[Retention and Acceptance Audit]
    R --> S{Acceptance gate}
    S -->|Accepted in a future validated run| T[Promote validated model]
    S -->|Recorded branches rejected| U[Keep K0 authoritative]
```

The diagram describes the intended conditional workflow. In the recorded
three-cycle experiments, the adapted branches were rejected and the
authoritative model remained K0. The diagram does not imply that an
adapted K1 model was successfully promoted.

### Main components

  -----------------------------------------------------------------------
  Component                           Role
  ----------------------------------- -----------------------------------
  XML/XSD inspection                  Understand the source structure and
                                      expected fields

  Streaming extraction                Read drug records, molecular
                                      structures, and available
                                      biological associations

  RDKit preprocessing                 Validate, sanitize, standardize,
                                      and identify duplicate structures

  Graph preparation                   Represent atoms, bonds, features,
                                      and 3D coordinates

  EGNN-style encoder                  Learn geometry-aware molecular
                                      representations

  Variational latent model            Produce and sample a 32-dimensional
                                      latent representation

  SELFIES decoder                     Generate candidate molecular
                                      strings conditioned on latent
                                      vectors

  In-silico evaluation                Calculate QED, an SA-related
                                      utility/proxy, and predicted target
                                      relevance

  Acquisition policy                  Prioritize candidates using the
                                      original multiobjective and
                                      diversity-aware policy

  EWC experiments                     Evaluate parameter movement and
                                      retention-related proxy
                                      measurements

  Evidence audits                     Verify recorded outputs, document
                                      provenance gaps, and create reports
  -----------------------------------------------------------------------

**Terminology note:** The historical acquisition formula includes more
than a simplified UCB expression. Use the actual recorded score
definition when interpreting candidate rankings. The SA-related metric
must retain its implemented definition and should not automatically be
described as a laboratory synthesis result.

------------------------------------------------------------------------

## Recorded results

The figures below summarize the recorded run history supplied with the
project. They are historical observations, not guarantees of performance
on another machine, dataset, or software environment.

### Native XML processing --- Phase 29

  Metric                                    Recorded result
  --------------------------------- -----------------------
  XML source size                             1,544.664 MiB
  Top-level DrugBank drug records                    17,430
  Small-molecule records                             13,166
  Records with extracted SMILES                      12,313
  Valid standardized records                         12,303
  Unique standardized SMILES                         11,942
  Full benchmark repetitions                              3
  Mean processing time                     171.8094 seconds
  Mean throughput                     101.47 records/second
  Mean peak in-kernel RSS                      1,226.82 MiB

**Memory caveat:** Peak RSS was measured inside the running Jupyter
process. It is not an isolated XML-parser memory measurement. The
recorded experiment did not benchmark XML against MySQL, so it does not
establish a relative speedup over MySQL.

### Three-cycle molecular generation --- Phase 25

  Metric                            Cycle 1   Cycle 2   Cycle 3
  ------------------------------- --------- --------- ---------
  Generation attempts                   320       320       320
  Structurally valid outputs            320       319       320
  Within-cycle unique molecules         313       318       314
  Scored candidates                     109       110       109
  Selected feedback candidates           50        50        50

Additional recorded results:

-   **328** scored candidate records across the three cycles.
-   **150** feedback-selection records (50 per cycle).
-   **934** globally unique generated molecules after cross-cycle
    duplicate handling.
-   **6** experimental adaptation branches across three cycles and two
    EWC conditions.

The within-cycle unique counts sum to 945; the pooled global unique
count is 934 because some structures appear in more than one cycle.

### Final audit status --- Phases 30--32

  ------------------------------------------------------------------------------------------------------
  Phase                               Recorded status
  ----------------------------------- ------------------------------------------------------------------
  Phase 30                            `PASS_FINAL_K0_SYSTEM_METRICS_RETROSPECTIVE_NO_BIOLOGICAL_CLAIM`

  Phase 31                            `PASS_PHASE31_ARTIFACT_AUDIT_K0_WITH_DOCUMENTED_GAPS`

  Phase 32                            `PASS_PHASE32_K0_EVIDENCE_AUDIT_WITH_DOCUMENTED_GAPS`
  ------------------------------------------------------------------------------------------------------

Phase 31 indexed 16 artifact groups: 12 complete and 4 partial. Phase 32
investigated the historical provenance gaps but found no earlier
eligible local digest matches for 26 current-only supporting files. A
newly calculated checksum verifies current bytes; it does not prove
historical identity.

**Continual-learning conclusion:** The recorded result was
`D_MIXED_OR_NO_CONSISTENT_RETENTION_ADVANTAGE`. Ground-truth
catastrophic forgetting was **not established**, and K0 remained the
authoritative model.

------------------------------------------------------------------------

## Project phases

The notebook has evolved through 32 numbered phases, including
diagnostics, experimental branches, and read-only audits. Use saved run
IDs, manifests, and output prefixes as the source of truth when a phase
heading is ambiguous.

  -----------------------------------------------------------------------
  Phase group                         Main work
  ----------------------------------- -----------------------------------
  **1--2**                            Environment setup, DrugBank XML/XSD
                                      inspection, source-field mapping

  **3--6**                            Streaming extraction, intermediate
                                      datasets, RDKit standardization,
                                      biological-association preparation

  **7--8**                            Molecular graph construction and
                                      baseline corpus preparation

  **9--13**                           EGNN-style encoder, variational
                                      latent learning, SELFIES decoder,
                                      baseline predictor, Fisher
                                      reference

  **14--16**                          Latent candidate generation,
                                      chemical screening, in-silico
                                      scoring and uncertainty

  **17--22**                          Feedback adaptation experiments,
                                      retention checks, acquisition,
                                      dynamic re-encoding diagnostics

  **23A--23D / 24A**                  Feedback-pair preparation,
                                      stability--plasticity diagnostics,
                                      EWC experiments, read-only audits,
                                      historical knowledge ledger

  **25**                              Three-cycle molecular generation,
                                      scoring, selection, experimental
                                      adaptation

  **26A**                             Retrospective EWC versus no-EWC
                                      ablation

  **27**                              K0 latent-manifold and experimental
                                      embedding-movement analysis

  **28A**                             Forgetting-related proxy analysis
                                      and retrospective ranking
                                      benchmarks

  **29**                              Native DrugBank XML streaming
                                      benchmark

  **30**                              Consolidated system evaluation
                                      metrics

  **31**                              Sixteen scientific artifact groups
                                      and architecture/evidence packaging

  **32**                              Final provenance, integrity,
                                      reproducibility, and
                                      evidence-readiness audit
  -----------------------------------------------------------------------

> **Branch rule:** The recorded experimental adaptation branches were
> rejected. Do not execute an archived K1-dependent continuation simply
> because its cell remains in the notebook. Follow the acceptance gate
> and the exact historical run lineage.

## Repository layout

A representative project layout is:

``` text
MOSAIC/
├── db/
│   ├── drugbank.xml              # Obtain separately under applicable terms
│   └── drugbank.xsd              # Obtain/verify separately as permitted
├── data/
│   ├── extracted/
│   ├── processed/                # Metrics, reports, manifests, phase artifacts
│   ├── graphs/
│   └── feedback/
├── models/
│   └── checkpoints/
├── outputs/
│   ├── metrics/
│   ├── figures/
│   ├── logs/
│   └── candidates/
└── Implv5(7).ipynb                # Notebook snapshot; rename if desired
```

This is a guide to the expected layout, not a guarantee that every
folder is populated in every run. Early phases create the initial
project structure. The final audit phases write uniquely prefixed flat
files into the existing `data/processed` directory and do not create new
subdirectories.

## Requirements

Libraries used across the notebook include:

-   Python and Jupyter / IPython
-   PyTorch
-   NumPy and pandas
-   lxml
-   RDKit
-   SciPy and scikit-learn
-   Matplotlib
-   SELFIES
-   PyArrow and/or fastparquet
-   psutil
-   umap-learn (for optional UMAP analysis)

Not every phase requires every package. The supplied notebook metadata
reports Python **3.14.4**, but the project does not include a fully
pinned dependency lockfile. To reproduce a historical run, use a
compatible environment and record the actual installed versions.
Different package versions or hardware can produce different numerical
results.

## DrugBank data and licensing

The DrugBank XML and XSD are **not included** in this repository. Obtain
them through an authorized source and comply with the applicable
DrugBank license and terms.

Do not commit restricted XML, private model checkpoints, or XML-derived
data unless redistribution is explicitly permitted. Check whether the
license restricts sharing extracted molecular records, target
associations, fingerprints, or processed datasets.

The implemented pipeline processes native XML and does not require an
intermediate MySQL database. The benchmark does not, by itself,
establish that XML is faster than a relational database.

## Installation

### 1. Clone the repository

``` bash
git clone <YOUR_REPOSITORY_URL>
cd <YOUR_REPOSITORY_NAME>
```

Replace the placeholders with your actual repository URL and directory
name.

### 2. Create a compatible environment

The following is a starting example, **not a tested lockfile for the
historical environment**. Choose compatible Python, PyTorch, and RDKit
builds for your platform.

``` bash
conda create -n mosaic python=3.12 -y
conda activate mosaic
conda install -c conda-forge rdkit lxml numpy pandas scipy scikit-learn matplotlib pyarrow fastparquet psutil umap-learn -y
```

Install PyTorch using the instructions appropriate for your operating
system and CPU/CUDA configuration:

<https://pytorch.org/get-started/locally/>

Then install Jupyter and SELFIES support:

``` bash
python -m pip install selfies jupyter ipykernel
```

If you need to reproduce an existing benchmark, do not upgrade packages
without first preserving the original environment and checking
compatibility.

### 3. Verify key imports

``` python
import numpy
import pandas
import torch
from lxml import etree
from rdkit import Chem
import selfies
```

Resolve missing or incompatible dependencies before running a phase.

## Configuration

The initial notebook uses `Path.cwd()` to determine the project root.
Start Jupyter from the directory containing the project data and code.

The original Windows project root was similar to:

``` text
E:\MyPhD\My Patent-1\Implementation
```

The source files are expected at:

``` text
./db/drugbank.xml
./db/drugbank.xsd
```

Some later standalone phase cells define explicit paths or
phase-specific overrides. When moving the project, update the supported
configuration for the cell being executed. Changing the working
directory alone may not update a hard-coded historical path.

## Running the notebook safely

### Fresh reproduction

1.  Obtain the authorized DrugBank XML/XSD files.
2.  Create and verify a compatible Python environment.
3.  Open Jupyter from the project root.
4.  Restart the kernel and run the required initialization cell.
5.  Execute subsequent required cells individually and review each QC
    result.
6.  Preserve run IDs, manifests, source hashes, and output prefixes.
7.  Follow the explicit acceptance gate before running
    adaptation-dependent continuation cells.
8.  Do not run archived K1-dependent branches when the preceding model
    update was rejected.
9.  Do not assume a fresh run will reproduce historical values unless
    the data, code, package versions, configuration, random seeds, and
    execution conditions match.

**Do not use `Run All` on the historical notebook.** It contains
iterative-development cells, diagnostics, legacy headings, and
branch-specific code. Running all cells indiscriminately can rerun
training or execute a continuation that is not appropriate for the
current model state.

### Inspecting the completed historical run

If verified outputs already exist, do not rerun Phases 1--29 just to
recreate the final reports.

-   **Phase 30:** Consolidates saved metrics.
-   **Phase 31:** Packages the sixteen scientific artifact groups and
    architecture evidence.
-   **Phase 32:** Audits historical provenance and integrity and writes
    the documentation-readiness report.

Execute each audit cell individually and preserve the exact historical
run IDs.

## Outputs and provenance

Early phases produce molecular datasets, graph files, model checkpoints,
metrics, and figures. Later phases write uniquely prefixed files such
as:

``` text
phase25_<run_id>_
phase26A_<run_id>_
phase27_<run_id>_
phase28A_<run_id>_
phase29_<run_id>_
phase30_<run_id>_
phase31_<run_id>_
phase32_<run_id>_
```

Depending on the phase, outputs include CSV tables, JSON summaries,
technical reports, figures, manifests, artifact registries, and
readiness handoffs.

The audit phases are designed to preserve historical files, keep the
authoritative K0 state unchanged, and write flat files directly into the
existing processed directory. Read each phase's final QC summary and
manifest; a qualified pass can mean integrity checks succeeded while
provenance gaps remain.

## Reproducibility

For a reproducible audit or experiment:

-   Keep original input data and checkpoints unchanged.
-   Preserve all source and output manifests.
-   Record the actual software environment and hardware.
-   Pin the historical run IDs when auditing a specific execution.
-   Distinguish historical hashes from hashes calculated today.
-   Preserve failed and rejected experimental outcomes.
-   Keep K0 separate from experimental adaptation checkpoints.
-   Do not silently substitute a different source file or run when a
    required input is missing.

A current SHA-256 proves the identity of the bytes at the time of
calculation. Without an independently recorded earlier hash, it cannot
prove the file is identical to its historical version.

## Scientific limitations

MOSAIC is a computational research prototype, not a medical device or a
clinically validated drug-discovery system.

1.  **Predicted biological relevance is not measured bioactivity.** A
    model score alone does not establish experimental binding affinity,
    potency, efficacy, or safety.
2.  **SA-related utility is not proof of synthesis feasibility.**
    Preserve the precise scoring definition used in the implementation.
3.  **Dataset-relative novelty is not patent novelty.** Novelty against
    a DrugBank reference population does not establish worldwide novelty
    or freedom to operate.
4.  **EWC effectiveness remains unresolved.** Recorded experiments
    showed mixed/no consistent retention advantage, and different
    training seeds limit causal attribution.
5.  **Ground-truth catastrophic forgetting was not established.**
    Prediction drift and latent displacement are retention-related
    proxies, not verified original-task performance loss.
6.  **No adapted model was accepted.** K0 remains authoritative; the six
    experimental branches are not accepted K1 states.
7.  **No MySQL comparative benchmark was performed.** Native XML
    processing removes the intermediate database requirement but does
    not establish a speed advantage.
8.  **Full XSD validation is not established by schema inspection
    alone.** Report only the validation actually performed.
9.  **Patentability is not assessed by this code.** Implementation
    evidence does not establish novelty, inventive step, statutory
    eligibility, or freedom to operate.

## Troubleshooting

### `PROCESSED_DIR is not defined`

Some early cells depend on variables initialized in the setup phase. Run
the required initialization cells in order or use the documented
configuration options for the standalone phase.

### XML or XSD file not found

Check the project root and confirm that the source files are present at
the expected paths. Review the specific phase's path overrides if you
moved the project.

### Missing historical source file

Inspect the selected run manifest and source lineage. Do not silently
substitute another run. Use an explicit path override only after
verifying that the replacement is the intended source.

### SHA-256 mismatch

Stop and investigate. Do not replace a pinned historical digest with a
new checksum to force a pass.

### `PARTIAL` artifact status

Inspect the artifact completeness matrix. Partial status may reflect
missing historical checksum evidence, missing data content, or
incomplete source mapping. These are different issues and must remain
separately classified.

### CUDA or PyTorch error

Confirm that the installed PyTorch build matches your platform and
accelerator. Avoid changing packages during an important historical
reproduction without recording the environment change.

### High memory usage

The Phase 29 RSS measurement reflects the running Jupyter process. Other
notebook cells may retain tensors, models, or arrays in memory; do not
interpret that value as isolated parser memory.

## Responsible use

Generated molecules are computational hypotheses for further
investigation. MOSAIC does not replace medicinal chemistry review,
laboratory synthesis, biological assays, toxicology, clinical research,
or regulatory evaluation.

Use the system for research and methods development. Real-world
biological or therapeutic claims require appropriate independent
validation.

## License and citation

No repository license was supplied with the notebook snapshot. Do not
assume that the code or associated data are licensed for unrestricted
reuse. Before publishing, add a suitable `LICENSE` file and verify
redistribution rights for every included component and dataset.

If MOSAIC is used in academic work, cite the associated paper, preprint,
or patent disclosure once its final bibliographic details and public
availability are established. Do not present computational proxy results
as experimentally validated findings.

------------------------------------------------------------------------

```{=html}
<p align="center">
```
`<strong>`{=html}MOSAIC --- Molecular Optimization through Scoring,
Active Learning, and Iterative
Consolidation.`</strong>`{=html}`<br>`{=html}
`<sub>`{=html}Computational molecular design with traceable evidence and
explicit scientific limitations.`</sub>`{=html}
```{=html}
</p>
```
