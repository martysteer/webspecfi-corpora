### Apex Magazine - web scrape log and notes


```
printf 'https://apex-magazine.com/category/short-fiction/page/%s/\n' {1..104} > apex-urls.txt

wget --directory-prefix=scrape/htmlbackup  --trust-server-names --wait 3 --tries=3 --waitretry=70 -U 'Mozilla/5.0 (X11; Linux x86_64; rv:30.0) Gecko/20100101 Firefox/30.0' --input-file apex-urls.txt

xmllint --html --xpath '//h3[@class="elementor-post__title"]/a/@href' scrape/htmlbackup/* | sed 's/ href="\([^"]*\)"/\1/g' | sort | uniq > apex-urls.txt 

wget --directory-prefix=scrape/storiesbackup  --trust-server-names --wait 3 --tries=3 --waitretry=70 -U 'Mozilla/5.0 (X11; Linux x86_64; rv:30.0) Gecko/20100101 Firefox/30.0' --input-file apex-urls.txt
```


To get the tags/categories from the <article> classes:

```
xmllint --html --xpath '//link[@rel="canonical"]/@href | //div[@data-elementor-type="single-post"]/@class' scrape/storiesbackup/* | sed 's/ href="\([^"]*\)"/\1,/g' | sed 's/ class="\([^"]*\)"/\"\1\"/g' > classes.txt

python ../traffy-meta.py split-classes "./scrape/classes.txt" "./scrape/classes.csv"
```

Manually tidied the text up into CSV format. Next, use the function is in traffy-meta to split classes, and join with the default traffy json metadata:

I also had to manually get the title and author names from the stories because traffy didn't scrape that out and it doesn't exist in any meta html tags.

```
xmllint --html --xpath '//link[@rel="canonical"]/@href | //div[@data-id="72a17fc"]//h2/text() | //div[@data-id="ced5b79"]//span[@class="elementor-icon-list-text elementor-post-info__item elementor-post-info__item--type-date"]/text() | //div[@data-id="522c903"]//div[@class="elementor-shortcode"]/text()' scrape/storiesbackup/* | sed 's/ href="\([^"]*\)"/\1,/g' | sed 's/ class="\([^"]*\)"/\"\1\"/g'
```

Both classes.csv and titles.csv can be joined using the source column however.


```
trafilatura --inputdir ./scrape/storiesbackup --outputdir ./scrape/jsonfiles --output-format json --hash-as-name --no-comments

python ../traffy-meta.py generate-corpus "./scrape/jsonfiles/*" .

csvtk join --fields "source;source" ./metadata.csv ./scrape/titles.csv > ./metadata2.csv
csvtk join --fields "source;source" ./metadata2.csv ./scrape/classes.csv > ./metadata3.csv
```

Manually rearranged and deleted columns, then re-exported to CSV. Final task is to merge the new metadtaa back into the JSONL.

```
python ../traffy-meta.py merge-csv-into-jsonl "metadata.csv" "corpus.jsonl"
```

Done! - MS, 2023

