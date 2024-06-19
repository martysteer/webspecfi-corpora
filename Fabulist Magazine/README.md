# Commands to collect and process the Fabulist spec-fi mag
### https://fabulistmagazine.com/

Using trafilatura to crawl, scrape and process, a la https://trafilatura.readthedocs.io/en/latest/tutorial0.html


1. Construct the source pages urls .txt file. These listing pages contain the stories.
2. Use wget to scrape those HTML listing pages
3. Use xmllint to extract the links to the listing pages
4. Use wget to scrapte the story pages
5. Process web pages into JSON (which usually contains enough metadata)


6. Use ./scripts/convert-to-jsonl.ipynb to compile metadata and content into neat-ish JSON lines corpus and metadata csv file.


```
wget --directory-prefix=scrape/storypages/ --input-file=scrape/fabulist-src.txt --trust-server-names

xmllint --html --xpath '//h3/a/@href' scrape/storypages/* | cut -d'"' -f 2 > fabulist-urls.txt

wget --directory-prefix=scrape/htmlbackup/ --input-file=fabulist-urls.txt --trust-server-names

trafilatura --inputdir ./scrape/htmlbackup --outputdir ./scrape/jsonfiles --output-format json --nocomments --hash-as-name
```

Ran the notebook manually.

- MS, 2023