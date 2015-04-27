---
layout:     post
title:      MDN CSS Summary
date:       2015-04-26 21:29:00
summary:    Basic part about CSS!
categories: CSS
---

[MDN CSS](https://developer.mozilla.org/en-US/docs/Web/Guide/CSS/Getting_started "MDN Getting started with CSS")
---
## 1. What is CSS?
Cascading Style Sheet is a language for specifying how documents are presented to users.

Here the Document means a collection of information that is structured using a markup language.

## 2. Why using CSS?
For style, html is just the content. But sometimes html has style also, like strong will bolder the content inside the tag.

## 3. How css works?
  1. The browser converts the markup language and the css into the DOM. The DOM represents the document in the computer's memory. It combines the content with the style.
  2. The browser displays the contents of the DOM.

## 4. Cascading and inheritance
Has 3 Resources form a cascading:

  1. in an external files
  2. in a definition at the beginning of the document
  3. on a specific element in the body of the element

**In the read html:**

  1. Part of style comes from your browser default style.
  2. Part of style comes from customized browser setting or cutomized style definition file.
  3. Part of style comes from stylesheets linked to the document.

## 5. Selector
1. tag selector
2. class selector
3. id selector
4. pseudo-class selector
  * `:link`
  * `:visited`
  * `:active`
  * `:hover` `link — :visited — :hover — :active`
  * `:focus`
  * `:first-child`
  * `:nth-child`
  * `:nth-last-child`
  * `:nth-of-type`
  * `:first-of-type`
  * `last-of-type`
  * `:empty`
  * `:target`
  * `:checked`
  * `:enabled`
  * `:disabled`
    
  ```css
    selector:pseudo-class {
        property: value;
    }
  ```
5. selectors based on relationships

selector | selects
---|---
A E | Any E element that is a descendant of an A element (that is: a child, or a child of a child, etc.)
A > E | Any E element that is a child (i.e. direct descendant) of an A element
E:first-child | Any E element that is the first child of its parent
B + E | Any E element that is the next sibling of a B element (that is: the next child of the same parent)

## 6. Readable CSS
  1. white space
  2. comments /\*   \*/
  3. Group the selects together

## 7. Text Styles
  There is a convenient shorthand property, font, which you can use to sepcify serveral aspects at once.
  
 * bold, italic, and small-caps
 * the size
 * the line height
 * the font typeface


 ```css
    p {font: italic 75%/125% "Comic Sans MS", cursive;}
 ```

  1. if you want specify the font faces, we can use the `font-family` property.
    some built in default typefaces: `serif`, `sans-serif`, `cursive`, `fantasy` or `monospace`.
  2. about font size:
    * You can use some built-in values: `small`, `medium`, `large`
    * You can use values relatvie to the font size of the parent element like: `150%`, `1.5em`. em is equivalent to the width of the letter `m` in the font size of it parent.
    * You can use the actual size like: `14px` for the display device, or `14pt` for a printer.
  3. `line-height`:
    specify the spacing between lines.
  4. Decoration:
    the separate `text-decoration` property can sepcify some other styles, like: `underline` or `line-through`.
  5. others:
    - `font-style: italic`
    - `font-weight: bold`
    - `font-variant: small-caps`
    if we want turn off these, we can set it value as `normal` or `inherit`.
  6. **Notice:**
    In a complex stylesheet, avoid using the shorthand font property, because of its side-effects(resetting other individual properties).
    `@font-face` to sepcify the online font.

## 8. Color
  1. using a number sign(hash) and three hexadecimal digits like `#000`.
  2. for the full palette, specify two hexadecimal digits for each component like: `#ff0000`.
  3. using like this: `rgb(1,1,1)`, the digital is from 0 to 255.

**CSS properties**:
 * `color`: a propety on text
 * `background-color`: it can be set as `transparent` to explicitly remove the color, and revealing the parent element's background. It is the default value.

### 10. Content
  One of the important advantages of CSS is that it helps you to separate a document's style from its content. yet there are situations where it makes sense to specify certain content as part of the stylesheet, not as part of the document.

#### 1) Text Content
Sse the rule of :before or :after to the selector. Then in the content attribute, specify the value.
  
  ```css
  .ref:before {
    font-weight: bold;
    content: "Reference:";
  }
  ```
#### 2) Image Content
To add an iamge before or after an element, we can sepcify the URL of an image file in the value of the content property.
  
  ```css
  a:after {
    content: url("../image/1.png");
  }
  body {
    background: #00FF00 url(bgimage.gif) no-repeat fixed top;
  }
  ```

### 11. Lists
Some special properties thtat are designed for lists.

  * `list-style`: the type of marker

***Notice***:
The selector in your CSS rule can either select the list item elements(`<li>`), or it can select the parent list element(like `<ul>`) so that the list elements inherit the style.

#### 1) Unordered lists list-style
  * `disc`
  * `circle`
  * `square`

#### 2) Ordered lists list-style
  * `decimal`
  * `lower-roman`
  * `upper-roman`
  * `lower-latin`
  * `upper-latin`

#### 3) counters
Some browser do not support counters.

  * First, we need a *counter* with a name that you specify.
  * Then, in some element before the counting is to start, reset the counter with the property `counter-reset` and the name of your counter. The parent of the elments you are counting is a good place to do this, but you can use any element that comes before the list items.
  * Then, In each element where the counter increments, use the property `counter-increment` and the name of your counter.
  * To display your counter, add `:before` or `:after` to the selector and use the `content` property.
  * In the value of `content` property, specify `counter()` with the name of your counter.

  ```css
  h3.numbered {
    counter-reset: mynum;
  }
  p.numbered:before {
    /* Notice the string concat */
    content:counter(mynum) ":";
    conter-increment: mynum;
    /* The benefit of using the counter is that we can specify the style of the marker */
    font-weight: bold;
  }
  ```
  ***Notice***: The of counter is more powerful than you think, it not only increase by number, it can also increase by latin character. (`content:"(" counter(headnum, upper-latin) ") "`).

### 12. Boxes
  Four parts in the Boxs:
  
  * element
  * padding
  * border
  * margin

#### 1) The coloring:
  The padding is always the same color as the element's background. So when you set the background color, you see the color applied to the element itself and its padding. The margin is always transparent.

#### 2) Borders
  To specify the same border all around an element, use the `border` property. Specify the width, the style and the color.

  The style of the border has:
  
  * `solid`
  * `dotted`
  * `dashed`
  * `double`
  * `inset`
  * `outset`
  * `ridge`
  * `groove`

The attribute of every border:

 * `border-top`
 * `border-right`
 * `border-bottom`
 * `border-left`

  ```css
  img.class {
    border: 2px solid #ccc;
  }
  ```
### 13 Layout
  * Size units: it is better to specify sizes as a percentage or in ems.

#### 1) Text layout
  * `text-align`:
    aligns the content. Use one of these values: left, right, center, justify
  * `text-indent`:
    indents the content by an amount that you sepcify

  * `floats`
  * `clear`

#### 2) Position
  * `relative`
  * `fixed`
  * `absolute`
  * `static`

### 14 Tables
  * Borders: cells have no margins
    But cells do have borders and padding. By default, the borders are separated by the value of the table's `border-spacing` property. You can also remove the spacing completely by setting the table's `border-collapse` property to collapse.
  * captions
    A `<caption>` element is a label that applies to the entire table. By default, it is displayed at the top of the table.
    To move to the bottom, set its `caption-side` property to bottom. The property is inherited, so alternatively you can set it on the table or another ancestor element.
  * empty cells
  show the empty cell by specify the `empty-cells:hide` property.

### 15 Media
  * @media print(following by the media type)

  ```css
  @media print {
     #nav-area {
      display: none;
     }
  }
  ```
Types of media:

   * screen
   * print
   * projection
   * all

#### 1) Printing

  * `@page` rule can set the page margins.
  * `page-break-before`, `page-break-after`, `page-break-inside`


  ```css
  @import url("fineprint.css") print, tv;

  @page {margin: 1in;}
  h1 {page-break-before: always }
  ```

#### 2) User Interfaces
CSS has some special properties for devices that support a user interface, like a computer displays. These make the document's apprearance change dynamically as the user works with the interface.

  * `E:hover`
  * `E:focus`
  * `E:active`
  * `E"link`
  * `E:visited`

The cursor property specify the shape of pointers.

  * `pointer`
  * `wait`
  * `progress`
  * `default`

An `outline` property creates an outline that is often used to indicate keyboard focus.