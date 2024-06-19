## Tor.com
https://www.tor.com/



```
printf 'https://www.tor.com/category/all-fiction/page/%s/\n' {1..55} > tor-fiction-urls.txt
wget --directory-prefix=scrape/indexpages --input-file=scrape/tor-fiction-urls.txt --trust-server-names --wait 1
xmllint --html --xpath '//h2/a/@href' scrape/indexpages/* | sed 's/ href="\([^"]*\)"/\1/g' | sort | uniq > tor-urls.txt

# Slow scrape, with log, because of a lot of 403 forbidden errors from this server:

wget --directory-prefix=scrape/htmlbackup --input-file=tor-urls.txt --trust-server-names --wait 10 --tries=3 --waitretry=60 --append-output=log.txt -U 'Mozilla/5.0 (X11; Linux x86_64; rv:30.0) Gecko/20100101 Firefox/30.0'

trafilatura --inputdir ./scrape/htmlbackup --outputdir ./scrape/jsonfiles --output-format json --hash-as-name --no-comments



```

There is quite a lot of tags and category info in the HTML, so let's scrape that out, filter and clean it up, and then join it into the metadata:

```
xmllint --html --xpath '//meta[@property="og:url"]/@content | //article[not(@class="related-post")]/@class' scrape/htmlbackup/* | sed 's/ content="\([^"]*\)"/\n\1,/g' | sed 's/ class="\([^"]*\)"/\"\1\"/g' > classes.txt
```

Manually tidied the text up to look like this:

```
source,classes
https://www.tor.com/2008/07/20/after-the-coup/,"category-original-fiction tag-hard-science-fiction tag-john-harris tag-john-scalzi tag-old-mans-war tag-original-fiction tag-science-fiction tag-short-fiction tag-tor-com-original tag-wars genre-science-fiction genre-space-opera category-all-fiction"
https://www.tor.com/2008/07/20/down-on-the-farm/,"category-original-fiction tag-charles-stross tag-cthulhu-mythos tag-fantasy tag-horrorcraig-phillips tag-humor tag-laundry-files tag-lovecraftian tag-original-fiction tag-short-fiction tag-tor-com-original genre-fantasy genre-lovecraftian category-all-fiction"
https://www.tor.com/2008/10/09/jackandtheaktuals/,"category-original-fiction tag-marcos-chin tag-mathematics tag-original-fiction tag-rudy-rucker tag-science-fiction tag-short-fiction tag-tor-com-original genre-science-fiction category-all-fiction"
```

Now, going to use GPT4 to help me write a function which processes this.... Done. Function is in traffy-meta. Run it like this, and join the resulting csv with the metadata.csv:

```
python ../traffy-meta.py split-classes "./scrape/classes.txt" "./scrape/classes.csv"

# Rename columns and join
csvtk rename --fields "tag,category" --names "tags,categories" ./scrape/classes.csv > ./scrape/classes2.csv
csvtk join --left-join --fields "source;source" ./metadata.csv ./scrape/classes2.csv > ./scrape/metadatajoined.csv

# Rearrange fields
csvtk cut -f 1-5,18,17,8-15,19- ./scrape/metadatajoined.csv > metadata2.csv

rm ./scrape/classes2.csv



```





