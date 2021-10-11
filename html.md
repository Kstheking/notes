# HTML

The html declarations is not case sensitive

## Attributes 

- lang attribute on html tag `lang="en-US"`
- title attribute defines extra info and is displayed as tooltip when hovering on the element  `<p title="this is a fuckin para"> hey </p>`
- attributes also work if without quotes (only if they are single no space words like https://fuck.everyone.com ) but yea don't

## Headings, Paragraphs

- each heading has its own default size however it can be changed `<h1 style="font-size:60px;">Heading 1</h1>`
- the browser automatically removes any extra spaces (trimming down to one space) and line breaks (happens within h1 p and mostly all tags I suppose)
- with `pre` which stands for preformatted text the space won't be trimmed nor line breaks will be trimmed 

## Styles and formatting

- background-color, font-family, font-size:300%, (these all defined inside style attribute or in css stylesheet
- A screen reader will pronounce the words in `<em>` with an emphasis, using verbal stress.
- `mark` element marks or highlights an element 
- `del`to mark deleted text, usually it will get striked through
- `ins` to mark inserted text, usualy browser will display it as underlined 
- `sub` for subscript `sup` for superscript 

## Quotations 
 
- blockquote gets indented 
- `q` element gets quotation marks around them
- `abbr` to show abbreviations 
- `address` to show address, shows it in italics and also adds a line break before and after 
- `cite` just shows stuff in italic, used to define title of a creative work 
- `bdo` for bidirectional override ` <bdo dir="rtl">This text will be written from right to left</bdo>`

## comments and CSS and links

- `<!-- Write your comments here -->`
- inline css by style attribute, internal css by style tag, external using link tag `<link rel="stylesheet" href="https://www.w3schools.com/html/styles.css">`
- target attribute in links can have four values , `_self` open in same window, `_blank` open in new window or tab, `_parent` open document in parent frame, `_top` opens document in full body of window [detailed link](https://stackoverflow.com/questions/18470097/difference-between-self-top-and-parent-in-the-anchor-tag-target-attribute)
- to link to email address ` <a href="mailto:someone@example.com">Send email</a>`

## link colors 

```
a:link{ //some css}
a:visited{//some css}
a:hover{//some css}
a:active{//some css}
```

- `a` vs `a:link` a applies to all `a` tags while `a:link` applies to all links which means all `a` tags with href attribute  
- [css class preference](https://stackoverflow.com/questions/12258596/class-overrule-when-two-classes-assigned-to-one-div)

## Bookmarks and images

- use id attribute to create a bookmark `<h2 id="C4">Chapter 4</h2>`
- link to it `..href="#C4"..` if in same page or `...somePage.html#C4....` 
- the width and height attributes of an img tag always define values in pixels
- if u don't specify the width and height the image will still load but the webpage may flicker
- use CSS float property to float the image to left or right of a text 

## image maps
- the tag `map` defines an image map. an image map is an image with different clickable areas defined with area tags  
 
```
 <img src="workplace.jpg" alt="Workplace" usemap="#workmap">

<map name="workmap">
  <area shape="rect" coords="34,44,270,350" alt="Computer" href="computer.htm">
  <area shape="rect" coords="290,172,333,250" alt="Phone" href="phone.htm">
  <area shape="circle" coords="337,300,44" alt="Coffee" href="coffee.htm">
</map> 
```

- remember to use `usemap` attribute on the image 
- the area tag has four values for shape attribute `rect`, `circle`, `poly`, `default`
- rect requires xy coords of top left and bottom right 
- circle requires xy coords of center and the radius (it is in pixels obviously)
- poly requires sy coords of all points you want to make polygon out of 
- we can also attach an onClick attribute to area tags 

## background image

- `background-repeat:no-repeat` css property
- to cover the entire element without change in proportions `background-size:cover;background-attachment:fixed;`
- The `background-attachment` property sets whether a background image scrolls with the rest of the page, or is fixed.
- to stretch the image (change in original proportions) to fill the entire element `background-size: 100% 100%`

## picture 

- the picture tag allows to view different images for different resolutions 

```
 <picture>
  <source media="(min-width: 650px)" srcset="img_food.jpg">
  <source media="(min-width: 465px)" srcset="img_car.jpg">
  <img src="img_girl.jpg">
</picture> 
```

- the img tag acts as the default for browsers that do not support picture element or no size fit
- for multiple matching types it will choose the first element 

## tables 

- table, tr, th for the header columns, td for the data
- th has bold and text-align:center as css by default you could change it like `th{ font-weight:normal}`
- to collapse borders in table when you set `border: 1px solid black;` to th, td, table use `border-collapse: collapse;`
- rounded corners : `border-radius: 10px`

#### note 
Using a percentage as the size unit for a width means how wide will this element be compared to its parent element

- if you change the size of any th or td then for that column max size will be used (let's say if you change one td style to `width: 70%` then all the column elements will be of that size)
- to style the row style the row

## table headers 
- to span the header over more than one  column use the colspan property `<th colspan="2"> ...</th>`
- arbitrary colspan will just create empty data cells where there is nothing to fill
- we can add a caption which will just appear above the table `<table> <caption> ... </caption> <tr> .. </tr> </table>`
- the caption can be anywhere in the table between tr also, it just should be inside of the table no matter where (although if you put it between td or th elements the column will be split and the remaining row elements will be displayed in next line)
- to get vertical header just use th in first column
- to change space between cells use border-spacing `table { border-spacing:30px;}`

## colspan and rowspan 

- rowspan works just like colspan but obviously for rows 
- to style every even child `tr:nth-child(even) { ... }` this will style even numbered rows (1 indexed)
- for vertical stripes style td the same way above
- to specify only one side border `border-bottom: 1px solid black;`

## colgroup 

- to specify styles for a particular group of columns 

```
<table>
  <colgroup>
    <col span="2" style="background-color: #D6EEEE">
  </colgroup>
  <tr>
    <th>MON</th>
    <th>TUE</th>
    <th>WED</th>
    <th>THU</th>
... 
```

- here we specify in the col's span attribute that for 2 columns we want that style 
- if we add another col element inside that colgroup then those respective styles will be applied on the afterwards columns according to the span
- and if we add another colgroup tag, then those styles will start applying from columns which were yet not styles by previous colgroups 
- there are only few properties that can be used in colgroup, all other colgroup properties will have no effect `width`, `visibility`, 'background`, `border` 
- hide columns by setting `visibility:collapse` in the respective colgroup 

## lists
 
- ul, ol, li
- you can also have description list using dl tag where dt will be the term and dd will be the description 
- change the list style using css property `list-style-type: disc` options are `disc` , `circle`, `square`, `none`
- to change ul into navbar style use `li {float:left}` and to remove underline from links `a{text-decoration:none}`
- for ol to change the type use type  attribute which takes five values `1` (numbers) `A` (uppercase alphabet) `a` (smallcase alphabet) `I` (uppercase roman) `i` (lowercase roman) 
- for A to Z if it runs out it behaves just like excel AA AB ...
- to change the starting count use start attribute `start="50"` this start only accepts a number and won't work with anything else, but no matter the type used it will work as long as start is in number

## block and inline 

- block elements always begin on a new line (well unless it is floated) takes up as much width as possible and has a top and bottom margin 
- An inline element cannot contain a block-level element!
- to grab elements by class name in js `document.getElementsByClassName('someClassName')`
- for the bookmarks using id attribute obviously it can be applied to any element 

## iframe

- `<iframe src="url" title="someTitle"> ... </iframe>` the title is not gonna be displayed as a tooltip but it is beneficial for screen readers 
- the width and height styling can be done in the same way as img tag (width height attributes other than styling through css)
- to link to an iframe use the name of the iframe as the target of the link `<a target="iframeName"> ..` while iframe has `<iframe name="iframeName"> ..` in thise case first the iframe will display whatever its src has but when we click on the link then whatever href of the link points to will be shown in iframe

## javascript 

- through js we can change innerHTML, style.background-color, or any style attribute and any attribute 
- the `noscript` tag displays to people who have js disabled or it is not supported

## head 

- title tag is the title of the page 
- link is used to define relationship between current document and external resource, the rel attribute probs stands for relationship 
- meta tag for metadata

```
<meta name="keywords" content="HTML, CSS, JavaScript"> // and other name content pairs 
//like description author
<meta charset="UTF-8">

<meta http-equiv="refresh" content="30"> //refresh document every 30sec

<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

- the base tag specifies the base url and any relative url will be opened relative to this base url, it can also specify target value which will be used for all links in the document unless explicitly specified 
- the base tag must either contain the `href` or the `target` attribute or both

## layout 

- semantic elements, like, section defines a section in document, article for article, aside for content that is aside like sidebar 
- details tag for details which can be viewed or collapsed   

```
<details>
  <summary>Epcot Center</summary>
  <p>Epcot is a theme park at Walt Disney World Resort featuring exciting attractions, international pavilions, award-winning fireworks and seasonal special events.</p>
</details>
```

- here summary tag tells the summary which should be visible and on clicking the > like thingy we will see the entire details 

## responsive web design and computer code

- max-width:100% makes sure image scales down when browser size goes down but doesn't go beyond its original size 
- responsive text size : using `vw` unit the size will vary on the `viewport width` 1vw = 1% of viewport width 
- media queries can also be used to display different styles based on browser size and stuff `@media screen and (max-width: 800px)`
- code with code tag, kbd tag for keyboard input display (simply displays stuff with monospace input)
- samp to define sample output 
- code tag does not preserve extra whitespaces and line breaks to fix it use it inside pre tag
- var for variables 
- header tag (not the head tag) represents a container for introductory content and set of navigation link 
- footer , nav

## figure and entitites

- figure element to show self-contained content like diagrams and shit, use img inside it and a figcaption tag to appear as label below the image 
- entitties are used to display stuff that normally can't be displayed such as < which is displayed using `&lt;` or `&#60;`
- others include `&nbsp;` `&euro;`
- emojis are characters in unicode and can be displayed with their number like `&#128516`

## url encode 

- scheme://prefix.domain:port/path/filename
- url encoding: all characters are converted into ASCII so space becomes `%20` or `+`

## XHTML and forms

- Extensible HTML (a stricter version of html)
- the for of the label should be equal to the `id` (notice not the name) of the input element 
- for radio and checkboxes clicking on label also toggles the radio/checkbox
- form attributes, `action` defines what action, `target` where to show the response after submitting 
- `method` the method to use while submitting form data, using `method="get"` will result in an url which will contain all the data 
- `autocomplete="on"` will turn autocomplete in the form and the browser will try to fill in the values base on what values you put in previously
- novalidate is a boolean attribute (you don't provide value to it, just use it like `disabled`) if it is there it means that form input shouldn't be validated 

```
 <label for="cars">Choose a car:</label>
<select id="cars" name="cars" size="3">
  <option value="volvo">Volvo</option>
  <option value="saab">Saab</option>
  <option value="fiat">Fiat</option>
  <option value="audi">Audi</option>
</select> 
```

- the size attr tells how many options to be visible at once, use `multiple` attribute (boolean attr) to allow user to select more than one element 
- textarea with rows and cols attribute 
- fieldset tag used to group related form fields in one group and legend tag to tell what it is    

```
 <form action="/action_page.php">
  <fieldset>
    <legend>Personalia:</legend>
    <label for="fname">First name:</label><br>
    <input type="text" id="fname" name="fname" value="John"><br>
    <label for="lname">Last name:</label><br>
    <input type="text" id="lname" name="lname" value="Doe"><br><br>
    <input type="submit" value="Submit">
  </fieldset>
</form> 
```

- it will draw  a border around fields 
- to get a predefined list of options for an input element use datalist   

```
 <form action="/action_page.php">
  <input list="browsers">
  <datalist id="browsers">
    <option value="Internet Explorer">
    <option value="Firefox">
    <option value="Chrome">
    <option value="Opera">
    <option value="Safari">
  </datalist>
</form> 
```

- output element shows the result from a calculation 


## input types 

- input type reset resets form values to whatever default they had
- input type color for color, input type date (we can also add min and max attribute on it to restrcit date range),
- input type file for file upload 
- restrictions: checked, disabled, max, min, maxlength, pattern (specify regex to be matched against), readonly, size (width of input field), step (intervals for an input field, for example input type number with step 10 value 0 will go 0 to 10 to 20 ..) 
- input type range, input type search a simple input field, input type tel for telephone numbers, input type time, input type url 
- with step specified you can't enter values outside step allows
- input type image makes it a submit button and it can accept width and height attribute

## input attributes and input form attributes

- autofocus is a boolean attribute which focuses on that input when the page loads
- if input is outside the form the `form="formId"` will tell what form this input is part of (it won't rearrange the structure but still will be part of the form)
- to change what url the form gets processed as use formaction="someOtherPage" on the input (possibly submit type input)
- in a similar way formmethod attr overrides the method attr  

```
 <form action="/action_page.php" method="get">
  <label for="fname">First name:</label>
  <input type="text" id="fname" name="fname"><br><br>
  <label for="lname">Last name:</label>
  <input type="text" id="lname" name="lname"><br><br>
  <input type="submit" value="Submit using GET">
  <input type="submit" formmethod="post" value="Submit using POST">
</form> 
```

- similarly formtarget attr , and formnovalidate 


#### meter tag ? audio ? video ? 
