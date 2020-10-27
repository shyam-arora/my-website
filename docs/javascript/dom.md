---
id: dom
title: Dom
---

## Browser environment, specs

Here’s a bird’s-eye view of what we have when JavaScript runs in a web-browser:
Window

- DOM:
  - document
  - DOM is not only for browsers
- BOM: host environment
  - navigator
    - userAgent
    - platform
  - location
  - XMLHttpRequest
  - setTimeout, alert etc
- Javascript

## Walking the DOM

All operations on the DOM start with the document object. That’s the main “entry point” to DOM.
![Alt text](/img/domwalk.png "Title")
DOM collections
childNodes looks like an array. But actually it’s not an array, but rather a collection – a special array-like iterable object.

```js
for (let node of document.body.childNodes) {
  alert(node); // shows all nodes from the collection
}
```

- DOM collections are read-only
- Don’t use for..in to loop over collections
- Siblings are nodes that are children of the same parent.

## Searching: getElement*, querySelector*

- document.getElementById: Only document.getElementById, not anyElem.getElementById
- elem.getElementsByTagName(tag)
- elem.getElementsByClassName(className)
- elem.querySelectorAll: By far, the most versatile method, elem.querySelectorAll(css) returns all elements inside elem matching the given CSS selector

```html
<ul>
  <li>The</li>
  <li>test</li>
</ul>
<ul>
  <li>has</li>
  <li>passed</li>
</ul>
<script>
  let elements = document.querySelectorAll("ul > li:last-child");

  for (let elem of elements) {
    alert(elem.innerHTML); // "test", "passed"
  }
</script>
```

- elem.querySelector: The call to elem.querySelector(css) returns the first element for the given CSS selector.

- matches
  The elem.matches(css) does not look for anything, it merely checks if elem matches the given CSS-selector. It returns true or false.

- closest
  The method elem.closest(css) looks the nearest ancestor that matches the CSS-selector. The elem itself is also included in the search.

- Live collections
  All methods "getElementsBy\*" return a live collection. Such collections always reflect the current state of the document and “auto-update” when it changes. In contrast, querySelectorAll returns a static collection. It’s like a fixed array of elements.
  ![Alt text](/img/cssSelector.png "Title")

## Node properties: type, tag and contents

![Alt text](/img/domInher.png "Title")

- EventTarget – is the root “abstract” class. Objects of that class are never created. It serves as a base, so that all DOM nodes support so-called “events”

---

console.dir(elem) versus console.log(elem)
Most browsers support two commands in their developer tools: console.log and console.dir. They output their arguments to the console. For JavaScript objects these commands usually do the same.

But for DOM elements they are different:

console.log(elem) shows the element DOM tree.
console.dir(elem) shows the element as a DOM object, good to explore its properties.
Try it on document.body.

---

Each DOM node belongs to a certain class. The classes form a hierarchy. The full set of properties and methods come as the result of inheritance.

Main DOM node properties are:

- nodeType: We can use it to see if a node is a text or an element node. It has a numeric value: 1 for elements,3 for text nodes, and a few others for other node types. Read-only.
- nodeName/tagName: For elements, tag name (uppercased unless XML-mode). For non-element nodes nodeName describes what it is. Read-only.
- innerHTML: The HTML content of the element. Can be modified.
- outerHTML: The full HTML of the element. A write operation into elem.outerHTML does not touch elem itself. Instead it gets replaced with the new HTML in the outer context.
- nodeValue/data: The content of a non-element node (text, comment). These two are almost the same, usually we use data. Can be modified.
- textContent: The text inside the element: HTML minus all. Writing into it puts the text inside the element, with all special characters and tags treated exactly as text. Can safely insert user-generated text and protect from unwanted HTML insertions.
- hidden: When set to true, does the same as CSS display:none.

DOM nodes also have other properties depending on their class. For instance, input elements (HTMLInputElement) support value, type, while 'a' elements (HTMLAnchorElement) support href etc. Most standard HTML attributes have a corresponding DOM property.

## Attributes and properties

- Attributes – is what’s written in HTML.
- Properties – is what’s in DOM objects

Methods to work with attributes are:

elem.hasAttribute(name) – to check for existence.
elem.getAttribute(name) – to get the value.
elem.setAttribute(name, value) – to set the value.
elem.removeAttribute(name) – to remove the attribute.
elem.attributes is a collection of all attributes.

## Modifying the document

- Methods to create new nodes:
  - document.createElement(tag) – creates an element with the given tag,
  - document.createTextNode(value) – creates a text node (rarely used),
  - elem.cloneNode(deep) – clones the element, if deep==true then with all descendants.
- Insertion and removal:

  - node.append(...nodes or strings) – insert into node, at the end,
  - node.prepend(...nodes or strings) – insert into node, at the beginning,
  - node.before(...nodes or strings) –- insert right before node,
  - node.after(...nodes or strings) –- insert right after node,
  - node.replaceWith(...nodes or strings) –- replace node.
  - node.remove() –- remove the node.
    Text strings are inserted “as text”.

