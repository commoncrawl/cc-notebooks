# Jupyter Notebooks to Analyze Common Crawl Data


* analyzing data using the [columnar index](https://commoncrawl.org/2018/03/index-to-warc-files-and-urls-in-columnar-format/)
  - blocking of internet connections from and to the Islamic Republic of Iran during the November 2019 crawl: [net-blocking-iran-cc-main-2019-47.ipynb](./cc-index-table/net-blocking-iran-cc-main-2019-47.ipynb)
  - total number of captures 2013 – 2019, domain coverage and approximation of unique URLs for the `.edu` top-level domain: [cc-main-2013-2019-metrics.ipynb](./cc-index-table/cc-main-2013-2019-metrics.ipynb)
  - correlations between character sets and lanuages: [correlation-language-charset.ipynb](./cc-index-table/correlation-language-charset.ipynb)
* [truncated record payloads in WARC Files](./warc-truncation/):
  - verify that all truncated payloads are annotated by the [WARC-Truncated header](https://iipc.github.io/warc-specifications/specifications/warc-format/warc-1.1/#warc-truncated)
  - which MIME types are mostly affected by truncation? Aggregations using the columnar index.

