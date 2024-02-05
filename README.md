## Description
Analyzing My Little Pony language, and how much each pony speaks. This is an exercise performed using comand line tools in order to search thorugh dialog data in the form of a CSV file.

## Size of the dataset
- wc -l clean_dialog.csv
- result: 36860 including the header => 36859 entries

## Structure of the data
### Get the headers: 
    head -n 1 clean_dialog.csv 
Result: "title","writer","pony","dialog".
### Get details about the fields:
    csvcut -c title,writer,pony,dialog clean_dialog.csv  | csvstat

Result:
- title : contains Text, most common entry is *My Little Pony The Movie*
- writer : contains Text, most common is *Amy Keating Rogers*
- pony : contains Text, most common values are *Twilight Sparkle*, *Rainbow Dash*, *Pinkie Pie*, *Applejack*, *Rarity*
- dialog : contains Text, most common entries are: *Huh?*, *Whoa!*, *Eeyup.*, *What?*, *What?!*

Note: *csvstat clean_dialog.csv* provides both column information and row count to get answers to the questions above.

## Number of episodes covered
    csvcut -c title clean_dialog.csv | uniq | wc -l
Result: 198 including the header "Title" so we have 197 episodes.

## Possible issues
eg. "Boast Busters","Chris Savino","Snips","[nervous laughter] We<U+0097> We have a<U+0097> a tiny problem."

eg. "Boast Busters","Chris Savino","Spike","No, you don't understand, it's<U+0097>"

- These encodings "<U+0097>" represent yelps or hiccups while a pony is speaking and they're not formatted like other sounds such as [gasp] or [nervous laughter]. They could play a role in identifying emotion during the speech such as fear.

- The entries don't have timestamps, making it hard to pinpoint issues in dialog in the video episodes such as the ones above, to figure out what is not being accurately represented. For example, it took me quite a bit to find the exact moment of the above-mentioned dialog in the Boast Busters episode.

- The pony column includes values such as "All sans Twilight Sparkle" or "All except Twilight Sparkle and Fluttershy" which we have to account for when evaluating how often a pony speaks. Using "grep" will also count these entries because it's not aware of the keywords "except" or "sans". Even then, there are some versions of a single pony (eg. Twilight Sparkle) that are not necessarily the pony itself, it could be seen as a different character if we look into technicalities: "Illusion Twilight Sparkle" or "Mean Twilight Sparkle"

