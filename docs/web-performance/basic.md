---
id: basic
title: Basic
---

## Accessibility

When someone describes a site as "accessible," they mean that any user can use all its features and content, regardless of how the user accesses the web — even and especially users with physical or mental impairments.

- Semantic HTML: This means using the correct HTML elements for their intended purpose as much as possible
- CSS and JavaScript accessibility best practices
  - CSS
    - Correct semantics and user expectation: h1, h2, em, Abbreviations
    - Color and color contrast
    - Hiding things: tabs etc
  - JavaScript
    - The problem with too much JavaScript :hover
    - Other JavaScript accessibility concerns
- WAI-ARIA (Web Accessibility Initiative - Accessible Rich Internet Applications) is a specification written by the W3C, defining a set of additional HTML attributes that can be applied to elements to provide additional semantics and improve accessibility wherever it is lacking. There are three main features defined in the spec:
  - Roles: role="navigation"
  - Properties: aria-required="true"
  - States: aria-disabled="true"

## Web performance

Web performance is all about making web sites fast, including making slow processes seem fast. Does the site load quickly, allow the user to start interacting with it quickly, and offer reassuring feedback if something is taking time to load (e.g. a loading spinner)?

This includes the following major areas:

- Reducing overall load time
- Making the site usable as soon as possible
- Smoothness and interactivity
- Perceived performance is how fast a website seems to the use
  1. **First paint** is reported by the browser and provides the time, in ms, of when the page starts changing; but this change can be a simple background color update or something even less noticable.
  2. **First Contentful Paint** (FCP) reports the time when the browser first rendered anything of signifigance, be that text, foreground or background image, or a canvas or SVG; capturing the very beginning of the loading experience
  3. The **First Meaningful Paint**, or FMP, is the when content appears on the screen that is actually meaningful; which is a better metric for user-perceived loading experience
- Measuring performance:
  - Performance APIs
  - Tools and metrics: PageSpeed Insights, Lighthouse, network tools
- Multimedia, Images: Optimizing image delivery
- JavaScript performance
  - Download impact
    - Reducing the amount of JavaScript that is needed.
    - Remove unused code
    - Split the JavaScript into smaller files
    - Optimize these smaller files: Minification
