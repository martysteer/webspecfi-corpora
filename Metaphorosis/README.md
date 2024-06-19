# Commands to collect and process the Metaphorosis spec-fi mag
### https://magazine.metaphorosis.com/

Using trafilatura to crawl, scrape and process, a la https://trafilatura.readthedocs.io/en/latest/tutorial0.html


1. Construct the urls.txt file from the sitemap.
2. Filter the sitemap for appropriate story pages, and remove any duplicates
3. Swap tmp file
4. Scrape web pages
5. Process web pages into JSON (which usually contains enough metadata)
6. Use ./scripts/convert-to-jsonl.ipynb to compile metadata and content into neat-ish JSON lines corpus and metadata csv file.


```
trafilatura --sitemap "https://magazine.metaphorosis.com/" --list > metaphorosis-urls.txt

grep "/story/" metaphorosis-urls.txt | sort | uniq > tmp.txt
rm metaphorosis-urls.txt
mv tmp.txt metaphorosis-urls.txt

wget --directory-prefix=scrape/htmlbackup/ --input-file=metaphorosis-urls.txt --trust-server-names

trafilatura --inputdir ./scrape/htmlbackup --outputdir ./scrape/jsonfiles --output-format json --nocomments --hash-as-name

```

Ran the notebook manually, then cleaned out the unnecessary files (keeping only a zip of the html files. Using the commands above the corpus can be recreated.)

- MS, 2023