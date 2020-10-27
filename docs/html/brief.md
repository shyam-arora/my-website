---
id: brief
title: Brief
---

## [Render-tree Construction, Layout, and Paint](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/render-tree-construction)

1. The DOM and CSSOM trees are combined to form the render tree.
2. Render tree contains only the nodes required to render the page.
3. Layout computes the exact position and size of each object.
4. The last step is paint, which takes in the final render tree and renders the pixels to the screen.

![Alt text](/img/render-tree-construction.png "Title")

## [CSS Specificity](https://www.w3schools.com/css/css_specificity.asp)

If there are two or more conflicting CSS rules that point to the same element, the browser follows some rules to determine which one is most specific and therefore wins out.

The universal selector (\*) has low specificity, while ID selectors are highly specific!

### Specificity Hierarchy

Every selector has its place in the specificity hierarchy. There are four categories which define the specificity level of a selector:

1. Inline styles - An inline style is attached directly to the element to be styled.
2. IDs - An ID is a unique identifier for the page elements, such as #navbar.
3. Classes, attributes and pseudo-classes - This category includes .classes, [attributes] and pseudo-classes such as :hover, :focus etc.
4. Elements and pseudo-elements - This category includes element names and pseudo-elements, such as h1, div, :before and :after.

## [CSS Selectors](https://www.w3schools.com/css/css_selectors.asp)

We can divide CSS selectors into five categories:

1. Simple selectors (select elements based on name, id, class)
2. Combinator selectors (select elements based on a specific relationship between them)
   1. descendant selector (space): The descendant selector matches all elements that are descendants of a specified element.
   2. child selector (>): The child selector selects all elements that are the children of a specified element.
   3. adjacent sibling selector (+): The adjacent sibling selector selects all elements that are the adjacent siblings of a specified element.
   4. general sibling selector (~): The general sibling selector selects all elements that are siblings of a specified element.
3. Pseudo-class selectors (select elements based on a certain state): link, visited, hover, first-child etc.
4. Pseudo-elements selectors (select and style a part of an element): ::first-letter, ::first-line, ::before etc
   Notice the double colon notation - ::first-line versus :first-line
   The double colon replaced the single-colon notation for pseudo-elements in CSS3. This was an attempt from W3C to distinguish between pseudo-classes and pseudo-elements.
5. Attribute selectors (select elements based on an attribute or attribute value): a[target], [attribute="value"]

## What's the difference between a relative, fixed, absolute and statically positioned element?

A positioned element is an element whose computed position property is either relative, absolute, fixed or sticky.

- **static** - The default position; the element will flow into the page as it normally would. The top, right, bottom, left and z-index properties do not apply.
- **relative** - The element's position is adjusted relative to itself, without changing layout (and thus leaving a gap for the element where it would have been had it not been positioned).
- **absolute** - The element is removed from the flow of the page and positioned at a specified position relative to its closest positioned ancestor if any, or otherwise relative to the initial containing block. Absolutely positioned boxes can have margins, and they do not collapse with any other margins. These elements do not affect the position of other elements.
- **fixed** - The element is removed from the flow of the page and positioned at a specified position relative to the viewport and doesn't move when scrolled.
- **sticky** - Sticky positioning is a hybrid of relative and fixed positioning. The element is treated as relative positioned until it crosses a specified threshold, at which point it is treated as fixed positioned.

## Notes

- A **repaint** occurs when changes are made to an elements skin that changes visibly, but do not affect its layout.
  Examples of this include outline, visibility, background, or color. According to Opera, repaint is expensive because the browser must verify the visibility of all other nodes in the DOM tree.

- A **reflow** is even more critical to performance because it involves changes that affect the layout of a portion of the page (or the whole page).
  Examples that cause reflows include: adding or removing content, explicitly or implicitly changing width, height, font-family, font-size and more

- **window.onload** is fired when DOM is ready and all the contents including images, css, scripts, sub-frames, etc. finished loaded. This means everything is loaded.

- **document.onload** is fired when DOM (DOM tree built from markup code within the document)is ready which can be prior to images and other external content is loaded.

- **documentFragment** a very lightweight or minimal part of a DOM or a subtree of a DOM tree. It is very helpful when you are manipulating a part of DOM for multiple times. It becomes expensive to hit a certain portion of DOM for hundreds time. You might cause reflow for hundred times. Stay tuned for reflow.

If you are changing dom that cause expensive reflow, you can avoid it by using documentFragment as it is managed in the memory.

```javascript
// bad practice. you are hitting the DOM every single time
var list = ['foo', 'bar', 'baz', ... ],
    el, text;
for (var i = 0; i < list.length; i++) {
    el = document.createElement('li');
    text = document.createTextNode(list[i]);
    el.appendChild(text);
    document.body.appendChild(el);
}
  //good practice. you causing reflow one time
var fragment = document.createDocumentFragment(),
    list = ['foo', 'bar', 'baz', ...],
    el, text;
for (var i = 0; i < list.length; i++) {
    el = document.createElement('li');
    text = document.createTextNode(list[i]);
    el.appendChild(text);
    fragment.appendChild(el);
}
document.body.appendChild(fragment);
```

## References

- [**HTML elements reference**](https://developer.mozilla.org/en-US/docs/Web/HTML/Element)
- [**JS Dom**](https://www.thatjsdude.com/interview/dom.html)
- [**Web Components**](https://css-tricks.com/an-introduction-to-web-components/)
