# webspecfi-corpora

This data repository contains scraped stories from some old and new web zines.

These are or were all public pages, either from the live web or the internet archive. All rights remain with the original authors and/or the magazines in which their imaginations were published.

The corpora should all be the same structure and can be combined into a larger corpus by concatenating all the corpus.jsonl files together, or merging the CSV files.

This public Nomic Atlas map has all the data uploaded, so you can explore it a bit there: https://atlas.nomic.ai/data/martysteer/webspecfi-corpora/map

[<img src="nomic-atlas-word-embeds-broad-topics.png" alt="nomic-atlas-word-embeds" style="zoom:800%;" width="80%" />](https://atlas.nomic.ai/data/martysteer/webspecfi-corpora/map)

Ad here is the current list of magazines and number of stories from each:

```bash
webspecfi-corpora git:(main) $ wc -l **/*.jsonl | sort -r 
   13976 total
    2990 Bewildering Stories/corpus.jsonl
    2597 Aphelion/corpus.jsonl
     964 ClarkesWorld/corpus.jsonl
     824 Beneath Ceasless Skies/corpus.jsonl
     807 Tor.com/corpus.jsonl
     606 Apex Magazine/corpus.jsonl
     587 Uncanny Magazine/corpus.jsonl
     519 Dargonzine/corpus.jsonl
     500 Weird Fiction Review/corpus.jsonl
     468 Abyss & Apex/corpus.jsonl
     445 Fireside Fiction/corpus.jsonl
     381 Metaphorosis/corpus.jsonl
     361 The Future Fire/corpus.jsonl
     255 MythAxis Magazine/corpus.jsonl
     235 Vestal Review/corpus.jsonl
     213 Cosmic Roots and Eldritch Shores/corpus.jsonl
     193 Bourbon Penn/corpus.jsonl
     160 Fabulist Magazine/corpus.jsonl
     154 Ibn Qirtaiba/corpus.jsonl
     134 Chaos Theory Tales Askew/corpus.jsonl
     121 Quanta/corpus.jsonl
     116 Three-lobed Burning Eye/corpus.jsonl
      94 Would That It Were/corpus.jsonl
      65 Atomjack/corpus.jsonl
      63 Betwixt/corpus.jsonl
      44 Scifi Dimensions/corpus.jsonl
      41 Darker Matter/corpus.jsonl
      39 Capricious SF/corpus.jsonl
```

And the published date distributions, with some validation errors:

```bash
webspecfi-corpora git:(main) ✗ cat **/corpus.jsonl | jq -r '.date'| cut -d- -f1 | sort | uniq -c | sort -r
1135 2023
 932 2022
 854 2020
 811 2017
 756 2018
 738 2016
 725 2015
 708 2011
 677 2021
 675 2019
 648 2013
 626 2014
 606 2012
 458 2010
 339 2009
 309 1999
 304 null
 293 2008
 291 2006
 233 2007
 230 2005
 195 2004
 186 2000
 157 2003
 154 2002
 149 1998
 142 2001
 141 1997
  30 1996
  28 1990
  28 1986
  22 1995
  21 1989
  21 1988
  19 1987
  13 1994
  12 Summer 2022
  11 1993
  10 1992
  10 1991
   9 Spring 2022
   9 November 2007
   9 February 2008
   8 May 2008
   7 December 2001
   7 August 2008
   6 January 1996
   5 November 2006
   5 May 2007
   5 May 2006
   5 January 2018
   4 Winter 2012
   4 Summer 2012
   4 Spring 2012
   4 September 2018
   4 September 2006
   4 September 1997
   4 November 1998
   4 May 2017
   4 May 2000
   4 March 2007
   4 July 2016
   4 July 2006
   4 January 1999
   4 February 2019
   4 December 2017
   4 December 2015
   4 August 1999
   4 August 1997
   4 April 2016
   4 April 2000
   3 September 2015
   3 September 2000
   3 September 1999
   3 October 2007
   3 October 1999
   3 October 1997
   3 November 1999
   3 November 1997
   3 May 1999
   3 June 1999
   3 June 1998
   3 June 1997
   3 July 2000
   3 July 1999
   3 July 1997
   3 January 2000
   3 February 2000
   3 February 1999
   3 December 1999
   3 December 1998
   3 August 1996
   2 September 1996
   2 October 1998
   2 November 1996
   2 November 1995
   2 May 2003
   2 May 1997
   2 May 1996
   2 March 2000
   2 March 1999
   2 March 1998
   2 March 1997
   2 June 2000
   2 June 1995
   2 July 1996
   2 January 2017
   2 January 2007
   2 January 1998
   2 February 1998
   2 February 1997
   2 December 2000
   2 December 1997
   2 August 2000
   2 August 1998
   2 April 1999
   2 April 1997
   1 September 1994
   1 October 2016
   1 May 1998
   1 March 1996
   1 March 1995
   1 March 1994
   1 June 1994
   1 July 2001
   1 December 1994
   1 December 1993
   1 April 1996
```





The data was scraped using wget and trafilatura, then the automatically detected metadata was manually cleanup a little bit, but overall it's not great metadata I'm afraid to say. Any improvements are welcome. Just make a pull reqeust.

Notes of the commands used to produce each of these are in the various README.md files, but I got a little lazy repeating myself over and over so they are incomplete. (hah!) Each magazine has a plain text list of the issue and story URL's that were used for scraping, so the entire corpus could easily be scraped again, if you have another usecase or improved pipeline (e.g. [Jina AI's Reader API](https://jina.ai/reader)). The CSV files have the same metadata fields as the JSONL files, but without the full text.

This collection was created mid-2023 so some of the live magazine sites will have published new literature since then.

There are some amazing stories in here. Please use them fairly and transparently, and cite your sources. Thx.

---

Marty Steer, Freelance Computational Humanist :-P~
2024
