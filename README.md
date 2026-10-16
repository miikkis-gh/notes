# notes

Built with [MkDocs](https://www.mkdocs.org/) and the [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/) theme.

## Setup

    python3 -m venv .venv
    .venv/bin/pip install -r requirements.txt

`requirements.txt` pins the `mkdocs-material` version, which pulls in MkDocs itself.

## Commands

- `.venv/bin/mkdocs serve`: local preview at <http://127.0.0.1:8000> that reloads on save
- `.venv/bin/mkdocs build --strict`: builds the site into `site/` and fails on warnings such as broken links

## Layout

- `mkdocs.yml`: site settings, theme and Markdown extensions
- `docs/`: the pages, one Markdown file per page; with no `nav` in `mkdocs.yml`, every page appears in the menu automatically (subfolders become sections)
- `docs/CNAME`: custom domain, copied into the built site

## Deploy

`.github/workflows/deploy.yml` runs `mkdocs build --strict` on every push to `main` and publishes `site/` to GitHub Pages.

## Docs

- MkDocs: <https://www.mkdocs.org/user-guide/>
- Material for MkDocs: <https://squidfunk.github.io/mkdocs-material/reference/>
