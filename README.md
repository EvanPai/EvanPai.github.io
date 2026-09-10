# EvanPai.github.io

Source for my personal website and portfolio (Evan Pai), live at
**<https://evanpai.github.io>**.

Built with [Quarto](https://quarto.org) and published with GitHub Pages from the
`docs/` folder on `main`.

## Structure

| Path | What it is |
| --- | --- |
| `_quarto.yml` | Site configuration: title, navbar, theme, `output-dir: docs` |
| `index.qmd` | Home page (Quarto `about` layout) |
| `about.qmd` | Longer introduction |
| `blog.qmd` | Listing page, generated from everything in `posts/` |
| `projects.qmd` | Selected project write-ups |
| `posts/<slug>/index.qmd` | One folder per blog post, images alongside the post |
| `images/` | Site-wide images |
| `styles.css` | Custom styling on top of the `litera` theme |
| `.nojekyll` | Kept at the repo root; Quarto copies it into `docs/` on every render |
| `docs/` | Rendered site — committed on purpose, this is what GitHub Pages serves |

## Building the site

Requires [Quarto](https://quarto.org/docs/get-started/).

```bash
quarto preview            # live preview while editing
quarto render             # rebuild docs/ from scratch
```

## Publishing a change

```bash
quarto render
git status                # check nothing unwanted is about to be staged
git add .
git commit -m "describe the change"
git push origin main
```

GitHub Pages rebuilds within a minute or two of the push. Pushing `.qmd` files
without rendering first will not change the live site.

## Adding a blog post

```bash
mkdir posts/my-new-post
```

Create `posts/my-new-post/index.qmd` with a `title`, `author`, and `date` in the
YAML header, put any images in the same folder, then render and push. The post
appears on the blog listing automatically.
