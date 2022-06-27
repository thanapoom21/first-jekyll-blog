---
layout: default
title: Get, Set, Manipulate, and Traverse
parent: jQuery
nav_order: 2
description: "Getting, Setting, Manipulating, and Traversing in Action"
---

## Get, Set, Manipulate, and Traverse data in JavaScript using jQuery

```js
/**
  *
  This text() method will add string to the DOM.
  However, this method will not be able to compile html elements.
  *
  **/
$(".selectedElement").text("This is a new text");
```

```js
// This html() method will add strings of texts and html elements to the DOM.
$(".selectedElement").html(
  "This is a text with <strong>an html element</strong>"
);
```

```js
/**
  *
  We can use constant variable to store string value and use it later.
  Then we add newText variable to the selected element using append() method.
  *
  **/
const newText = "This is a new text";
$(".selectedElement").append(newText);
```

Let's say we want to manipulate the value of a form input.

```js
<label>Title</label>
<input type="text" id="titleInput"/>

<label>Content</label>
<textarea id="contentInput"></textarea>

<button id="previewButton">Preview</button>

<div class="previewArea">
  <h2 id="titlePreview"></h2>
  <div id="contentPreview"></div>
</div>
```

```js
/**
  *
  Use click method with a callback function.
  Create local variables to store input values.
  Render the data in variables to selected IDs using text() and html().
  *
  **/
$("#previewButton").click(function() {
  const title = $("#titleInput").val();
  const content = $("#contentInput").val();

  $("#titlePreview").text(title);
  $("#contentPreview").html(content);
});
```
