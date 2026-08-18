# Using Common Crawl via Hugging Face

This folder contains examples of how to use Common Crawl data via the Hugging Face ecosystem (buckets, jobs, ...).

Currently, the hosting of Common Crawl data on Hugging Face is experimental, i.e., the data is limited to a small number of crawls. The primary data distribution channel is still AWS S3.

Notebooks:

- [cc-index-hf.ipynb](cc-index-hf.ipynb): Common Crawl's URL Index via Hugging Face
- [s3-hf.ipynb](s3-hf.ipynb): Using Common Crawl data via Hugging Face's S3-compatible gateway
- [warcio-hf.ipynb](warcio-hf.ipynb): Reading WARC files from Hugging Face
- [cdxt-hf.ipynb](cdxt-hf.ipynb): Querying CDX files from Hugging Face (CURRENTLY NOT POSSIBLE DUE TO MISSING CDX FILES)

See also:

- https://huggingface.co/docs/hub/storage-buckets
- https://huggingface.co/docs/hub/storage-buckets-s3
