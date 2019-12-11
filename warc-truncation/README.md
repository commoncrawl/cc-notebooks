# Truncated Records in WARC Files

Content payload in Common Crawl archives is truncated if the content exceeds a limit of
- [500 kiB in 2008 – 2012 ARC files](https://groups.google.com/d/topic/common-crawl/hQTRnWahcHA/discussion)
- 1 MiB in WARC files (since 2013)

The truncation is required to keep the crawl archives at a limited size and ensure that a broad sample of web pages is covered. It also avoids that the archives are filled by accidentally captured video or audio streams. The crawler needs to buffer the content temporarily and a limit ensures that this is possible with a limited amount of RAM for many parallel connections.

The notebooks in this folder analyze various aspects of payload truncation:
- [cc-main-2018-43-single-warc-file.ipynb] - truncation counts for a single WARC file of CC-MAIN-2018-43 and a broken marking of truncated records
- [cc-main-2019-35-100-warc-files.ipynb] - marking of truncated records has been fixed for CC-MAIN-2019-35 and 100 randomly selected WARC files are analyzed to verify the marking and get more detailed metrics

