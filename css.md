# CSS

- we can't use negative values in padding
- [negative z index](https://stackoverflow.com/questions/33217407/css-negative-z-index-what-does-it-mean)
-  `*` is the universal selector 
-  css comments are like `/* */`
-  hsl hue saturation lightness hue(0-255) saturation and lightness are percentage and an alpha sometimes 0-1
-opacity 0-1

## background

- background-color, background-image, background-repeat : `repeat-x` or `repeat-y` or `no-repeat`, `background-position : right top`, background-attachment 
- to use shorthand background , the order is `background-color` > `background-image` > `background-repeat` > `background-attachment` > `background-position`

## borders 

- border-style : dotted, dashed, solid, double, groove, ridge, inset, outset, none, hidden
- border-style can have four values (top right bottom left order) `border-style: dotted dashed solid double`
- border-width, border-color
- instead of the multi value style, you can also specify individually for every side like `border-top-style : dotted`
- border shorthand, order is `border width` > `border style` > `border color` (actually order does not matter just this order is more common)
- we can also specify individual like border-left as shorthand

## margin

- negative values are allowed 
- auto (browser calculates the margin) length (in px vt cm) % or `inherit` (inherit from parent)
- margin shorthand as usual top to left clockwise 
- margin auto will center the element 
- margin collapse : The top and bottom margins of blocks are sometimes combined (collapsed) into a single margin whose size is the largest of the individual margins. Note that the margins of floating and absolutely positioned elements never collapse.

## padding

- negative values are not allowed 
- the width property sets the width of the content excluding padding 
- if you want width to be the width of box even with padding use `box-sizing: border-box` the content width will decrease accordingly 

## height and width 

- height/width : initial, set it to default 
- If you for some reason use both the width property and the max-width property on the same element, and the value of the width property is larger than the max-width property; the max-width property will be used (and the width property will be ignored

## Outline 

- An outline is a line that is drawn around elements, OUTSIDE the borders
- Outline differs from borders! Unlike border, the outline is drawn outside the element's border, and may overlap other content. Also, the outline is NOT a part of the element's dimensions; the element's total width and height is not affected by the width of the outline.
- outline-style same values as border-style , and all other properties 
- outline shorthand also same and order also does not matter
- `outline-offset:10px` sets offset between outline and border

## text 

- color, text-align : left | right | center | justify (in justify Text is spaced to line up its left and right edges to the left and right edges of the line box, except for the last line.)
- to change the direction of text 

```
direction: rtl;
unicode-bidi: bidi-override;
```

- the browser attempts to define how text should appear however to override it and make sure the text is reversed according to direction property we use `unicode-bidi : bidi-override` which works on block level elements
- for lnline element we could use `unicode-bidi: embed` which will allow embedding of rtl text in ltr and vice versa 
- vertical-align vertically aligns an inline, inline-block or table-cell box. vertical-align : baseline | text-top | text-bottom | sub | super 
- text-decoration : overline | underline | none | line-through

### text transform 

- text-transform : uppercase | lowercase | capitalize 
- text-indent : 50px
- letter-spacing : 5px (can have negative values completely squashing letter together)
- line-height : 0.8
- word-spacing : 10px
- white-space (tellls whether white-space is collapsed and if then how it wraps) : nowrap | normal 

### text shadow
- text-shadow : It accepts a comma-separated list of shadows to be applied to the text and any of its decorations.

```
/* offset-x | offset-y | blur-radius | color */
text-shadow: 1px 1px 2px black;
/* offset-x | offset-y | color */
text-shadow: 5px 5px #558abb;
/* color | offset-x | offset-y | blur-radius */
text-shadow: #fc0 1px 0 10px;
```

- When more than one shadow is given, shadows are applied front-to-back, with the first-specified shadow on top.

## Fonts 

### font family 

- `font-family: "Times New Roman", Times, serif;`
- font-style to specify italic text (or oblique) `font-style:italic`
- `font-weight: normal | bold | light
- font-variant : normal | small-caps (in small-caps small letters will look like capitals and only a little smaller than the capitals)
- font-size : 1px | 1em (1 em is current font size about 16px) | 1vw (viewport width 1 %)
- font shorthand requires font-size and font-family, rest have default values 
- font-size and line-height is specified in shorthand like `12px/30px` 12px is font-size and line-height is 30px

## links 

- a:hover MUST come after a:link and a:visited
- a:active MUST come after a:hover

## lists 

- to have an image as the list marker `list-style-image : url('something.gif');`
- list-style-position : outside | inside (with inside the points will be a part of the list content.  As it is part of the list item, it will be part of the text and push the text at the start:) 
- list-style shorthand with the order list-style-type > list-style-position > list-style-image 
- if any property missing default value will be used 

## table 
- vertical-align : top | bottom | middle (aligns the content of the th td elements middle is default) 
- to add horizontai scroll bar on table when it is too big use this on any container containing table `<div style="overflow-x:auto;">`(with overflow-x: scroll the scrollbars will be there whether the element is overflowing or not) 

## max-width 

- [max-width and width](https://blog.prototypr.io/what-even-is-the-difference-between-width-and-max-width-8f37b282c7f1) (basically if width > max-width; let the browser use max-width. if width < max-width; let the browser use width.)

## position 

- static | relative | fixed | absolute | sticky 
- static = default value, just place as the normal flow of the page 
- relative, element is postioned relative to **it's** normal position 
- it can be modified by the top right bottom left properties 
- fixed, it just stays at some part in the viewport and doesn't move even on scrolling 
- absolute , positioned **relative** to the nearest positioned ancestor (moves along with scrolling)
- sticky : behaves like a **relative** until user scrolls upto the point it just sticks (fixed positioning)
- z-index can be negative 
- if z-index same then the one that came last in html will be above 

## overflow 

- overflow: visible | hidden | scroll | auto 
- overflow-x, overflow-y

## float and clear 

- The float CSS property places an element on the left or right side of its container, allowing text and inline elements to wrap around it. The element is removed from the normal flow of the page, though still remaining a part of the flow (in contrast to absolute positioning).
- float : left | right | none 
- When we use the float property, and we want the next element below (not on right or left), we will have to use the clear property.
- clear : none | left(pushed below left floating elements) | right | both | inherit 

### clearfix 

- to make sure a div will fully enclose all its floating children and none will overflow 

```
.whateverClassName:after {
  content: "";
  display: table;
  clear: both;
}
```

## inline-block 

- Compared to display: inline, the major difference is that display: inline-block allows to set a width and height on the element.
- Also, with display: inline-block, the top and bottom margins/paddings are respected, but with display: inline they are not.
- Compared to display: block, the major difference is that display: inline-block does not add a line-break after the element, so the element can sit next to other elements.

## align 

- to center align an element `margin: auto` (doesn't work for vertical align because margin calculated with auto will be 0)
- centering using transform 

```
position : absolute;
top : 50% ;
left : 50%;
transform : translate(-50%, -50%)
```

## combinators 

- descendant selector (space) 
- child selector (>) 
- adjacent sibling selector (+) (immediately **After**)
- general sibling selector (~) (all the **next** sibling)

## pseudo class 

- first-child 

```
p:first-child {
  color: blue;
}
```

- it will match any **p** element that is the first child of any element 
- :lang allows to define css for lang attribute elements 

```
q:lang(no) {
  quotes: "~" "~";
}
...
<p>Some text <q lang="no">A quote in a paragraph</q> Some text.</p>
```

- p:last-child = select p which is the last child of any element 
- p:last-of-type = select p which is last p among child of any element 
- :active, :checked, :disabled, :enabled, :focus, :hover,  :invalid, :not(selector), :nth-child(n), :nth-last-child(n)
- :in-range (for input elements this acts when value for that input is within range as specified for that input)
- :empty = element with no children 
- :nth-of-type(n), :nth-last-of-type(n), :optional , :out-of-range, :read-only, :read-write (element with no readonly)
- :only-child (only child of its parent) 
- :only-of-type(n) = element that is the only child of that type 
- :required, :root, :valid, :visited
- :target selects an element whose id matches a part of url 

## pseudo-element 

- ::first-line (only applicable to block elements)
- ::first-letter (only applicable to block elements)
- ::before to insert some content before the content of the element 

```
h1::before {
  content: url(smiley.gif);
}
```

- ::after
- ::marker selects the markers of list items
- ::selection = matches the selection user selects (of text and stuff) , properties available  = color, background, cursor, outline

#### opacity css property which takes values from 0 to 1 

## sprites 

- src attribute can not be empty 
- `background: url(img_navsprites.gif) 0 0;` the two attributes define the x and y that the image is at 

## attribute selectors 

- The [attribute] selector is used to select elements with a specified attribute.
- `a[target]` , `a[target="_blank"]`
- `[title~="flower"]` the title attribute contains flower in its values (among all the multiple values )
- [attribute|="value"] = attribute starts with value either as a whole word or with hypen like "top fuck" or "top-fuck"
- [attribute^="value"] = attribute starts with a word, no whole word required 
- [attribute$="value"] = attribute end with a value , no whole word required 
- [attribute*="value"] = attribute contains that value, no whole word required 


#### resize:none will prevent textareas from being resized 

## css counters 

- counter-reset - Creates or resets a counter
- counter-increment - Increments a counter value
- content - Inserts generated content
- counter() or counters() function - Adds the value of a counter to an element
- The counter's name must not be "none", "inherit", or "initial"; otherwise the declaration is ignored.

```
body {
  counter-reset: section;
}

h2::before {
  counter-increment: section;
  content: "Section " counter(section) ": ";
}
```

- The counter() function has two forms: 'counter(name)' or 'counter(name, style)'. The generated text is the value of the innermost counter of the given name in scope at the given pseudo-element.The counter is rendered in the specified style (decimal by default).
- The counters() function also has two forms: 'counters(name, string)' or 'counters(name, string, style)'. The generated text is the value of all counters with the given name in scope at the given pseudo-element, from outermost to innermost, separated by the specified string. The counters are rendered in the specified style (decimal by default).


## units 

- em : Relative to the font-size of the element (2em means 2 times the size of the current font)
- rem :  	Relative to font-size of the root element
- vmin : Relative to 1% of viewport's* smaller dimension
- vmax : Relative to 1% of viewport's* larger dimension

## specificity 

- Start at 0, add 1000 for style attribute, add 100 for each ID, add 10 for each attribute, class or pseudo-class, add 1 for each element name or pseudo-element.
- !important can only be overriden with another !important in which case the normal specificity battle ensues 

### border-image 

-border-image

## gradients 

- `background-image: linear-gradient(direction, color1, color2, ...);`
- if direction not mentioned top to bottom is default 
- background-image: linear-gradient(to right, red, yellow);
- background-image: linear-gradient(to bottom right, red, yellow);
- when using degree 0 is to top 90 is to right 
- background-image: linear-gradient(180deg, red, yellow); (this will go top to bottom)

- repeating-linear-gradient 

```
/* A repeating gradient tilted 45 degrees,
   starting blue and finishing red, repeating 3 times */
repeating-linear-gradient(45deg, blue, red 33.3%);

/* A repeating gradient going from the bottom right to the top left,
   starting blue and finishing red, repeating every 20px */
repeating-linear-gradient(to left top, blue, red 20px);

/* A gradient going from the bottom to top,
   starting blue, turning green after 40%,
   and finishing red. This gradient doesn't repeat because
   the last color stop defaults to 100% */
repeating-linear-gradient(0deg, blue, green 40%, red);

/* A gradient repeating five times, going from the left to right,
   starting red, turning green, and back to red */
repeating-linear-gradient(to right, red 0%, green 10%, red 20%);
```

- if no color stop given for first color it defaults to 0, it I change color stop of first color to something like 10% then the last color will come in those 10%
- [color stops](https://www.quirksmode.org/css/images/colorstops.html) basically color stops define at what percentage we want a color to be there 

### radial gradients

- background-image: radial-gradient(shape size at position, start-color, ..., last-color);
- By default, shape is ellipse, size is farthest-corner, and position is center.
- background-image: radial-gradient(red, yellow, green);
- background-image: radial-gradient(red 5%, yellow 15%, green 60%);
- background-image: radial-gradient(circle, red, yellow, green);
- size has four values : closest-side | farthest-side | closest-corner | farthest-corner
- background-image: radial-gradient(closest-side at 60% 55%, red, yellow, black);
- repeating-radial-gradient 
- background-image: repeating-radial-gradient(red, yellow 10%, green 15%);

## shadow 

-  box-shadow: 10px 10px 5px grey;

## text effects 

- text-overflow (how overflowed content should be displayed) : clip | ellipsis
- one **has** to set overflow property (to something other than default) for the text-overflow to work 

- word-wrap : break-word (break the word if it is too long and allow it to wrap around)
- The word-break property in CSS is used to specify how a word should be broken or split when reaching the end of a line. The word-wrap property is used to split/break long words and wrap them into the next line.
- word-break: break-all; It is used to break the words at any character to prevent overflow.
- word-wrap: break-word; It is used to break the words at arbitrary points to prevent overflow.
- word-wrap:break-word will wrap words on to the next line and wrap them so that words do not break in the middle the `word-break: break-all` doesn't care 

- writing-mode (whether text is horizontal or vertical) : horizontal-tb | vertical-rl | vertical-lr (here lr tb shows in which direction the next line will be)

## web fonts

- to use one's own fonts 

```
@font-face {
  font-family: myFirstFont;
  src: url(sansation_light.woff);
}
```

## 2d transforms 

- transform : translate | rotate | scaleX | scaleY | scale | skewX | skewY | matrix 
- translate moves stuff acc to x and y `transform: translate(50px, 100px);`
- transform: rotate(20deg); (positive is clockwise)
- transform: scale(2, 3); (scales the width to be twice and height to be thrice)
- scaleX for width, scaleY for height 
- skew(20deg, 30deg) skews an element 20 deg along x axis and 30 deg along y axis
- for x skewing for positive values it skews to the left
- matrix(scaleX(),skewY(),skewX(),scaleY(),translateX(),translateY())
- transform: matrix(1, -0.3, 0, 1, 0, 0);

## 3d transforms 

- transform : rotateX, rotateY, rotateZ

## transitions 

- To create a transition effect, you must specify two things: 1) the CSS property you want to add an effect to 2) the duration of the effect
- If the duration part is not specified, the transition will have no effect, because the default value is 0.

```
div {
  width: 100px;
  height: 100px;
  background: red;
  transition: width 2s;
}
```

- transition: width 2s, height 4s;
- transition-timing-function : ease | linear | ease-in | ease-out | ease-in-out - cubic-bezier(n,n,n,n) 
- ease-in will start the animation slowly, and finish at full speed. ease-out will start the animation at full speed, then finish slowly. ease-in-out will start slowly, be fastest at the middle of the animation, then finish slowly. ease is like ease-in-out , except it starts slightly faster than it ends.
- transition-delay : 1s;
-  `transition: width 2s, height 2s, transform 2s;`

```
/* specifying one by one */
  transition-property: width;
  transition-duration: 2s;
  transition-timing-function: linear;
  transition-delay: 1s;
```

- shorthand `transition: width 2s linear 1s;`

## animations 

```
 /* The animation code */
@keyframes example {
  from {background-color: red;}
  to {background-color: yellow;}
}

/* The element to apply the animation to */
div {
  width: 100px;
  height: 100px;
  background-color: red;
  animation-name: example;
  animation-duration: 4s;
} 
```

- default value of animation-duration is 0s

```
@keyframes example {
  0%   {background-color: red;}
  25%  {background-color: yellow;}
  50%  {background-color: blue;}
  100% {background-color: green;}
}
```

- animation-delay (how much delay before start of animation)
- Negative values are also allowed. If using negative values, the animation will start as if it had already been playing for N seconds.
- The animation-iteration-count property specifies the number of times an animation should run.
- you can also `animation-iteration-count:inifnite`
- animation-direction (what direction to play animation in) : normal | reverse | alternate (first forward then backwards) | alternate-reverse
- animation-timing-function same as transition-timing-function

### animation-fill-mode 

- CSS animations do not affect an element before the first keyframe is played or after the last keyframe is played. The animation-fill-mode property can override this behavior.
- The animation-fill-mode property specifies a style for the target element when the animation is not playing (before it starts, after it ends, or both).
- animation-fill-mode : none | forwards (The element will retain the style values that is set by the last keyframe)  | backwards (The element will get the style values that is set by the first keyframe, and retain this during the animation-delay period ) | both (The animation will follow the rules for both forwards and backwards, extending the animation properties in both directions)

### animation shorthand 

```
div {
  animation-name: example;
  animation-duration: 5s;
  animation-timing-function: linear;
  animation-delay: 2s;
  animation-iteration-count: infinite;
  animation-direction: alternate;
}
```

```
animation: example 5s linear 2s infinite alternate;
```

## variables 

```
 :root {
  --blue: #1e90ff;
  --white: #ffffff;
}
.container {
  color: var(--blue);
}
```

- local will take precedence over global 
- to get css variable value using js `getComputedStyle( document.querySelector(':whatevr') ).getPropertyValue('--blue')`

## flexbox 

- display : flex
- flex-direction : row | column | row-reverse | column-reverse
- flex-wrap : wrap | nowrap | wrap-reverse (the elements go left to right then wrap up going left to right again)
- flex-flow : row wrap;
- justify-content : center  | flex-start (default) | flex-end | space-around | space-between 
- align-items (kind of like justify-content but defines where the element goes if container height larger than content width) : flex-start | flex-end | stretch (default)  | baseline (by text baseline) 
- align-content (used to align the flex lines, basically among all the lines of flex formed after wrapping how they interact) : space-between | space-around | stretch (default) | center | flex-start | flex-end 

### flex items 

- the children in flexbox automatically become flex items and they have the following properties 

```
<div class="flex-container">
  <div style="order: 0">1</div>
  <div style="order: -1">4</div>//negative values allowed 
  <div style="order: -1">2</div>//same value then first defined nigga will come first 
  <div style="order: 1">3</div> 
</div>
```

- flex-grow, specifies with what magnitude does one flex item grows in comparison to others 
- if width is fixed using width or something, then it won't shrink beyond that but can grow

```
 <div class="flex-container">
  <div style="flex-grow: 1">1</div> //negative values allowed but only upto some extent after which width can't be reduced
  <div style="flex-grow: 1">2</div>
  <div style="flex-grow: 8">3</div>
</div> 
```

- flex-shrink (default 1)
- flex-basis : 200px (specifies initial length of a flex-item)
- The flex property is a shorthand property for the flex-grow, flex-shrink, and flex-basis properties.

- The align-self property specifies the alignment for the selected item inside the flexible container.The align-self property overrides the default alignment set by the container's align-items property.

