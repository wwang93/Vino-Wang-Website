# AGENTS.md
# Guidance for agentic coding in this repo.

## Project snapshot
- This is a Quarto website project (see `_quarto.yml`).
- Output is written to `docs/` and `docs/site_libs/`.
- R dependencies are managed with `renv` (see `renv.lock`).
- R version in lockfile: 4.4.1.
- No Cursor or Copilot instruction files are present in this repo.

## Build / lint / test commands

### Environment setup
- Restore packages: `Rscript -e "renv::restore()"`
- Check renv status: `Rscript -e "renv::status()"`

### Build / preview
- Build full site: `quarto render`
- Build a single page: `quarto render index.qmd`
- Live preview with hot-reload: `quarto preview`
- Clean Quarto output: `quarto clean`

### Linting (optional)
- Lint all R sources (if lintr installed): `Rscript -e "lintr::lint_dir('.')"`
- Lint a single file: `Rscript -e "lintr::lint('path/to/file.R')"`

### Formatting (optional)
- Format R code (if styler installed): `Rscript -e "styler::style_dir('.')"`
- Format one file: `Rscript -e "styler::style_file('path/to/file.R')"`

### Tests
- There is no tests directory in this repo right now.
- If tests are added with testthat:
  - Run all tests: `Rscript -e "testthat::test_dir('tests/testthat')"`
  - Run a single test file: `Rscript -e "testthat::test_file('tests/testthat/test-foo.R')"`
  - Run a single test by name: `Rscript -e "testthat::test_file('tests/testthat/test-foo.R', filter = 'name')"`

## Repo structure and conventions
- Top-level pages are `.qmd` files (e.g. `index.qmd`, `Projects.qmd`).
- Section content is organized in folders: `Projects/`, `Portfolio/`, `Teaching/`, `Publications/`, `Blogs/`.
- Shared site config is in `_quarto.yml`.
- Custom styling is in `styles.css`.
- Built site artifacts go to `docs/` and `docs/site_libs/`.

## Code style guidelines

### R code style
- Use 2 spaces for indentation (matches the `.Rproj` config).
- Prefer `snake_case` for variables and functions.
- Use `UpperCamelCase` only for R6/RC classes or S4 classes.
- Prefer explicit namespaces (`pkg::fn`) inside small chunks.
- If many functions are used, a single `library(pkg)` at top is OK.
- Avoid `attach()` and implicit search path side effects.
- Set seeds (`set.seed(...)`) for any random output in rendered pages.
- Keep chunk outputs deterministic to avoid noisy diffs in `docs/`.

### Quarto / QMD style
- Use YAML front matter at top of `.qmd` pages.
- Prefer Quarto chunk options with the `#|` syntax.
- Keep chunk options consistent (`echo`, `message`, `warning`, `fig.width`).
- Avoid hidden side effects in chunks; make data loading explicit.
- When a chunk must fail the build, use `stop(...)` with a clear message.
- When a chunk can degrade gracefully, use `warning(...)` or `message(...)`.

### Imports and dependencies
- Use `renv` for package changes:
  - Add dependency: `Rscript -e "renv::install('pkg')"`
  - Update lockfile: `Rscript -e "renv::snapshot()"`
- Do not edit `renv.lock` by hand.
- Keep package usage minimal in QMD chunks to speed builds.

### Naming conventions
- Files: `Title_Case.qmd` for top-level pages already follow this style.
- New QMD files should follow existing naming patterns.
- Avoid spaces in filenames; use underscores if needed.
- Section folders should be descriptive and match navbar labels.

### Error handling
- For fatal issues, prefer `stop("clear message")`.
- For recoverable issues, use `warning(...)` and continue.
- Use `tryCatch()` when optional content may fail but the page should render.
- Avoid suppressing errors unless you also log a message.

### Data and assets
- Store images in `images/` or local section folders.
- For large binary assets, prefer external hosting and link them.
- Keep `docs/` in sync by re-rendering, not manual edits.

### CSS and styling
- Keep changes centralized in `styles.css`.
- Existing fonts are loaded via Google Fonts; reuse unless a full redesign.
- Avoid overriding Quarto theme styles unless needed.
- Prefer small, focused selectors over broad global resets.

## Content editing tips
- For new pages, add to `_quarto.yml` navbar if it should be visible.
- Use relative links to keep site portable.
- When changing structure, update both source `.qmd` and built `docs/`.

## Checks before committing
- Run `quarto render` to update `docs/`.
- Ensure no unintended changes in `docs/site_libs/`.
- Confirm `renv.lock` matches package changes.

## Notes for agents
- No Cursor rules in `.cursor/rules/` or `.cursorrules`.
- No GitHub Copilot instructions in `.github/copilot-instructions.md`.
- Follow existing patterns; do not introduce new tooling without need.
