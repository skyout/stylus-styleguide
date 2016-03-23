Stylus Style Guide
============================

*A guide to developing robust stylesheets in Stylus*

Table of Contents
-----------------
    
1. [General Principles](#general)
2. [Formatting](#formatting)
    - Spacing and Indentation
    - Sorting Properties
    - Chaining
    - Nesting
    - Shorthand Properties
    - Shorthand Values
    - Quotation Marks
3. [Naming](#naming)
    - Case
    - IDs and Classes
    - Delimiters
4. [Imports and Charset](#imports)
5. [Mixins](#mixins)
6. [Fonts](#fonts)
7. [Variables](#variables)
    - Declaration
    - Default Values
    - Manipulation
8. [Comments](#comments)
9. [Thanks](#thanks)



<a name="general">General Principles</a>
-----------

Make sure there is plenty of space around all of your selectors and comments to maintain readability. Alignment is also key to keeping a file readable, so ensure that all selectors and properties are properly indented. Any files that you will be including should be prefixed with an underscore `_` so that they are easily found at the top of a folder.

**Be consistent.** If you’re editing code, take a few minutes to look at the code around you and determine its style. If they use spaces around all their arithmetic operators, you should too. If their comments have little boxes of hash marks around them, make your comments have little boxes of hash marks around them too.

The point of having style guidelines is to have a common vocabulary of coding so people can concentrate on what you’re saying rather than on how you’re saying it. If code you add to a file looks drastically different from the existing code around it, it throws readers out of their rhythm when they go to read it. Avoid this.



<a name="formatting">Formatting</a>
----------


### Spacing and Indentation

Indentation should be **four spaces**. If using tabs in a code editor, ensure that tabs are set to four spaces. Each selector should have at least one line break above, this helps to separate selectors and ideas. Properties should immediately follow the selector without a line break, which will aide in putting context to the properties. Remove any trailing whitespaces. Trailing whitespaces are unnecessary and can complicate diffs.


```sass
// bad
h1
  padding: 0
  margin: 0
h2
  color: #846383
  margin-bottom: 20px


// good
h1
    margin: 0
    padding: 0
    
h2
    color: #846383
    margin-bottom: 20px
```

### Sorting Properties

All properties should be on their own line. Properties should be ordered alphabetically to enable easy lookup. Any includes should go at the top of the propertly list, directly below the selector. Multiple includes should also be ordered alphabetically. 



```sass
// bad
h1
    padding: 5px
    clear()
    width: 200px
    color: #000


// good
h1
    clear()
    color: #000
    padding: 5px
    width: 200px
```

### Chaining

When possible, if multiple methods have the same styles or very similar styles then chain them together using commas. If two selectors have very similar styles with one or two differences, chain them together with the first selector's styles, then after set the minor changes to the second selector. 

```sass
// bad
ol
    color: #000
    line-height: 1.5
    list-style: none outside none
    padding: 0

ul
    color: #666
    line-height: 1.5
    list-style: none outside none
    padding: 0


// good
ol, ul
    color: #000
    line-height: 1.5
    list-style: none outside none
    padding: 0
    
ul
    color: #666
```

### Nesting

Element selector nesting should only be used when necessary. Direct descendents also should be used sparingly due to the performance hit that comes with their use. Nesting can cause unecessarily large file sizes when a lot of nesting occurs. 

Descendant selectors are inefficient because, for each element that matches the key, the browser must also traverse up the DOM tree, evaluating every ancestor element until it finds a match or reaches the root element. The less specific the key, the greater the number of nodes that need to be evaluated. 

Nesting should be four spaces, and any selectors should have a line break before them.


```sass
// bad
body
    border-top: 1px solid #000
    
    > article
        background: #ccc
        padding: 0
        
    h1
        color: #000


// good
body
    border-top: 1px solid #000
    
article
    background: #ccc
    padding: 0
    
h1
    color: #000
```
    
    
Classes, pseduo-classes, and other modifiers should nest inside their selector to maintain readability. The same rules with a line break above apply to these modifiers. Pseduo-class modifiers should go as close to their parent selector as possible. This allows developers to quick see any hover or other states without having to find them later in the file. Class modifiers should also go near their selector whenever possible.  


```sass
// bad
h1.green
    color: #0085e3
        
h1:first-child
    padding-top: 0


// good
h1

    &:first-child
        padding-top: 0

    &.green
        color: #0085e3
```

Global styles for elements and very common classes should be placed near the top of the global stylesheet. This can include things like body, link, input, paragraph, and header styles. 

```sass
@charset "UTF-8"

body
    border-top: 3px solid #333

a
    color: #008fc5
    cursor: pointer
    outline: none
    text-decoration: none

input
    border: 1px solid #ccc
    
h1
    font-size: $font-size + 10
    
p
    color: #666
    line-height: 1.5
    
        
// rest of the file
```
    



### Shorthand Properties


CSS offers a variety of shorthand properties (like `font`, `background`, `list-style`, etc) that should be used during the first declaration. 

**Font**

`font` should be declared on the body tag with the full single-line declaration as follows: 

```sass
font: font-style font-weight font-variant font-size/line-height font-family
// font: normal normal normal 14px/1.5 Arial, Helvetica, sans-serif
```

Any subsequent modifications to that font tag should be done in multi-line declaration, *unless* there is a change to the `font-family`, then the full declaration is required. This will keep a single font for a document and promotes proper cascading. 

`Font-weight` should always be declared using numbers instead of keywords. This aids in use of `@font-face` declarations. `Line-height` should always be declared using `em`, however without the unit. 

**Background**

`background` should be declared using the single-line declaration the first time.

```sass
background: color image repeat attachment position(left, top) size clip attachment
// background: #eee url("img/pic.jpg") no-repeat scroll left top auto padding-box border-box 
```

Like font, Any subsequent modifications to that font tag should be done in multi-line declaration, *unless* there is a change to the `url()`, then the full declaration is required. One exception to this is if there original declaration only sets a color then the single `background: color` shorthand is acceptable. 

```sass
background: #fff
```

**List Style**

Like the two preceding properties, `list-style` should be declared initially using the single-line declaration the first time. Any subsequent modifications to that font tag should be done in multi-line declaration, *unless* there is a change to the `url()`, then the full declaration is required.

```sass
list-style: list-style-type list-style-position list-style-image
// list-style: disc outside none
```


### Shorthand Values

Omit unit specification after `0` values unless they are required. For color values that permit it, 3 character hexadecimal notation is shorter and more succinct. As a general rule, if a unit specification isn't required, then don't use it.

```sass
// bad
p
    color: #000000
    padding: 0px
    line-height: 1.5em


// good
p
    color: #000
    padding: 0
    line-height: 1.5
```



Shorthand values should be used for `padding`, `margin`, and `border` if **more than one** value is going to be set.

```sass
// bad
p
    padding-left: 20px
    padding-top: 15px
    

// good
p
    padding: 15px 0 0 20px
    
```

For `padding`, `margin`, and `border`, 1 2 or 4 values are acceptable. Leaving off single values can cause ambiguous code and lead to difficult comprehending. 

```sass
// bad
p
    padding: 0 20px 10px


// good
p
    padding: 0
    
q
    padding: 0 20px

time
    padding: 10px 20px 15px 5px

```

Similarly when declaring `box-shadow` and `text-shadow` ambigiously leaving off values is bad form. Always declare all values of these properties. However, mixins are encourage for these due to vendor prefixes. 

```sass
box-shadow: inset 3px 3px 1px 0 rgba(0,0,0,0.3)
// box-shadow: none h-shadow v-shadow blur spread color 

text-shadow: 1px 1px 1px rgba(0,0,0,0.5)
//text-shadow: h-shadow v-shadow blur color
```


### Quotation marks

Use double `"` rather than single `'` quotation marks for attribute selectors or property values. Also don’t forget to quote attribute values in selectors. Exceptions can be made for values that require use of quotes inside the property value, such as the `content` or `quote`. 

```sass
// bad
html
    background: #fff url('img/bg.png') repeat 0 0
    font-family: 'open sans', arial, sans-serif

input[type=search]
    margin-left: 1em


// good
html
  background: #fff url("img/bg.png") repeat 0 0
  font-family: "open sans", arial, sans-serif

input[type="search"]
  margin-left: 1em
```






<a name="naming">Naming</a>
------


### Case

**Use only lowercase**. This applies to selectors, properties, and property values. Exceptions are for any strings inside css property values, such as `content`. 

```sass
// bad
UL
    color: #EEEEEE
    line-height: 1.5
    
    &:FIRST-CHILD
        padding-top: 20px


// good
ul
    color: #000
    line-height: 1.5
    
    &:first-child
        padding-top: 20px
```




### IDs and Classes

Use meaningful or generic ID and class names. Instead of presentational or cryptic names, always use ID and class names that reflect the purpose of the element in question, or that are otherwise generic. Names that are specific and reflect the purpose of the element should be preferred as these are most understandable and the least likely to change. Generic names are simply a fallback for elements that have no particular or no meaning different from their siblings. They are typically needed as “helpers”. Using functional or generic names reduces the probability of unnecessary document or template changes.


```sass
// bad

// meaningless
#yee-1901

// presentational
.button-green, .clear


// good

// specific
.gallery, .login, .video 

// generic
.aux, .alt
```

Use ID and class names that are as short as possible but as long as necessary. Try to convey what an ID or class is about while being as brief as possible. Using ID and class names this way contributes to acceptable levels of understandability and code efficiency. Unless necessary, do not use element names in conjunction with IDs.


```sass
// bad

//too long
#navigation

// not descriptive enough
.atr


// good

#nav, .author
```

### Delimiters

Separate words in ID and class names by a hyphen. Do not concatenate words and abbreviations in selectors by any characters (including none at all) other than hyphens, in order to improve understanding and scannability.


```sass
// bad

// not readble
#videoid

// camel instead of hypen
.errorMessage


// good

#video-id, .error-message
```

<a name="imports">Imports and Charset</a>
------------------

Stylus imports and charset declarations should always be at the top of each Stylus file. Important are used to include variables, mixins, fonts, and other chunks of code that are reusable. 

* `@charset` declaration should always be the first thing. The default charset to be used is UTF-8.
* Variables, fonts, and then mixin declarations should then immediately follow. 
* If the file needs a reset or normalize, that should then follow next. 
* Comments should also be used to describe any other mixins that are not the standard `_variables`, `_fonts`, `_mixins`, or `_reset`
* File extenions should only be used if the extension is not the default `.styl`

```sass
// bad

// imports
@import "_mixins.styl"
@import "_fonts.styl"
@import "_variables.styl"
@import "_reset.styl"
@import "_tips.styl"

@charset "UTF-8"


// good
@charset "UTF-8"

@import "_variables"
@import "_fonts"
@import "_mixins"
@import "_reset"

// global tooltip styles
@import "_tips"
```





<a name="mixins">Mixins</a>
------

Mixins are declared using the `function` type declaration. Mixins should be housed in a file called `_mixins.styl`. Mixins are used for code that is frequently repeated through the stylesheets, but may not necessarily correspond to a single element or class. 

* Mixin names should follow dashed, lowercase formatting.
* Mixins should be order alphabetically by their title for organization.
* If applicable, provide default settings in the mixin.


```sass
// bad
button(color)
    background: color
    display: block
    text-align: center


// good
button(color = blue)
    background: color
    display: block
    text-align: center
```

Mixins are encouraged for multiple vendor prefix properties such as box-shadow, transition, transform, gradient, etc. 

<a name="fonts">Fonts</a>
---

If outside font files are needed, then a separate font file should be created and named `_fonts.styl`. Multiple styles and weights of a font should be set using `@font-face`, but should all have the same font family name. Ensure when declaring the font to provide backup font styles.  

```sass
// proxima nova
@font-face
    font-family: 'proxima-nova'
    src: url('/fonts/ProximaNovaRegular/ProximaNova-Reg-webfont.eot')
    src: url('/fonts/ProximaNovaRegular/ProximaNova-Reg-webfont.eot?#iefix') format('embedded-opentype'), url('/fonts/ProximaNovaRegular/ProximaNova-Reg-webfont.svg#proxima_nova_rgregular') format('svg'), url('/fonts/ProximaNovaRegular/ProximaNova-Reg-webfont.woff') format('woff'), url('/fonts/ProximaNovaRegular/ProximaNova-Reg-webfont.ttf') format('truetype')
    font-weight: normal
    font-style: normal
    
font-family: 'proxima-nova'
    src: url('/fonts/ProximaNovaBold/ProximaNova-Bold-webfont.eot')
    src: url('/fonts/ProximaNovaBold/ProximaNova-Bold-webfont.eot?#iefix') format('embedded-opentype'), url('/fonts/ProximaNovaBold/ProximaNova-Bold-webfont.svg#proxima_nova_rgbold') format('svg'), url('/fonts/ProximaNovaBold/ProximaNova-Bold-webfont.woff') format('woff'), url('/fonts/ProximaNovaBold/ProximaNova-Bold-webfont.ttf') format('truetype')
    font-weight: 700
    font-style: normal
```


<a name="variables">Variables</a>
---------


### Declaration

Variables are declared using the `=` notation. Variables should be housed in a file called `_variables.styl`. Variables are used for common colors, fonts, and numbers used throughout the stylesheets. 

* Variables should follow dashed formatting
* Ensure that variables are not ambiguous and describe the value they hold. 
* Like variables should be grouped together to help with context and readability.
* Comment each variable or group to document the values. 
* Inheritance of colors should be used whenever possible

```sass
// bad

$red = #ea5b54
$green = #98fe98
$primary = #00853e
$secondary = #008fc5

// good

// error and success colors
$error-red = #ea5b54
$success-green = #98fe98

// brand colors
$brand-primary-color = #00853e
$brand-secondary-color = #008fc5
```

### Default Values

Each project should have default variables that are associated with it. These include the following: `font-size`, `font-family`, `line-height`. These values should be set to the body in the global stylesheet. 

```sass
body
    font: normal $font-size/$line-height "proxima-nova", sans-serif
```

### Manipulation

When manipulating a sizing variable in a stylesheet, use relative `*` modifiers instead of a fixed value. This allows for simple global font changes in the future, as well as simple font scaling for responsive designs. Any arithmetic operators should have a space before and after.  

```sass
// bad
p
    font-size: 16px
    line-height: 1.7


// good
p
    font-size: $font-size * 1.15
    line-height: $line-height * 1.2
```


<a name="comments">Comments</a>
--------
Well commented code is extremely important. Take time to describe components, how they work, their limitations, and the way they are constructed. Don't leave others in the team guessing as to the purpose of uncommon or non-obvious code.

Comment style should be simple and consistent within a single code base.

* Place comments on a new line directly above their subject.
* Comments should have a space directly after the `//`.
* Align comments with the selectors they pertain to.
* Use terse comments that convey ideas.
* Comment major code ideas.
* Comments on every selector is unnecessary.

```sass
// bad

// too far away

h1
    color: #000
    

/*
    Improper use
    of the multiline
*/

section
    padding: 0


// good

// basic comment
h1
    color: #000

// Block of comments that
// pertain to the section
// below and other things
// and maintain spacing
section
    padding: 0
```


<a name="thanks">Thanks</a>
======

Thanks to [CSS/Sass style guide](https://github.com/isellsoap/css-sass-style-guide) for an outline of ideas and veribage for expressing concepts. 
