# Commands to collect and process the Cosmic Roots and Eldritch Shores mag
### https://cosmicrootsandeldritchshores.com

Using trafilatura to crawl, scrape and process, a la https://trafilatura.readthedocs.io/en/latest/tutorial0.html


1. Construct the urls.txt file from the sitemap.
2. Filter the sitemap for appropriate story pages, and remove any duplicates (manually edit)
3. Scrape web pages. Slowly. This site blocks crawlers quite agressively.


4. Process web pages into JSON (which usually contains enough metadata)
5. Use ./scripts/convert-to-jsonl.ipynb to compile metadata and content into neat-ish JSON lines corpus and metadata csv file.


```
wget --directory-prefix=scrape/storypages/ --input-file=scrape/cosmicroots-src.txt --trust-server-names --wait 60

trafilatura --inputdir ./scrape/htmlbackup --outputdir ./scrape/jsonfiles --output-format json --nocomments --hash-as-name

```

Ran the notebook manually, then cleaned out the unnecessary files (keeping only a zip of the html files. Using the commands above the corpus can be recreated.)

- MS, 2023