# Markdown Crash Course
Markdown is a lightweight markup language with plain-text-formatting syntax. It is also a software tool, written in Perl, that converts the plain text formatting to HTML/XHTML and other formats

## What is Markdown used for?
* Creating and formating README files e.g. GitHub
* Creating blogs and websites
* It can also be used to create notes, presentations, self published books, format emails, create documentation etc.
* Convert formatted plain text to structurally correct HTML/XHTML 

## What can be formatted using Markdown?
* Headings and sub-headings
* Numbered lists and bullets
* Italics, bold text
* Links
* Images
* Blocks of Code
* Tables

## Markdown Flavours
The original Markdown syntax specification was outlined by John Gruber (creator of Markdown along with Aaron Swartz) in 2004. Several individuals and organizations added thier own extensions to these specifications and hence were born different flavors of Markdown. These Markdown flavors are also considered to be lightweight Markup languages in their own right. Some of the popular Markdown flavors include:

* CommonMark
* GitHub Flavored Markdown (GFM)
* Markdown Extra
* MultiMarkdown
* R Markdown

Basically these flavors are a superset of Gruber’s basic Markdown syntax and add aditional features such as tables, code blocks, syntax highlighting, URL auto-linking, footnotes etc.

# Basic Syntax

## Comments
<!-- Ths is a Comment -->
To add a comment in Markdown use - \<\!\-\- Comment \-\-\> <br>
Escape characters using a backslash (\\)

## Headings
Levels of headings are created by using # sign.
# Heading 1
## Heading 2
### Heading 3
#### Heading 4
##### Heading 5
###### Heading 6

## Paragraphs and blanks
Use a blank line to separate one or more lines of text, for example:

*The purpose of lorem ipsum is to create a natural looking block of text (sentence, paragraph, page, etc.) that doesn't distract from the layout. A practice not without controversy, laying out pages with meaningless filler text can be very useful when the focus is meant to be on design, not content.*

*The passage experienced a surge in popularity during the 1960s when Letraset used it on their dry-transfer sheets, and again during the 90s as desktop publishers bundled the text with their software. Today it's seen all around the web; on templates, websites, and stock designs. Use our generator to get your own, or read on for the authoritative history of lorem ipsum.*

### Line breaks
Line breaks can be created by ending a line with two or more spaces, and then typing a return. Or by adding a \<br\> tag at end of line.

## Text Emphasis
### Bold text
To bold text, add two asterisks or underscores before and after a word or phrase. To bold the middle of a word for emphasis, add two asterisks without spaces around the letters. <br>
**Omnium Rerum Principia Parva Sunt** <br>
__Pecunia Nervus Belli__ <br>
Non Omnia **P**ossumus Omnes <br>

### Italics
To italicize text, add one asterisk or underscore before and after a word or phrase. To italicize the middle of a word for emphasis, add one asterisk without spaces around the letters. <br>
*Omnium Rerum Principia Parva Sunt* <br>
_Pecunia Nervus Belli_ <br>
Non Omnia *P*ossumus Omnes <br>

### Bold and Italics
To emphasize text with bold and italics at the same time, add three asterisks or underscores before and after a word or phrase. To bold and italicize the middle of a word for emphasis, add three asterisks without spaces around the letters. <br>
***Permitte Divis Cetera***  
___Anil Desperandum___  
 Carpe ***D***iem
 
 ## Strikethrough
 To achive strikethrough use two tilde characters (\~\~).  
 ~~Pecunia Nervus Belli~~

## Lists

### Numbered Lists
To create an ordered list, add line items with numbers followed by periods. The numbers don’t have to be in numerical order, but the list should start with the number one.

1. Ars Longa, Vita Brevis 
2. Omnium Rerum Principia Parva Sunt
3. Vivamus, Moriendum Est

Or, alternatively <br>
1. Ars Longa, Vita Brevis 
1. Omnium Rerum Principia Parva Sunt
1. Vivamus, Moriendum Est

### Bullets
To create an unordered list, add dashes (-), asterisks (*), or plus signs (+) in front of line items. Indent one or more items to create a nested list.

- Ars Longa, Vita Brevis 
- Omnium Rerum Principia Parva Sunt
- Vivamus, Moriendum Est <br>

* Ars Longa, Vita Brevis 
  * Omnium Rerum Principia Parva Sunt
    * Vivamus, Moriendum Est <br>

## Blockquotes
To create a blockquote, add a > in front of a paragraph.
> Veritas Odit Moras 

Blockquotes can contain multiple paragraphs. Add a > on the blank lines between the paragraphs.  
> Timendi Causa Est Nescire
> Vivamus, Moriendum Est

Blockquotes can be nested. Add a >> in front of the paragraph you want to nest.
> Nemo Sine Vitio Est <br>
> Magna Servitus Est Magna Fortuna
 >> Vitam Impendere Vero 

Blockquotes can contain other Markdown formatted elements. Not all elements can be used — you’ll need to experiment to see which ones work.
> #### Timendi Causa Est Nescire
>
> - Nemo Sine Vitio Est
> - Profits were higher than ever.
>
>  *Magna Servitus Est Magna Fortuna*




## References
https://www.markdownguide.org
