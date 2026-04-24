# RFind-explorer (archived)

> **This repository has moved.** Its multi-panel tSNE, 2D scatter, and
> correlation views are now part of the
> [**RFind-sc Panel Registry**](https://tmurano.github.io/rfind-panels/).
>
> - **Registry + analysis workbench**: https://tmurano.github.io/rfind-panels/
> - **Registry repo**: https://github.com/tmurano/rfind-panels
> - **R package**: https://github.com/tmurano/RFindsc
> - **Python package**: https://github.com/tmurano/rfindsc-py

The `index.html` in this repo now serves a redirect page; visitors to
`tmurano.github.io/RFind-explorer/` are auto-forwarded to the registry.
The original interactive Explorer HTML is preserved in the git history
at commit [`eef7577`](https://github.com/tmurano/RFind-explorer/commit/eef7577)
for reference.

## Why the move

`RFind-explorer` and `rfind-panels` each shipped their own ~30 MB
Hammond embed, and the Explorer's Visualize / Compare views had no
coupling to the panel catalog or the submission flow. Consolidating
into one page removed the duplication and gave contributors a single
URL for browse → try → analyze → submit.

## License

[MIT](./LICENSE).
