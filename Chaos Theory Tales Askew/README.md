Chaos Theory: Tales Askew
Web magazine from the internet archive.org
Extracted June 2023 by :-P~
---

(method same as the other webzines, but paying attention to targeting the TOC, extracting specific title/author metadata from an XML chunk within a HTML comment. This poses particular parsing and tool knowlege requirements. Luckily the metadata fields can be got at using just textual line matching, and some subsequent post-processing of the text to reform it into CSV)


grep -E '<title>|<o:Author>' scrape/htmlbackup/* > scrape/authors.csv


Then reformed this with find/replace, and some manual correction via Apple Numbers.app.

```
filename,title,author
abigail.htm,Abigail, 
again.htm,Mr,Arthur A. Roberts
alexander.htm,,laptop123
...
```

But, the trafilatura extract does not preserve the filename or source URL correctly, so we will  have to retrieve this (or reconstruct it) next...

Perhaps we can fuzzy join on the Title fields to get the data more closely aligned...


