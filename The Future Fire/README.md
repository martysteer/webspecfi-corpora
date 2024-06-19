# Commands to collect and process The Future Fire spec-fic mag
### http://futurefire.net/index.html

Using trafilatura to crawl, scrape and process, a la https://trafilatura.readthedocs.io/en/latest/tutorial0.html


1. Construct the source pages urls .txt file. These listing pages contain the backissues.
2. Use wget to scrape those HTML listing pages, use xmllint to extract the links pages, and remove PDF and MOBI files.
4. Use wget to scrape the issue pages.
5. I then used ChatGPT to help me figure out an xmllint command, which didn't work, so I got it to help me write a python command line script to spit out the URL's I needed. It can be run using the command below.
5. Use wget to scrape the story pages.
6. Process web pages into JSON (which usually contains enough metadata)

7. Use ./scripts/convert-to-jsonl.ipynb to compile metadata and content into neat-ish JSON lines corpus and metadata csv file.


```
wget --directory-prefix=scrape/srcpages/ --input-file=scrape/tff-src.txt --trust-server-names

xmllint --html --xpath '//a/@href' scrape/srcpages/* | cut -d'"' -f 2 | grep '.html' > scrape/tff-issues.txt

wget --directory-prefix=scrape/issuepages/ --input-file=scrape/tff-issues.txt --trust-server-names

# chatGPT helped write this script.
python scrape/extract-tff-links.py "scrape/issuepages/*" | sort | uniq > tff-urls.txt

wget --directory-prefix=scrape/htmlbackup/ --input-file=tff-urls.txt --trust-server-names

trafilatura --inputdir ./scrape/htmlbackup --outputdir ./scrape/jsonfiles --output-format json --nocomments --hash-as-name
```

Ran the notebook manually.

- MS, 2023