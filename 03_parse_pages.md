Task: given a random HTML page. Return an array containing an image url and image title. Image title with elements index number must be a key to an url so an element could be reached directly. Propose any live URL to solve the problem.

Here is the link to [jsfiddle](https://jsfiddle.net/sergej_gavrilenko/ugs3a6qs/)

Short description about how it works:
* User put html source to the textarea and click 'Parse'
* Application parse source code using DOMParser
* App selects all img tags from parsed dom object
* App goes through all imgages and checks if src is defined (if full link to the image is present and image can be reached)
* If src available it puts new object with title and url to the list of parsed images
* At the end it dumps all parsed results to the console.
