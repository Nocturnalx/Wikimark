# Wikimark
This document details how to use wikimark, why it was made, and how it works.

**Why?** - Wikimark was made to provide short hand syntax for the common and essential HTML used in the utladal wiki in order to bring better quality of life for editors. This should both speed up wpm, be less confusing, and less prone to errors than having to write out the full HTML syntax for each element.

## How to use
This section contains a cheat sheet you can quickly refer back to when creating new entries on the wiki, as well as a more in depth explanation of each tag.

### Cheat sheet
|Effect|Tag|
|---|---|
|Bold Text|`(b)Text`|
|Quote Exerpt|`(quote)Text`|
|Exerpt with font|`(font)(Font Family)Text`|
|Embedded Video|`(vid)(https://youtube.com/watch?v=videoID)`|
|Image|`(img)(https://i.imgur.com/imgID.xyz)`|

**Please note** - *the parser works on a line by line basis meaning that if you include a new line(press enter) in the succeding text the text after the new line will be excluded from the generated html. - See the "Line by Line" section under "How does the wiki parser work?"*

### Detailed outline
1. **Bold - (b)**
    - Usage: `(b)Text goes here.`
    - Effect: This will place the subsequent text into a html `<b>` tag. i.e. `<b>Text goes here</b>`

2. **Quote - (quote)**
    - Usage: `(quote)Text goes here.`
    - Effect: This will place the subsequent text into a html `<span>` tag with an indent and will wrap the text in quotation marks. i.e. `<span style="padding: 5vw">"Text goes here."</span>`

3. **Font - (font)**
    - Usage: `(font)(Font Family)Text goes here.`
    - Effect: This will place the subsequent text into a html `<span>` tag with the given font being set with the style attribute. i.e. `<span style="font-family: Font Family;">Text goes here</span>`
    - Note: the font family you use should be one that is recognised by most modern browsers and spelling is important.
    
    List of common browser safe fonts:

    |Font (desc)|Wikimark|
    |---|---|
    |Arial (sans-serif) | (font)(Arial)|
    |Verdana (sans-serif) | (font)(Verdana)|
    |Tahoma (sans-serif) | (font)(Tahoma)|
    |Times New Roman (serif) | (font)(Times New Roman)|
    |Georgia (serif) | (font)(Georgia)|
    |Garamond (serif) | (font)(Garamond)|
    |Courier New (monospace) | (font)(Courier New)|
    |Brush Script MT (cursive) | (font)(Brush Script MT)|

4. **Video - (vid)**
    - Usage: `(vid)(https://youtube.com/watch?v=videoID)`
    - Effect: This will create an `<iframe>` embedded video player for the given youtube video, or other video source but some may not work.
    - Note: Iframes require a slightly different link: `youtube.com/embed/...` however you can directly paste in either the default link or the embed link and the parser will be able to detect regular links and translate them into embed links.

5. **Image - (img)**
    - Usage: `(img)(https://i.imgur.com/imgID.xyz)`
    - Effect: This will create an `<img>` tag with the "src" attribute set to the provided link and the css class set to a pre-made class which sets the max size/width. i.e. `<img class="page-img" src="https://i.imgur.com/imgID.xyz">`
    - Note: You do not have to use imgur for the image source, although, it is probably the most useful image hosting service. Also make sure that the link you are pasting is to the actual image resource and not just to the page that is displaying the image.

## How does the wiki mark parser work?
#### Line by Line
After sending the text from the `/wiki/editor/..` page, the main text body gets split by each newline into an array. 

ex:
```
[
    "(b)Overview",
    "John Doe (born 20/04/1969) is xyz and did something or other. Today he is doing something else.",
    "\n",
    "(b)John said",
    "(quote)Shh, I'm mewing!",
    "(b)A Relevant Video,"
    "(vid)(video_link.com/ID)"
]
```
The parser checks for the wikimark tag at the start of each new line, and only the start of the new line, so it is important that there are no spaces or other characters infront of the `(tag)`. And only the text that is included on the same line as the `(tag)` is included within the generated HTML.

For example, say you have a `(quote)` tag and you copy over a piece of text that has a new line in it:
```
(quote)This quote has two lines.
It might be a pair of short paragraphs or something.
```
The resulting HTML generated will look like:
```
<span>"This quote has two lines."</span> << quote HTML
It might be a pair of short paragraphs or something. 
    ^^ This line here is outside of the quotes
```
So it is important that you make sure everything is on the same line:
```
(quote)This quote has two lines. It might be a pair of short paragraphs or something.
=
<span>"This quote has two lines. It might be a pair of short paragraphs or something."</span>
```

#### Parsing result and reversing
The parser will replace each line 1:1 with its corresponding HTML. The parser will also include, at the start of the line, a HTML comment `<!--(tag)-->` which identifies this HTML as a generated tag which can be re-interpreted back to a wikimark tag. This means that you can include html directly yourself which wont be reinterpreted as a wikimark tag so long as it is one of the allowed HTML elements.

Allowed elements:
- `<b>`
- `<p>`
- `<i>`
- `<span>`
- `<a>`
- `<img>`
- `<iframe>`

#### Future plans
Currently `<a>` tags are not included in the parser as I have only written it to be a line by line thing, however, I am planning on creating a short hand for this. For now though you will just have to write them out as we have been doing as direct HTML tags within the corpus.
