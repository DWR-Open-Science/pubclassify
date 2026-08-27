# pubclassify 1.0.0

This is the first public release of pubclassify.

### Features

* Search peer-reviewed publication metadata in OpenAlex and Scopus by
  abstract, affiliation, or funder.
* Combine and deduplicate results using a consistent publication metadata
  schema.
* Enrich records with OpenAlex metadata and, where available, retrieve
  Elsevier acknowledgment text.
* Find OpenAlex funders, flag award identifiers, and create validated
  publication taxonomies.
* Classify publications into taxonomy fields with supported large language
  model providers.
* Configure credentials in the current R session or save masked configuration
  values to an `.Renviron` file.

### Deferred capabilities

* Crossref search is not included in the 1.0.0 feature set. The
  `pc_search_crossref()` interface is reserved for a future major release and
  currently returns an empty result.
* Embedding-assisted candidate retrieval in `pc_classify()` is reserved for a
  future major release. Use the default full-taxonomy classification path in
  1.0.0.
