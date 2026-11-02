# Personal website

Source for [loicdumortier.com](https://loicdumortier.com), built with
[Jekyll](https://jekyllrb.com/) and the
[Minimal Mistakes](https://mmistakes.github.io/minimal-mistakes/) theme.
The site is published with GitHub Pages.

## Local development

Requirements: Ruby 3.x, Bundler, and the gems in `Gemfile`.

```bash
bundle install
bundle exec jekyll serve --livereload
```

Then open <http://localhost:4000>.

If you do not have Ruby installed locally, you can build with Docker:

```bash
docker run --rm -it -v "$PWD":/srv/jekyll -w /srv/jekyll \
  jekyll/jekyll:latest sh -lc \
  "bundle install && bundle exec jekyll serve --host 0.0.0.0"
```

## Content overview

| File | Purpose |
| --- | --- |
| `index.md` | Home page: profile, focus areas, selected publications |
| `cv.md` | Web version of the CV |
| `publications.md` | Full publication list |
| `_data/publications.yml` | Publication metadata and BibTeX |
| `_data/navigation.yml` | Top navigation |
| `_config.yml` | Site settings, author profile, SEO defaults |
| `assets/cv/Loic_Dumortier_CV.pdf` | Downloadable CV |
| `assets/thesis/Loic_Dumortier_PhD_Thesis.pdf` | PhD thesis PDF |

## Adding a publication

Add an entry to `_data/publications.yml`:

```yaml
- title: "Paper title"
  authors:
    - "Loïc Dumortier"
    - "Co-author Name"
  venue: "Journal Name"
  year: 2026
  type: "Journal article"
  selected: true        # also show on the home page
  doi: "10.xxxx/xxxxx"
  arxiv: "2601.00000"   # optional
  pdf: "/assets/papers/paper.pdf"  # optional
  url: "https://..."    # optional record link
  note: ""              # optional
  bibtex: |
    @article{key,
      author = {Loïc Dumortier and Co-author Name},
      title  = {Paper title},
      year   = {2026}
    }
```

The list is generated automatically, grouped by year, with your name
highlighted.

## Replacing the CV or thesis PDF

Replace the file and keep the same filename:

```text
assets/cv/Loic_Dumortier_CV.pdf
assets/thesis/Loic_Dumortier_PhD_Thesis.pdf
```

If you rename a file, update the corresponding links in `cv.md`,
`_data/publications.yml`, and `index.md`.

## Deployment

GitHub Pages builds the site automatically on every push to `main`. The custom
domain is configured through the `CNAME` file (`loicdumortier.com`).

## Link checking

`.github/workflows/build.yml` builds the site and runs
[html-proofer](https://github.com/gjtorikian/html-proofer) on pull requests and
pushes to `main`. External links are not checked by default to avoid flaky
failures from publisher and DOI sites.
