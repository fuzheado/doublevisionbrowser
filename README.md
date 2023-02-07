# doublevision browser
A Jupyter notebook that browses Met Museum object and corresponding Wikidata items in parallel.

This notebook takes a list of Met Museum object IDs, and makes a clickable list from them.

Paste in a list of Met object IDs into the first box, which can be one per line or comma separated. Pressing the "Populate list" button will fill in the second list.

When an item is clicked, the Met Museum API is consulted and basic information is returned,
including an image if available as open access/public domain/CC0.

At the same time, Wikidata is consulted to see if an item exists with that same Met object ID,
using property P3634. If it is found, that item is displayed, as well as if there is an
image from Wikimedia Commons.

In this way, we can inspect sets of Met objects and compare what we have in Wikidata/Commons.

If the image is missing in Wikidata/Commons, there is a link to run GLAMingest, an earlier
tool created to create missing Wikidata or upload Commons files

## Running the interface
* You can run the browser with the MyBinder link: https://mybinder.org/v2/gh/fuzheado/doublevisionbrowser/HEAD?urlpath=voila%2Frender%2FDoublevision.ipynb
* This may take a while to run since it is generating a whole virtual environment for you

## Screenshot
![alt text](Pasted_Image_2_7_23__9_26_AM.png?raw=true)
