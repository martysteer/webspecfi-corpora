put first URL into urls.txt file
wget --directory-prefix=scrape/htmlbackup/ --input-file=wtiw-urls.txt --trust-server-names
inspect HTML, and select out the issue URL's
xmllint --html --xpath '//option' scrape/htmlbackup/index.html > scrape/issues.txt       
manually edit, find/replace and regex the issues.txt file
construct memento URL's and add to bottom of urls.txt file
wget --directory-prefix=scrape/htmlbackup/ --input-file=wtiw-urls.txt --trust-server-names


This is a difficult site to scrape. The mementos have significantly different HTML page layouts, and none of these are very easy to xpath links, story titles and authors out of. Also, strutural changes in the early issues means the first issue's stories were .doc files, the second issue has a stories.html page with links to files/html and otherwise, and all of the HTML is dreamweaver based, so full of verbose <font> markup. haha!


https://web.archive.org/web/20050421124105/http://www.wouldthatitwere.com/aprjun2001/eve.html
https://web.archive.org/web/20050421124105/http://www.wouldthatitwere.com/aprjun2001/ahead.html
https://web.archive.org/web/20050421124105/http://www.wouldthatitwere.com/aprjun2001/onelasttime.html

I ended up manually copy/paste the HTML blocks with the stories and issues info into a text file and cleaned that up a little bit. Then chunked it and fed it to ChatGPT, and asked it to clean and extract the info in CSV format. Then I used Apple Numbers to open that csv file and tidy up the values manually. That then is the next list of story URL's.

```
csvtk cut -f 6 scrape/issues-extracted-cleaned.csv >> wtiw-urls.txt 

wget --directory-prefix=scrape/htmlbackup/ --input-file=wtiw-urls.txt --trust-server-names --wait=2

trafilatura --inputdir ./scrape/htmlbackup --outputdir ./scrape/jsonfiles --output-format json --hash-as-name

(cleanup metadata tables)

csvtk join --outer-join --fields "fingerprint;fingerprint" ./metadata.csv ./scrape/metadata-cleaned.csv > ./metadata2.csv

python ../traffy-meta.py merge-csv-into-jsonl "metadata.csv" "corpus.jsonl"
```