- There are also “old school” methods:

  - parent.appendChild(node)
  - parent.insertBefore(node, nextSibling)
  - parent.removeChild(node)
  - parent.replaceChild(newElem, node)
  - All these methods return node.

- Given some HTML in html, elem.insertAdjacentHTML(where, html) inserts it depending on the value of where:

  - "beforebegin" – insert html right before elem,
  - "afterbegin" – insert html into elem, at the beginning,
  - "beforeend" – insert html into elem, at the end,
  - "afterend" – insert html right after elem.
    Also there are similar methods, elem.insertAdjacentText and elem.insertAdjacentElement, that insert text strings and elements, but they are rarely used.

- To append HTML to the page before it has finished loading:
  - document.write(html)
  - After the page is loaded such a call erases the document. Mostly seen in old scripts

## Styles and classes

- To manage classes, there are two DOM properties:

  - className – the string value, good to manage the whole set of classes.
  - classList – the object with methods add/remove/toggle/contains, good for individual classes.

- To change the styles:

  - The style property is an object with camelCased styles. Reading and writing to it has the same meaning as modifying individual properties in the "style" attribute. To see how to apply important and other rare stuff – there’s a list of methods at MDN.

  - The style.cssText property corresponds to the whole "style" attribute, the full string of styles.

- To read the resolved styles (with respect to all classes, after all CSS is applied and final values are calculated):
  - The getComputedStyle(elem, [pseudo]) returns the style-like object with them. Read-only.

```html
<head>
  <style>
    body {
      color: red;
      margin: 5px;
    }
  </style>
</head>
<body>
  The red text
  <script>
    alert(document.body.style.color); // empty
    alert(document.body.style.marginTop); // empty
  </script>
</body>

<head>
  <style>
    body {
      color: red;
      margin: 5px;
    }
  </style>
</head>
<body>
  <script>
    let computedStyle = getComputedStyle(document.body);

    // now we can read the margin and the color from it

    alert(computedStyle.marginTop); // 5px
    alert(computedStyle.color); // rgb(255, 0, 0)
  </script>
</body>
```

## [Element size and scrolling](https://javascript.info/size-and-scroll)

## [Window sizes and scrolling](https://javascript.info/size-and-scroll-window)

Page: DOMContentLoaded, load, beforeunload, unload

The lifecycle of an HTML page has three important events:

- DOMContentLoaded – the browser fully loaded HTML, and the DOM tree is built, but external resources like pictures img and stylesheets may be not yet loaded.
- load – not only HTML is loaded, but also all the external resources: images, styles etc.
- beforeunload/unload – the user is leaving the page.
  Each event may be useful:

- DOMContentLoaded event – DOM is ready, so the handler can lookup DOM nodes, initialize the interface.
- load event – external resources are loaded, so styles are applied, image sizes are known etc.
- beforeunload event – the user is leaving: we can check if the user saved the changes and ask them whether they really want to leave.
- unload – the user almost left, but we still can initiate some operations, such as sending out statistics.

- document.readyState is the current state of the document, changes can be tracked in the readystatechange event:
  - loading – the document is loading.
  - interactive – the document is parsed, happens at about the same time as DOMContentLoaded, but before it.
  - complete – the document and resources are loaded, happens at about the same time as window.onload, but before it.

## Cross-window communication

Broadly, one window may obtain a reference to another (e.g., via targetWindow = window.opener), and then dispatch a MessageEvent on it with targetWindow.postMessage(). The receiving window is then free to handle this event as needed. The arguments passed to window.postMessage() (i.e., the “message”) are exposed to the receiving window through the event object.
The window.postMessage() method safely enables cross-origin communication between Window objects; e.g., between a page and a pop-up that it spawned, or between a page and an iframe embedded within it.

To call methods and access the content of another window, we should first have a reference to it.

For popups we have these references:

- From the opener window: window.open – opens a new window and returns a reference to it,
- From the popup: window.opener – is a reference to the opener window from a popup.
  For iframes, we can access parent/children windows using:

- window.frames – a collection of nested window objects,
- window.parent, window.top are the references to parent and top windows,
- iframe.contentWindow is the window inside an `<iframe>` tag.
  If windows share the same origin (host, port, protocol), then windows can do whatever they want with each other.

Otherwise, only possible actions are:

- Change the location of another window (write-only access).
- Post a message to it.
  Exceptions are:

- Windows that share the same second-level domain: a.site.com and b.site.com. Then setting document.domain='site.com' in both of them puts them into the “same origin” state.
- If an iframe has a sandbox attribute, it is forcefully put into the “different origin” state, unless the allow-same-origin is specified in the attribute value. That can be used to run untrusted code in iframes from the same site.
  The postMessage interface allows two windows with any origins to talk:

- The sender calls targetWin.postMessage(data, targetOrigin).

- If targetOrigin is not '\*', then the browser checks if window targetWin has the origin targetOrigin.

- If it is so, then targetWin triggers the message event with special properties:

  - origin – the origin of the sender window (like http://my.site.com)
  - source – the reference to the sender window.
  - data – the data, any object in everywhere except IE that supports only strings.
    We should use addEventListener to set the handler for this event inside the target window.

**Reference**

1. DOM (https://javascript.info/)
