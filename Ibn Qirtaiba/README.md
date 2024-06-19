
wget --directory-prefix=scrape/htmlbackup/ --input-file=ibn-urls.txt --trust-server-names --wait 5

# Split issue pages into story section html files

for f in ../htmlbackup/iq-*.html; do iconv -f ISO-8859-1 -t UTF-8 "$f" | awk '/<h2><a/{i++}{print > ("../sections/" "'"${f%.*}"'-section" i ".html")}' ; done

Use traffy to process those.

(webscifi-3.9) ➜  Ibn Qirtaiba git:(quanta) trafilatura --inputdir ./scrape/sections --outputdir ./scrape/txtfiles --output-format txt --nocomments --hash-as-name --precision 

(webscifi-3.9) ➜  Ibn Qirtaiba git:(quanta) trafilatura --inputdir ./scrape/sections --outputdir ./scrape/jsonfiles --output-format json --nocomments --hash-as-name --precision

Open the traffy metadata.csv file and clean it up a bit. Removed non-story pages, populated some of the author and issue date fields, and manually looked up a dozen or so missing fingerprints. Then converted the metdata.csv file into jsonl:

(webscifi-3.9) ➜  csvtk csv2json websites/Ibn\ Qirtaiba/metadata2-TODO-merge-to-corpusjsonl.csv | jq -c '.[]' > websites/Ibn\ Qirtaiba/metadata2.jsonl

And finally, merged the extractred text from the text files back into the new JSONL. Matching uses fingerprints and txtfiles filenames.

'''
python ../traffy-meta.py merge-text-into-jsonl "./scrape/txtfiles/*" "./metadata2.jsonl" .
'''




# Interesting pages found on this site:

ylMes2jZEXXfP84w+t4nIdH1/MQ=,Coolest 10 SF Sites #10,,,,,,,,,,,
B+Iea8Vb6fJSA/J7FnwIjVU741s=,Coolest 10 SF Sites #4,,,2010-01-01,,,,,,,,
uYjgfBHNgRE1iZlZJTjiBtucDCY=,Coolest 10 SF Sites #1,,,2010-01-01,,,,,,,,
3ww+cbnikicsRrj30wXQ18OUENo=,Coolest 10 SF sites #2,,,1999-01-01,,,,,,,,
xbJ2qgs7ybxOUkZ1YAAk0eBbOQA=,Coolest 10 SF Sites #5,,,2010-01-01,,,,,,,,
zWIxdiWx8gddON7FnDfapPLabac=,Coolest 10 SF Sites #7,,,1997-10-09,,,,,,,,
IlLZ1Qj3fLpK4S4ZUjGMZEFCO0s=,Coolest 10 SF Sites #8,,,,,,,,,,,
tXKIJ5HtbAgS76ziU8lDYkkXnVQ=,Coolest 10 SF Sites #9,,,,,,,,,,,
MH5rUB5jYlcNXFq44TpRabGlvUg=,Coolest 10 SF Sites #6,,,2010-01-01,,,,,,,,
gbpYBkjFSGdbiz5ZWj8a1B7mfBY=,Coolest 10 SF Sites #11,,,,,,,,,,,


Some statistics from a Ibn Qirtaiba website user survey: 
gv1z/Dqk62auG1R3Y099sEFOULQ=,"Article: SF Audiences, part 1 © 1999 Daehee Park",,,1998-09-08,,,,,,,,
PD4DZ2vYmu/WF857msNdNL69tRo=,"Article: SF Audiences, part 2 © 1999 Daehee Park",,,1999-01-01,,,,,,,,

jCc4YqjGd04AOJ7slzlYrP31yLw=,SF on the Internet,,,,,,,,,,,
lPvVv2xi3O8UDHnJYv8/3kXqmWk=,Science Fiction: History and Criticism,,,,,,,,,,,


