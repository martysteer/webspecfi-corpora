Mythaxis Magazine - web scrape log

```
echo 'https://mythaxis.co.uk/archive.html' > mythaxis.urls.txt
wget --directory-prefix=scrape/htmlbackup  --trust-server-names --wait 10 --tries=3 --waitretry=70 -U 'Mozilla/5.0 (X11; Linux x86_64; rv:30.0) Gecko/20100101 Firefox/30.0' --input-file mythaxis-urls.txt
xmllint --html --xpath '//li/a/@href' scrape/htmlbackup/archive.html | sed 's/ href="\([^"]*\)"/https\:\/\/mythaxis\.co\.uk\/\1/g' | sort | uniq >> mythaxis-urls.txt

Then some manual tidy up, before scraping.

xmllint --html --xpath '//li/a/@href' scrape/htmlbackup/*.html | sed 's/ href="\([^"]*\)"/https\:\/\/mythaxis\.co\.uk\/\1/g' | sort | uniq > mythaxis-stories.txt
xmllint --html --xpath '//td/a/@href' scrape/htmlbackup/*.htm | sed 's/ href="\([^"]*\)"/https\:\/\/mythaxis\.co\.uk\/\1/g' | sort | uniq >> mythaxis-stories.txt


Scrape the stories.

wget --directory-prefix=scrape/htmlbackup  --trust-server-names --tries=3 --waitretry=70 -U 'Mozilla/5.0 (X11; Linux x86_64; rv:30.0) Gecko/20100101 Firefox/30.0' --input-file mythaxis-stories.txt 



trafilatura --inputdir ./scrape/htmlbackup --outputdir ./scrape/jsonfiles --output-format json --hash-as-name --no-comments

python ../traffy-meta.py generate-corpus "./scrape/jsonfiles/*" .

```

After pulling the metadata out of the index files and manually cleaning it up, I joined on the title. This merged a lot of the URL's into the traffy metadata, but there were some missing entries from the first scrape. So I put those story urls into extra-stories.txt and scraped again.
```
wget --directory-prefix=scrape/htmlbackupextra  --trust-server-names --tries=3 --waitretry=70 -U 'Mozilla/5.0 (X11; Linux x86_64; rv:30.0) Gecko/20100101 Firefox/30.0' --input-file scrape/extra-stories.txt

trafilatura --inputdir ./scrape/htmlbackupextra --outputdir ./scrape/jsonfilesextra --output-format json --hash-as-name --no-comments

python ../traffy-meta.py generate-corpus "./scrape/jsonfilesextra/*" .

```

Scraped those, then manually merged the metadata and jsonl with the other one, for a complete set! Hooray!

- MS 2023





