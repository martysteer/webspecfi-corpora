## Uncanny magazine - web scrape log

```
trafilatura --sitemap "https://www.uncannymagazine.com/" --list > sitemap.txt
```

Can't determine the stories from the URL structures, so going to scrape the fiction and backissues pages for story URL's.

```§
printf 'https://www.uncannymagazine.com/type/fiction/page/%s\n' {2..38} >> uncanny-index-urls.txt§
printf 'https://www.uncannymagazine.com/type/poetry/page/%s\n' {2..22} >> uncanny-index-urls.txt 
printf 'https://www.uncannymagazine.com/type/reprints/page/%s\n' {2..5} >> uncanny-index-urls.txt 
printf 'https://www.uncannymagazine.com/type/all-honors/page/%s\n' {2..7} >> uncanny-index-urls.txt

wget --directory-prefix=scrape/indexpages --input-file=scrape/uncanny-index-urls.txt --trust-server-names --wait 1 
xmllint --html --xpath '//h2/a/@href' scrape/indexpages/* | sed 's/ href="\([^"]*\)"/\1/g' | sort | uniq > uncanny-urls.txt
wget --directory-prefix=scrape/htmlbackup --input-file=uncanny-urls.txt --trust-server-names --wait 1 
```

Now, process the scraped stories, extract metadata, manually clean it a bit, and put the
clean metadata back into the corpus.jsonl:

```
trafilatura --inputdir ./scrape/htmlbackup --outputdir ./scrape/jsonfiles --output-format json --hash-as-name --no-comments
python ../traffy-meta.py generate-corpus "./scrape/jsonfiles/*" .
sed 's/ - Uncanny Magazine//g' metadata.csv > metadata2.csv
rm metadata.csv
mv metadata2.csv metadata.csv
python ../traffy-meta.py merge-csv-into-jsonl "metadata.csv" "corpus.jsonl"
```
