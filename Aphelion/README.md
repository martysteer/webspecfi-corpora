### Aphelion webzine - scrape log

Scrape index page, then get all backissues from that:

```
xmllint --html --xpath '//a/@href' scrape/htmlbackup/index.html | sed 's/ href="\([^"]*\)"/http\:\/\/www\.aphelion\-webzine\.com\1/g' | sort | uniq> scrape/issues.txt
```

Scrape those into htmlbackup, and find more links to each story:

```
xmllint --html --xpath '//a/@href' scrape/htmlbackup/* | sed 's/ href="\([^"]*\)"/\1/g' | sort | uniq | grep -E '(/shorts/|/features/|/serials.html|shorts.html)' > urls.txt
```

some issues have their stories listed directly there, others have an intermediate html page to the story listings, so the command above filters all urls by those patterns.

```
wget --directory-prefix=scrape/htmlbackupsecondlevel  --trust-server-names --wait 3 --tries=3 --waitretry=70 -U 'Mozilla/5.0 (X11; Linux x86_64; rv:30.0) Gecko/20100101 Firefox/30.0' --input-file scrape/aphelion-secondlevel.txt
```

Then get more urls from those and add to the bigger story URL list:
```
xmllint --html --xpath '//a/@href' scrape/htmlbackup/* | sed 's/ href="\([^"]*\)"/\1/g' | sort | uniq | grep -E '(/shorts/|/features/)' > urlssecondlevel.txt
```

And combine manually, into... 2600 URLs!!

Begin a low polite scrape:

```
wget --directory-prefix=scrape/storybackup  --trust-server-names --wait 3 --tries=3 --waitretry=70 -U 'Mozilla/5.0 (X11; Linux x86_64; rv:30.0) Gecko/20100101 Firefox/30.0' --input-file urls.txt
```





Traffy process and join traffy messy metadata:
```
trafilatura --inputdir ./scrape/storybackup --outputdir ./scrape/jsonfiles --output-format json --hash-as-name --no-comments 

python ../traffy-meta.py generate-corpus "./scrape/jsonfiles/*" .

```


TRo extract story title, author metadata from the HTML index files:
```

trafilatura --inputdir ./scrape/htmlbackup --outputdir ./scrape/xmlfiles --output-format xml --hash-as-name --no-comments --links

trafilatura --inputdir ./scrape/htmlbackupsecondlevel --outputdir ./scrape/xmlfiles --output-format xml --hash-as-name --no-comments --links


# The xml reformats all the messy HTML, so we can more reliable xpath select the table cell contents:

xmllint --format --html --xpath '//cell' scrape/xmlfiles/* > scrape/index-info.txt

Find replace to collapse lines:
"<lb/>
"

find, copy, paste and replace:
<p><ref target="(.*?)">(.*?)</ref><lb/>(.*?)<lb/>(.*?)<lb/></p>
\1,\2,\3,"""\4"""


<p><ref target="(.*?)">(.*?)</ref><lb/>(.*?)<lb/></p>
\1,\2,\3


csvmatch -i metadata.csv scrape/index-info-extracted.csv --fields1 title --fields2 title -i --join full-outer --output '1*' '2*'  > results-fuzzy.csv 


``

