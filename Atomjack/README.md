
```
wget --directory-prefix=scrape/htmlbackup/ --input-file=atomjack-urls.txt --trust-server-names

xmllint --html --xpath "//td/a/@href" scrape/htmlbackup/* | cut -d '"' -f2 | sed 's/.*/https:\/\/web.archive.org\/web\/20090321021722\/http:\/\/atomjackmagazine.com\/&/' | sort | uniq >> atomjack-urls.txt

# This appends deduplicated URL's extracted from the two archives pages.

wget --directory-prefix=scrape/htmlbackup/ --input-file=atomjack-urls.txt --trust-server-names

trafilatura --inputdir ./scrape/htmlbackup --outputdir ./scrape/jsonfiles --output-format json --nocomments --hash-as-name --precision

```

AtomJack has an interesting anachronistic side publication called Cybress Blog. It is interlinked with the magazine's issues.