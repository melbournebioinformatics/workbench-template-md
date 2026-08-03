# Melbourne Bioinformatics Workbench Lesson Template (Markdown)

The University of Melbourne fork of the [Carpentries Workbench][workbench] Markdown lesson
template. Use it to start a new MB training workshop.

It differs from the upstream Carpentries template in two ways. It pins our theme fork,
[`uom-varnish`][uom-varnish], in `config.yaml`:

```yaml
varnish: 'melbournebioinformatics/uom-varnish@main'
url: 'melbournebioinformatics.github.io/uom-varnish'
```

and it adds UoM institute codes to `carpentry:`, which selects the branding on the built site:
`uom` (University of Melbourne), `mb` (Melbourne Bioinformatics), `mig` (Melbourne Integrative
Genomics), `wehi` (Walter and Eliza Hall Institute), `abacbs` (Australian Bioinformatics and
Computational Biology Society). Keep both settings when you adapt this template.

Full contributor documentation lives in the [MB tutorials wiki][wiki].

## Which template do I want?

| | Use when |
|---|---|
| **`workbench-template-md`** (this one) | Episodes are prose, screenshots and fenced code blocks that are *shown*, not run. Command-line tutorials, GUI walkthroughs, conceptual material. |
| [**`workbench-template-rmd`**][rmd] | Episodes must *execute* code at build time, so output and plots are generated from the source. R, or Python via `reticulate`. |

If in doubt, start here. Markdown lessons build faster, have no package dependencies and are far
less trouble to maintain.

## Note about lesson life cycle stage

Although `config.yaml` states the life cycle stage as pre-alpha, **the template is stable and
ready to use**. The life cycle stage is preset to `"pre-alpha"` because that is the right setting
for a brand new lesson.

## Create a new lesson from this template

Click **Use this template** at the top right of
[the repository page](https://github.com/melbournebioinformatics/workbench-template-md), then
**Create a new repository**.

Name the repository in lowercase with dashes separating words, and make the name say what the
lesson is about plus either its focus or its technical level. `intro-to-git` is topic plus level;
`rna-seq-counts-to-genes` is topic plus scope. The name becomes the published URL, so it is worth
a minute of thought. A new lesson can start private and be made public later.

## Configure the new lesson

1. **Enable GitHub Pages.** _Settings_ → _Pages_, build from the `gh-pages` branch. That branch
   appears once the first build workflow has run, so check _Actions_ if it is not there yet.
2. **Fill in `config.yaml`.** Every field marked `# FIXME`: `title`, `carpentry_description`,
   `created`, `keywords`, `contact`, `source`, and `life_cycle` (`pre-alpha` → `beta` once the
   lesson is usable). Set `carpentry:` to your institute code, and list your episode files in
   teaching order under `episodes:`. **Leave the `varnish:` and `url:` lines alone.**
3. **Rename `FIXME.Rproj`** to match the repository name.
4. **Annotate the repository.** On the landing page, click the cog next to _About_, tick "Use your
   GitHub Pages website", and add topic tags: `lesson`, the life cycle stage, and the language.
5. **Adjust `CITATION.cff`, `CODE_OF_CONDUCT.md`, `CONTRIBUTING.md` and `LICENSE.md`** for your
   project. `CITATION.cff` is worth revisiting as the author list grows;
   [`cffinit`][cffinit] will generate one.
6. **Replace this README** with a description of your lesson, and delete these instructions.

## Build and preview locally

Requires R and pandoc. One-time setup on your machine:

```r
install.packages("pak")

options(repos = c(
  carpentries = "https://carpentries.r-universe.dev",
  CRAN        = "https://cloud.r-project.org"
))

pak::pak(c("sandpaper", "pegboard", "tinkr"))
pak::pak("melbournebioinformatics/uom-varnish")   # installs under the package name `varnish`

sandpaper::use_package_cache(prompt = FALSE)
```

We use `pak` rather than `devtools`, which has been split up and superseded. Then, from inside
the lesson repository:

```r
sandpaper::serve()          # live-reload preview on http://127.0.0.1:4321
sandpaper::build_lesson()   # one-off build into site/
sandpaper::check_lesson()   # structure and link validation
```

> ⚠️ **Never run `renv::init()` or `renv::activate()` in a lesson repository.** sandpaper manages
> its own renv profile and is not meant to be activated. Both commands write a root `.Rprofile`
> that hijacks every R session in the repo into an empty project library, at which point
> `sandpaper` appears to vanish. This is a Markdown lesson, so it has no lesson dependencies at
> all and `renv/` is gitignored on purpose. See [Renv and dependencies][renv-wiki].

## Writing episodes

Episodes live in `episodes/*.md`, one per major section, 15 to 45 minutes each and never longer
than an hour. Every episode needs `title`, `teaching` and `exercises` in its frontmatter, plus
`questions` and `objectives` blocks at the top and a `keypoints` block at the end.
`episodes/introduction.md` demonstrates every available block.

Callouts, challenges and solutions use Carpentries fenced divs. **Fence depth must match exactly
between the opening and closing markers**, or pegboard fails and the block renders as plain text.

- [Sandpaper documentation](https://carpentries.github.io/sandpaper-docs/episodes.html)
- [Component style guide](https://carpentries.github.io/sandpaper-docs/component-guide.html)
- [How the Workbench works](https://carpentries.github.io/workbench/workflow-guide.html)

[workbench]: https://carpentries.github.io/sandpaper-docs/
[uom-varnish]: https://github.com/melbournebioinformatics/uom-varnish
[rmd]: https://github.com/melbournebioinformatics/workbench-template-rmd
[wiki]: https://github.com/melbournebioinformatics/melbournebioinformatics.github.io/wiki
[renv-wiki]: https://github.com/melbournebioinformatics/melbournebioinformatics.github.io/wiki/Renv-and-dependencies
[cffinit]: https://citation-file-format.github.io/cff-initializer-javascript/
