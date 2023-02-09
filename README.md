# doublevision browser

This python Jupyter notebook browses Met Museum objects and corresponding Wikidata items in parallel. It takes a list of Met Museum object IDs, and makes a clickable list from them.

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

## ChatGPT use
Surprise! The first basic version of this tool was created through a dialogue with ChatGPT. 

I was familiar with Jupyter and its widget system (ipywidgets), but  ChatGPT was useful in creating the basic groundwork as I filled in the blanks. Overall, I have been impressed with ChatGPT's functionality as a code assistant. For well-known libraries like pandas and ipywidgets, I have been very satisfied with its help. With more specialized, more obscure modules like pywikibot, it has been less useful.

### Prompt
```
I want you to help create a user interface in Python for use with a Jupyter notebook that can visit an API via URL, grab several fields from the JSON that is returned, and display it to the user. This tool should have a text box that can take a list of numbers, which are called identifiers. The user can then press a button for these identifiers to populate a list box. In this list box, if the user clicks on an identifier, the API call is instantly made, and the program should display text fields on screen from the JSON, and an image. The JSON text fields are "objectID" and "title" and "department" and the image can be found in the field called "primaryImage." Produce the code that will create this interface.
```
Interestingly, the code for a Jupyter notebook was largely functional and correct, except for the display of the JPG file.

### Code generated
The code below from ChatGPT works if you replace the generic API URL with a functional one, such as one from The Met. The only other shortcoming is that the content is output to the console on JupyterLab, instead of inside a notebook cell. This can be corrected by making a widgets.Output() object for the HTML.
```
import requests
import ipywidgets as widgets
from IPython.display import display, HTML

def on_button_clicked(b):
    global identifiers
    identifiers = textbox.value.split(',')
    listbox.options = identifiers

def on_list_item_selected(change):
    objectID = change['new']
    url = f"https://api.example.com/{objectID}"
    response = requests.get(url)
    data = response.json()
    title = data['title']
    department = data['department']
    primaryImage = data['primaryImage']
    display(HTML(f"<p><b>Object ID:</b> {objectID}</p>"))
    display(HTML(f"<p><b>Title:</b> {title}</p>"))
    display(HTML(f"<p><b>Department:</b> {department}</p>"))
    display(HTML(f"<img src='{primaryImage}'>"))

textbox = widgets.Text(description="Identifiers:")
button = widgets.Button(description="Populate list")
button.on_click(on_button_clicked)
listbox = widgets.Select(options=[], description="Object ID:")
listbox.observe(on_list_item_selected, names='value')

display(textbox)
display(button)
display(listbox)
```