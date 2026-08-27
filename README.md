# pubclassify

`pubclassify` is an R package for finding peer-reviewed publications,
standardizing their metadata, and organizing them into a user-defined taxonomy.

Visit the [pkgdown site](https://dwr-open-science.github.io/pubclassify/) for
reference documentation and articles.

At a high level, the package:

- retrieves publication metadata from OpenAlex, Scopus, and Crossref
- standardizes and combines results across sources
- enriches records with abstracts, acknowledgments, funders, and affiliations
- classifies publications into custom scholarly categories using LLMs

## Installation

Install the development version from GitHub:

```r
# install.packages("remotes")
remotes::install_github("DWR-Open-Science/pubclassify")
```

For development from a local clone, install the package dependencies and load
the package with `devtools::load_all()`.

## Quick start

OpenAlex searches do not require an API key. Supplying an email address places
requests in OpenAlex's polite pool, which provides more reliable access.

```r
library(pubclassify)

pubs <- pc_search_openalex(
  "estuarine ecology",
  field = "abstract",
  max_results = 25,
  email = Sys.getenv("PUBCLASSIFY_EMAIL")
)

taxonomy <- pc_taxonomy(data.frame(
  field = c("Ecology", "Hydrology"),
  definition = c(
    "Study of organisms and their interactions with the environment.",
    "Study of water movement, distribution, and quality."
  )
))
```

To classify results, configure an LLM provider and then call
`pc_classify(pubs, taxonomy)`. Classification sends publication titles and
available abstracts, along with taxonomy definitions, to the selected provider.
Review your organization's data-sharing requirements and the provider's terms
before classifying non-public or sensitive data.

```r
# Set PUBCLASSIFY_LLM_KEY outside version control before running this.
pc_configure(llm_provider = "anthropic")
classified <- pc_classify(pubs, taxonomy)
```

## Data sources and credentials

- **OpenAlex**: no API key is required. Set `PUBCLASSIFY_EMAIL` to use the
  polite pool.
- **Scopus**: set `SCOPUS_API_KEY`; `SCOPUS_INSTTOKEN` may also be required
  for institutional COMPLETE-view access.
- **Crossref**: the search interface is present but not yet implemented, so
  `pc_search_crossref()` currently returns no results; this is a deferred
  functionality.
- **LLM classification**: requires a provider key, such as
  `PUBCLASSIFY_LLM_KEY` or the provider's standard environment variable.

Use `pc_configure()` to configure the current R session. `pc_save_config()`
can persist values in a user or project `.Renviron` file; never commit that
file or paste credentials into issues, code, or notebooks.

## Typical workflow

1. Securely configure API and model credentials with `pc_configure()`.
2. Search one or more bibliometric sources with `pc_search()`.
3. Combine and deduplicate records with `pc_combine()` and `pc_deduplicate()`.
4. Define a taxonomy with `pc_taxonomy()`.
5. Classify publications with `pc_classify()`.

## Main functions

- `pc_search()`: unified search interface across supported sources
- `pc_search_openalex()`, `pc_search_scopus()`, `pc_search_crossref()`:
  source-specific search helpers
- `pc_fetch_abstracts()` and `pc_fetch_acknowledgments()`: record enrichment
- `pc_find_funder()` and `pc_flag_awards()`: funding-related utilities
- `pc_taxonomy()` and `pc_taxonomy_example()`: taxonomy creation and examples
- `pc_classify()`: LLM-based classification into taxonomy labels

## Support and contributing

Please [open a GitHub issue](https://github.com/DWR-Open-Science/pubclassify/issues)
to report a bug, request a feature, or ask a question. For bug reports, include
the function call, expected and actual behavior, relevant error output, and a
small reproducible example when possible. The
[reprex package](https://reprex.tidyverse.org/) can help prepare a concise,
shareable example. Do not include credentials, tokens, or private publication
data.

See [CONTRIBUTING.md](CONTRIBUTING.md) for development setup, testing, and pull
request guidance. All participants are expected to follow the
[Code of Conduct](CODE_OF_CONDUCT.md).

## Citation

See `citation("pubclassify")` for citation information.

## Status

The package is under active development. OpenAlex and Scopus searches are
covered by the test suite; Crossref search is not yet implemented.
