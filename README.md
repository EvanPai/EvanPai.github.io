# EvanPai.github.io

Source for my personal website and portfolio (Evan Pai), live at
**<https://evanpai.github.io>**. It is a [Quarto](https://quarto.org) website with
computational blog posts in R and Python. It is rendered to `docs/` and served
by GitHub Pages from the `main` branch.

The Python and R environments are pinned with lockfiles (`uv.lock` and
`renv.lock`). The steps below rebuild the whole site from a fresh clone.

## 1. Install these first

| Tool | Version used | Get it from |
| --- | --- | --- |
| Git | any recent | <https://git-scm.com> |
| Quarto | 1.10.18 | <https://quarto.org/docs/get-started/> |
| uv | 0.12.7 | <https://docs.astral.sh/uv/getting-started/installation/> |
| R | 4.6.1 | <https://cloud.r-project.org> |

You do **not** need to install Python or any R packages yourself:

- `uv` downloads Python 3.14 (pinned in `.python-version`) if you do not have it.
- `renv` (1.2.4) installs itself the first time R starts in this folder, then
  installs the exact package versions in `renv.lock`.

Tested on macOS (Apple silicon). The commands are the same on Linux. On
Windows, run them in PowerShell or Git Bash.

## 2. Build the site, from clone to `docs/`

Run every command from the **top level of the repository** (the folder that
contains `_quarto.yml`). R only finds `.Rprofile`, and with it `renv`, when it
starts there.

**Shell:** clone the repository and enter it.

```bash
git clone https://github.com/EvanPai/EvanPai.github.io.git
cd EvanPai.github.io
```

**Shell:** create the Python environment in `.venv/` from `uv.lock`.

```bash
uv sync
```

**Shell:** restore the R packages from `renv.lock`. The first R start
bootstraps `renv` automatically.

```bash
Rscript -e 'renv::restore(prompt = FALSE)'
```

If you prefer an interactive R session, start R in the same top-level folder and
run this in the **R console**:

```r
renv::restore()
```

**Shell:** render the site. `uv run` makes Quarto use the project's `.venv` for
Python chunks. `.Rprofile` turns on `renv` for R chunks and points `reticulate`
at the same `.venv`.

```bash
uv run quarto render
```

## 3. Where the built site is and how to open it

The rendered site is written to **`docs/`**, with the home page at
`docs/index.html`. The easiest way to view it locally, with working search and
blog listing, is to serve the folder:

```bash
uv run python -m http.server 8000 --directory docs
```

Then open <http://localhost:8000> in a browser. Stop the server with `Ctrl+C`.
Opening `docs/index.html` directly also works for reading pages.

While editing, `uv run quarto preview` gives a live-reloading preview instead.

## 4. Data

All data comes from **[Epoch AI](https://epoch.ai/data)** under
[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/), which allows
redistribution with attribution. Each post that uses it credits it with a link.
Both files are snapshots downloaded on 23 September 2026 and **committed to the
repository**, so the build does **not** need the network to fetch data:

| File | Dataset | Size | Used by |
| --- | --- | --- | --- |
| `posts/gpu-hardware-trends/ml_hardware.csv` | [Data on Machine Learning Hardware](https://epoch.ai/data/machine-learning-hardware) | ~100 KB | R post, bonus post |
| `posts/ai-compute-scaling/notable_ai_models.csv` | [Data on AI Models](https://epoch.ai/data/ai-models) (Notable AI Models) | ~2.2 MB | Python post, bonus post |

The network is only needed once, for `uv sync` and `renv::restore()` to
download packages.

## Repository layout

| Path | What it is |
| --- | --- |
| `_quarto.yml` | Site configuration: title, navbar, theme, `output-dir: docs` |
| `index.qmd`, `about.qmd`, `projects.qmd` | Home, about and projects pages |
| `blog.qmd` | Blog listing, generated from everything in `posts/`, newest first |
| `posts/<slug>/index.qmd` | One folder per post, with its images and data alongside |
| `posts/gpu-hardware-trends/` | R post (knitr engine) |
| `posts/ai-compute-scaling/` | Python post (Jupyter engine) |
| `posts/r-and-python/` | R and Python in one post via `reticulate` |
| `pyproject.toml`, `uv.lock`, `.python-version` | Python environment (uv) |
| `renv.lock`, `.Rprofile`, `renv/activate.R` | R environment (renv) |
| `images/`, `styles.css`, `_photo-rows.html` | Site-wide images and styling |
| `.nojekyll` | Copied into `docs/` on render so GitHub Pages serves Quarto's files as-is |
| `docs/` | Rendered site, committed on purpose. This is what GitHub Pages serves |

## Updating the environments

```bash
uv add <package>                       # Python: updates pyproject.toml and uv.lock
Rscript -e 'renv::install("<pkg>")'    # R: install, use it in a post, then...
Rscript -e 'renv::snapshot()'          # ...record it in renv.lock
```

`renv` only records packages that a `.qmd` file actually loads with `library()`.

## Publishing a change

```bash
uv run quarto render
git status                # check nothing unwanted is about to be staged
git add .
git commit -m "describe the change"
git push origin main
```

GitHub Pages rebuilds within a minute or two of the push. Pushing `.qmd` files
without rendering first will not change the live site.
