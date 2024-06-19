# Bewildering stories - web scrape log

This one is quite large and well structured. A precision scrape should work very well.

Gathered index pages, and downloaded those with wget, then extracted issues, and then stories href links, and put those into .txt files. Then scraped all the stories and scraped the text, got the traffy metadata.

For the issue metadata, the issue index pages are really useful and published as tabulat HTML pages. So, converted them to csv files using trafilatura. This transforms the strings into a custom markdiwn-esque table format, using pipe (| | |) as line delimeters, and (||) as cell delmis.

So, we can gather all the tabulat metadata now by string cleaning the single lines and importing into apple numbers with custom separators.

```
trafilatura --inputdir ./scrape/indexes --outputdir ./scrape/csvfiles --output-format csv --hash-as-name --no-comments --links

cat scrape/csvfiles/* | sed 's/\t|/\n/g' | sed 's/| | |/\n/g' > scrape/indexes-xtract.csv  
```

and open in apple numbers using (double pipe) as field delimeter, and clean up a bit. This added issue number column mainly. The other columns were already extracts using trafilatura. However, the source URL is not present, so let's try to join that back in, by:

1. extract filename, html title and metadata author into CSV.
2. join to URL by the file.html name. They are mostly unique.
3. Join resulting data table into metadata.csv on title field.

```
for f in ./scrape/stories/*; do echo "$f"; xmllint --format --html --xpath "//head/title" $f ; done | sed 's/\.\/scrape\/stories\///g' > scrape/sources-titles.txt
```

Manually tidied up to CSV.

Then duplicated and prepared the stories urls file, matching the last part of the path and putting it into a second column:

```
Regex find: ([^/]+)(?=[^/]*/?$)
Regex replace" \1,\1
```

Now join. Had to manually correct some parsing errors and stuff, but after fixing those it joined quite nicely. I then manually removed empty records:

```
csvtk join --outer-join --fields "file;file" ./scrape/sources-titles.csv ./scrape/sources-urls.csv > ./scrape/sources-joined.csv
```

Now, with clean-ish title->url mapping csv, join back into metadata2.csv, and manually tidy up!

```
csvtk join --outer-join --fields "title;title" ./scrape/metadata2.csv ./scrape/sources-joined.csv > ./scrape/metadata3.csv
```

Then export to CSV, replace metadata.csv, and python traffy-meta merge.

```
python ../traffy-meta.py merge-csv-into-jsonl "metadata.csv" "corpus.jsonl"
```

It's not perfect, but it's a pretty good attempt at augmenting and refining the metadata, in bulk.

MS, 2023 :-P