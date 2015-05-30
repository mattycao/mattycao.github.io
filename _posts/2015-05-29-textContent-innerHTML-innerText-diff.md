---
layout:     post
title:      Difference between textContent, innerHTML, innerText
date:       2015-05-29 7:50:00
summary:    The details about the three properties
categories: Javascript
---

## Difference between textContent, innerHTML, innerText

The `Node.textContent` property represents the text content of a node and its descendants.

```javascript
var text = element.textContent;
element.textContent = "this is some sample text";
```
### textContent Description:
`textContent` returns null if the element is a document, a document type, or a notation. To grab all of the text and CDATA data for the whole document, one could use document.documentElement.textContent.

If the node is a CDATA section, a comment, a processing instruction, or a text node, `textContent` returns the text inside this `node` (the `nodeValue`).
For other node types, `textContent` returns the concatenation of the `textContent` attribute value of every child node, excluding comments and processing instruction nodes. This is an empty string if the node has no children.

**Notice**:
Setting this property on a node removes all of its children and replaces them with a single text node with the given value.

###Differences from innerText:
Internet Explorer introduced `element.innerText`. The intention is similar but with the following differences:
* While textContent gets the content of all elements, including `<script>` and `<style>` elements, the IE-specific property innerText does not.
* `innerText` is aware of style and will not return the text of hidden elements, whereas `textContent` will.
* As `innerText` is aware of CSS styling, it will trigger a reflow, whereas `textContent` will not, which makes the `textContent` more fast.

### Differences from innerHTML:
`innerHTML` returns the HTML as its name indicates. Quite often, in order to retrieve or write text within an element, people use `innerHTML`. `textContent` should be used instead. Because the text is not parsed as HTML, it's likely to have better performance. Moreover, this avoids an `XSS` attack vector.
**Notice**:
The textContent was introduced after IE 8(exclusive), 