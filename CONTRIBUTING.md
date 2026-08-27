# Contributing

Thank you for your interest in improving `pubclassify`. Contributions that make
publication retrieval, normalization, enrichment, and classification more
reliable and easier to use are welcome.

## Ways to Contribute

- Report bugs, unexpected search results, documentation gaps, or feature ideas
  by opening an issue.
- Improve documentation, examples, and tests through a pull request.
- Submit focused fixes or enhancements to package code and accompanying tests.

## Opening an Issue

Please include enough detail for a maintainer to understand and reproduce the
problem:

- A short description of the issue or proposed improvement.
- The function and arguments involved, along with expected and actual behavior.
- A minimal reproducible example, when possible.
- Relevant error messages, R version, operating system, and package version.

Do not include API keys, tokens, email addresses, private publication data, or
other sensitive information in issues, examples, or logs.

## Development Setup

Install the package dependencies, then load the development version from the
repository root:

```r
install.packages(c("devtools", "testthat"))
devtools::load_all()
```

Some package features call external bibliometric APIs or LLM providers. Unit
tests must not make live API calls or depend on credentials being configured.

## Making a Pull Request

1. Fork the repository and create a short-lived branch for your change.
2. Make a focused change with tests where practical.
3. Run the checks below locally.
4. Open a pull request describing the motivation, behavior change, and test
   results.

Keep pull requests narrowly scoped. Update documentation when public behavior
changes, and describe any follow-up work or manual testing that reviewers need
to perform.

The `main` branch is protected by a GitHub ruleset. Changes must be made through
a pull request, and the current commit must pass the **R-CMD-check** and
**Secrets scan** checks before merging. Code ownership is recorded in
`.github/CODEOWNERS` so review requests reach the project owner(s).

## Development Guidelines

- Preserve the standard result schema returned by `pc_search_*()` and
  `pc_combine()`; do not remove, rename, or reorder its core columns.
- Use `.pc_empty_result()` for zero-result paths.
- Do not add `dplyr`, `tidyr`, or `purrr` to package internals.
- Never commit credentials, tokens, local environment files, or sensitive data.
- Wrap roxygen examples that call external services in `\dontrun{}`.
- Add or update tests using local fixtures or mocked HTTP responses rather than
  live services.

## Testing and Documentation

Before opening a pull request, run:

```r
devtools::document()
devtools::test()
devtools::check()
```

For a smaller iteration loop, run the affected test file with
`testthat::test_file()`. If your change updates roxygen documentation, include
the generated `NAMESPACE` and `man/` changes in the pull request.

All pushes and pull requests are also scanned with
[Betterleaks](https://github.com/betterleaks/betterleaks) for accidentally
committed credentials. The scan uses `.github/betterleaks-baseline.json`, which
records only any reviewed pre-existing findings. Do not add new secrets to that
baseline; revoke or rotate exposed credentials immediately.

## Code of Conduct

All contributors are expected to follow the project's
[Code of Conduct](CODE_OF_CONDUCT.md).
