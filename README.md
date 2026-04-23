# RFind-explorer

Interactive HTML explorer for **RFind-sc** (Running Fisher for individuals, single-cell version) scoring results.

**Live demo**: https://tmurano.github.io/RFind-explorer/

## Overview

RFind-sc applies the RFind (Running Fisher for individuals) framework to single-cell RNA-seq data, producing a cell × axis score matrix that can be explored across multiple gene-set panels. This Explorer visualizes the multi-dimensional score space with:

- **2D / 3D scatter** with cluster/group coloring
- **Scatter matrix** across all axes
- **Correlation heatmap** (global + per-cluster)
- **Pair Analysis (LDA)** for 2-group discrimination
- **Full LDA** across all dimensions
- **tSNE color map** with panel score overlay

## Demo dataset

The embedded demo uses mouse microglia scRNA-seq from [Hammond et al. 2019 *Immunity*](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE121654) (n = 5,998 cells, 7 groups, 12 Seurat clusters) scored across 10 panels covering aging, demyelination, development, disease-associated microglia signatures (KS-DAM, LDAM), curated references (Marschallinger 2020), and active chromatin marks (H3K27ac, H3K4me2, H3K4me1).

## Axis on/off selector

Click the axis chips to include/exclude panels from scatter matrix, correlation, pair analysis, and LDA computations (minimum 2 axes required).

## Usage

Open [`index.html`](./index.html) in any modern browser — no server required. All data is embedded.

## Related

- **RFind-sc R package**: coming soon (see [ROADMAP](https://github.com/tmurano/RFind-explorer))
- **Panel registry** (Tier 0): coming soon

## License

[MIT](./LICENSE)

## Citation

Manuscript in preparation. Please cite as:

> Murano T. et al. *RFind-sc: rank-based gene-set scoring for single-cell RNA-seq.* (manuscript in preparation, 2026)
