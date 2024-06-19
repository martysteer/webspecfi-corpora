# Commands to collect and process the Clarkesworld sci-fi mag
### https://clarkesworldmagazine.com

Using trafilatura to crawl, scrape and process, a la https://trafilatura.readthedocs.io/en/latest/tutorial0.html


1. Construct the source pages urls .txt file. These listing pages contain the stories.
2. Use wget to scrape those HTML listing pages
3. Use xmllint to extract the links to the listing pages
5. Process web pages into JSON (which usually contains enough metadata)


6. Use ./scripts/convert-to-jsonl.ipynb to compile metadata and content into neat-ish JSON lines corpus and metadata csv file.


```
wget --directory-prefix=scrape/storypages/ --input-file=scrape/clarkesworld-src.txt --trust-server-names

<h2 class="entry-title"><a href="https://clarkesworldmagazine.com/chan_04_23/">Re/Union</a></h2>  

xmllint --html --xpath '//h2/a/@href' scrape/storypages/* | cut -d'"' -f 2 > clarkesworld-urls.txt

wget --directory-prefix=scrape/htmlbackup/ --input-file=clarkesworld-urls.txt --trust-server-names

trafilatura --inputdir ./scrape/htmlbackup --outputdir ./scrape/jsonfiles --output-format json --nocomments --hash-as-name
```
