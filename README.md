# RFind-explorer

Interactive HTML explorer for **RFind-sc** (Running Fisher for individuals,
single-cell version) scoring results.

**Live**: https://tmurano.github.io/RFind-explorer/

This is a single self-contained HTML file with the demo data embedded
(~30 MB). Open it in any modern browser — no server required.

## What you can do

- **tSNE color map** with per-panel score overlay (`Visualize` tab)
- **2D / 3D scatter** across any pair / triple of axes
- **Scatter matrix** across all axes
- **Correlation heatmap**, global and per-cluster (`Analyze` tab)
- **Pair Analysis (LDA)** for 2-group discrimination on any panel pair
- **Full LDA** across all axes simultaneously
- **Panel upload** (`Compose` tab) — drop a `Symbol, Fold Change[, Rank]`
  CSV and the Explorer scores all cells in the browser via Running Fisher
  in JavaScript, then adds the new panel as a live axis. HUMAN gene
  symbols are auto-converted to mouse via embedded ortholog table.
- **Axis on/off** chips — include/exclude panels from scatter matrix,
  correlation, pair analysis, and LDA on the fly.

## Demo dataset

Mouse microglia scRNA-seq from
[Hammond et al. 2019 *Immunity*](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE121654)
(n = 5,998 cells, 7 groups, 12 Seurat clusters) scored across 10 panels
covering aging, demyelination, development, disease-associated microglia
signatures (KS-DAM, LDAM), curated references (Marschallinger 2020), and
active chromatin marks (H3K27ac, H3K4me2, H3K4me1).

The Explorer also accepts an arbitrary scores CSV via the **Dataset CSV
swap** input — upload your own RFind-sc output to inspect any dataset.

## Related projects

The Explorer is the **visualization face** of the RFind-sc ecosystem.
Other components:

- **RFind-sc R package** — core scoring kernel + panel API:
  https://github.com/tmurano/RFindsc
  ```r
  devtools::install_github("tmurano/RFindsc")
  ```
- **rfindsc-py** — Python port (numerically equivalent to the R package):
  https://github.com/tmurano/rfindsc-py
- **rfind-panels** — community panel registry with web-based PR
  submission flow:
  https://tmurano.github.io/rfind-panels/

## Usage

Just open [`index.html`](./index.html). For a longer tour see
[`USAGE.md`](./USAGE.md).

## License

[MIT](./LICENSE)

## Citation

Murano *et al.*, "RFind-sc: bidirectional case-vs-control panel scoring
for direction-aware, multi-axis, and donor-invariant cell-state analysis
in single-cell RNA-seq" (manuscript in preparation, 2026).
