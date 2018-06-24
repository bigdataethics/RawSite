---
title: "Handling data in JSON and CSV formats"
date: 2017-10-28T09:16:42-05:00
draft: false
index: true
tag: "tutorials"
---

Python includes packages for handling data in JSON and CSV formats. JSON stands for JavaScript Object Notation format. It is basically a key-value pair store. CSV stands for Comma Separated Variables. It is a list of data values separated by comma (or some other delimiter).

If you open the tweets.txt file saved in the previous session, you will notice that each line is in the JSON format. Each tweet is enclosed in braces {}, and within the braces are key:value pairs separated by commas. 


The following import commands makes their methods available.
```
import json
import csv
```


The tweet data got from Twitter is in text format and therefore needs to be converted to JSON so that the various attributes can be easily accessed.


'''
with open("input_file","r") as infile, open("output_file","w") as outfile:
 for line in infile.readlines():
       if not line.strip():
           continue
       elif line:
           outfile.write(i) 
'''