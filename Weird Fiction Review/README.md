## Weird Fiction Review - corpus scrape commands

```
echo 'https://weirdfictionreview.com/category/fiction/' > wfr-urls.txt
trafilatura --sitemap "https://weirdfictionreview.com" --list > wfr-urls.txt
```

Sitemap urls don't allow us to determine fiction from other story types, so manually constructured file with fiction page links, and scraped that, to get URL's of all fiction stories.

```
wget --directory-prefix=scrape/fictionpages/ --input-file=wfr-fiction-pages.txt --trust-server-names --wait 1
xmllint --html --xpath '//h3 | //p[@class="entry-authors"] | //a[@rel="tag"]' scrape/fictionpages/* > tmp.txt
```

Then used regex and find/replace to convert this into a csv file, and manually tidies it up. Then cut the URL column out to scrape the story data.

```
csvtk cut -H -f1 scrape/wfr-fiction-pages.csv >> wfr-urls.txt
wget --directory-prefix=scrape/htmlbackup/ --input-file=wfr-urls.txt --trust-server-names --wait 1 
trafilatura --inputdir ./scrape/htmlbackup --outputdir ./scrape/jsonfiles --output-format json --hash-as-name --links
```

So this website scrapes beautifully! The manual cleanup wasn't necessary. Just some manual cleanup to the traffy-meta compiled metadata.csv file is needed, then those changes can be pushed back into the JSONL corpus (so metadata.csv and corpus.jsonl are in sync).

```
python ../traffy-meta.py generate-corpus "./scrape/jsonfiles/*" .
csvtk cut -f -comments metadata.csv > metadata2.csv
sed 's/ | Weird Fiction Review//g' metadata2.csv > metadata.csv
rm metadata2.csv
python ../traffy-meta.py merge-csv-into-jsonl "metadata.csv" "corpus.jsonl"
```

Done - MS, 2023


