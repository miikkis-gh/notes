# notes

Study handbook, built with [MkDocs Material](https://squidfunk.github.io/mkdocs-material/)
and published at <https://i.miikkis.fi> via GitHub Pages (`.github/workflows/deploy.yml`).

Local preview:

    python3 -m venv .venv && .venv/bin/pip install -r requirements.txt
    .venv/bin/mkdocs serve

New course page: copy `TEMPLATE.md` to `docs/courses/<code>.md`, add it to `nav` in `mkdocs.yml`.
Summarise in your own words and link to course material rather than re-hosting it.
