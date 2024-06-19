## Three-lobed Burning Eye
Scrape log

```
wget --directory-prefix=scrape/htmlbackup --input-file=tlbe-urls.txt -U 'Mozilla/5.0 (X11; Linux x86_64; rv:30.0) Gecko/20100101 Firefox/30.0'

xmllint --html --xpath '//li/a/@href' scrape/htmlbackup/issues.html | sed 's/ href="\([^"]*\)"/https\:\/\/www\.3lobedmag\.com\/\1/g' | grep '_story' | sort | uniq >> tlbe-urls.txt

wget --directory-prefix=scrape/htmlbackup --input-file=tlbe-urls.txt --trust-server-names -U 'Mozilla/5.0 (X11; Linux x86_64; rv:30.0) Gecko/20100101 Firefox/30.0' --wait 5




trafilatura --inputdir ./scrape/htmlbackup --outputdir ./scrape/jsonfiles --output-format json --hash-as-name --no-comments

python ../traffy-meta.py generate-corpus "./scrape/jsonfiles/*" .

```

So, the traffy process gets a lot of the metadata, but it's a bit messy. Going to try to harvest dates, issue numbers, authors from the archives page:

```
xmllint --html --xpath '//h3 | //h4 | //ul[@class="nav_toc"]' scrape/htmlbackup/issues.html > ./scrape/issues-metadata.txt

wget 'https://www.3lobedmag.com/feed/fiction.xml' -U 'Mozilla/5.0 (X11; Linux x86_64; rv:30.0) Gecko/20100101 Firefox/30.0' > scrape/fiction.xml

```


Also, get fresh list of stories from the fiction.xml feed:

```
wget 'https://www.3lobedmag.com/feed/fiction.xml' -U 'Mozilla/5.0 (X11; Linux x86_64; rv:30.0) Gecko/20100101 Firefox/30.0' --directory-prefix=scrape

Convert the text using https://www.convertsimple.com/convert-rss-to-csv/
Cleanup column names and remove some cols.
Output file fiction.csv
```

Join this with the TOC csv, and we have a rich metadata sheet, ready to replace over the traffy less-good metadata:

```
csvtk join --left-join --fields "source;source" ./scrape/issues-metadata.csv ./scrape/fiction.csv > ./scrape/issues-metadata-joined.csv

Open in numbers, remove columns, rename, format dates, etc.
Output issues-metadata-joined.csv

```

Now, having fresh scraped, redo the post-processing and joining:

```
trafilatura --inputdir ./scrape/htmlbackup --outputdir ./scrape/jsonfiles --output-format json --hash-as-name --no-comments

python ../traffy-meta.py generate-corpus "./scrape/jsonfiles/*" .

csvtk join --left-join --fields "source;source" ./metadata.csv ./scrape/issues-metadata-joined.csv > ./metadata2.csv

```

And finally, the joining didn't work easily, so I manually cleaned the original metadta using regular expressions... so, now putting it back inton the JSONL:

```
python ../traffy-meta.py merge-csv-into-jsonl "metadata.csv" "corpus.jsonl"
```

- Done, MS, 2023-05

