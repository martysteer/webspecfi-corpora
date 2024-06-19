## Vestal Review - web scrape commands

```
wget --directory-prefix=scrape/issues/ --input-file=vestal-urls.txt --trust-server-names --wait 1
xmllint --html --xpath '//a[contains(@class, "gutentor-button")]/@href' scrape/issues/index.html > tmp.txt
```

Then find/replace partial URL's into full URL's, so we can scrape issues TOC pages

```
wget --directory-prefix=scrape/tocpages/ --input-file=vestal-urls.txt --trust-server-names --wait 1
xmllint --html --xpath '//a[contains(@class, "gutentor-button")]/@href' scrape/issues/index.html > tmp.txt                
```

Then open and clean all the story URL's.

```
wget --directory-prefix=scrape/htmlbackup --input-file=vestal-urls.txt --trust-server-names --wait 1
trafilatura --inputdir ./scrape/htmlbackup --outputdir ./scrape/jsonfiles --output-format json --hash-as-name --no-comments
python ../traffy-meta.py generate-corpus "./scrape/jsonfiles/*" .
```

