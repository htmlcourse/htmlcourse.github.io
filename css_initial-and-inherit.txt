# CSS: Initial and Inherit

## Initial Values

The first thing to understand is that every CSS property has a default value called the `initial` value.

The `initial` of the `[color](https://developer.mozilla.org/en-US/docs/Web/CSS/color)` property varies from browser to browser, but has an effective `initial` value of `black`. That initial values applies to all text in your HTML, regardless of the tag being used.

The `[font-size](https://developer.mozilla.org/en-US/docs/Web/CSS/font-size)` property would have an `initial` value of `medium`. How is `medium` defined? In most browsers, `medium` is `16px`. 

The `[line-height](https://developer.mozilla.org/en-US/docs/Web/CSS/line-height)` property would have an `initial` value of `normal`. What does `normal` mean? Well, normal for most browsers is the ratio `1.2`.

### Display: inline

Counter-intuitively, the `initial` value of the `[display](https://developer.mozilla.org/en-US/docs/Web/CSS/display)` property is `inline`. 

Other HTML elements that I have previously said are "block" elements -- e.g., `<p>`, `<h1>`, `<h2>` -- would also have an initial value of `display: inline`. 

Note: that I haven't said specifically what HTML element has this property. But, at this point, if I were to define a `<div>` element, and no other stylesheet were available, it would be set as `display: inline`. Essentially, no `block` elements are defined yet because all HTML elements have `initial` values of `inline`. 

In a little bit, I'll demonstrate this to be the case when I `unset` the `display` property for some of these block elements. We'll see them revert back to their `initial` `inline` values.

## User-Agent Stylesheets

Normally, when I view a web page, normally I can expect "block" elements such as `<div>`, `<p>`, `<h1>`, and `<h2>` to display as blocks. But based on CSS's `initial` values, how does the browser do that?

All browsers ship with a set of default styles that are applied to every web page in what is called the “user-agent stylesheet”. Most of these stylesheets are open source so you can have a look through them:

- [Chromium UA stylesheet](https://chromium.googlesource.com/chromium/blink/+/master/Source/core/css/html.css) - Google Chrome & Opera
- [Mozilla UA stylesheet](https://dxr.mozilla.org/mozilla-central/source/layout/style/res/html.css) - Firefox
- [WebKit UA stylesheet](https://trac.webkit.org/browser/trunk/Source/WebCore/css/html.css) - Safari

## Inheritance

Many CSS properties can be inherited from their parents. A given HTML element that is nested inside another element is the child and the element it is nested within is the parent. 

1. The keyword value of `inherit` tells the browser to search for the closest parent element’s value and let the current element inherit that value. 

2. If there isn’t any value, the browser will use its user-agent stylesheet.

3. If there isn’t any user-agent style, it will use the initial base style. 

Most text-related CSS properties -- such as `color`, `font-size`, `line-height`, etc -- are inherited. 

Because of inheritance, it is common practice to set text styles on the `<body>` element (occasionally on `<html>`) and then override those defaults on just those elements you need to. 

```
body {
  font-family: "Segoe UI", Roboto, "Helvetica Neue", Arial, "Noto Sans", sans-serif;
  font-size: 1rem;
  font-weight: 400;
  line-height: 1.5;
}

h1 {
  font-family: Cheltenham, "ITC Bookman", serif;
  font-size: 2rem;
  font-weight: 700;
  line-height: 1.2;
}
```

## Non-inherited Properties

There are also non-inherited properties, which affect only the element which they define. These are all of the properties that don’t apply to text. 

For example, if you put a border on a parent element, its child will not also receive that border.

[MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/inheritance#Non-inherited_properties) has this example:

Given the style rules:

```
p { 
  border: medium solid; 
}
```

... and the markup:

```
<p>This paragraph has <em>emphasized text</em> in it.</p>
```

... the words "emphasized text" will not have a border (since the initial value of `[border-style](https://developer.mozilla.org/en-US/docs/Web/CSS/border-style)` is `none`).

## Unsetting Styles

The `unset` keyword is unique in that it works differently on inherited vs. non-inherited properties. So, if you set `color: unset`, it will revert to its `inherit` value (from the user-agent stylesheet), whereas `display: unset` will be equal to its `initial` value (its intrinsic value). 

If we’re resetting only one property, then `unset` is unnecessary: we can just use the `inherit` or `initial` values instead. But, to `unset` multiple properties, you can replace something like this:

```
.common-content * { 
  font-size: inherit; 
  font-weight: inherit;
  border-width: initial; 
  background-color: initial;
}
```

with this:

```
.common-content * { 
  all: unset;
}
```

## How Do Styles Get Applied in the Browser?

1. `initial` styles -- CSS properties are assigned `initial` values, even before a stylesheet is involved. They happen without referencing a specific HTML element.
2. User-agent stylesheets -- These are stylesheets that come built into the browser.
3. Custom user stylesheets -- There used to be possible for users to create a local stylesheet that could be applied on top of the browser-supplied user-agent stylesheet. Sometimes, it was just to do something quirky -- e.g., give all the pages on the internet a `hotpink` background with `limegreen` text -- but often it was to improve the accessibility of the user's browsing experience -- e.g., larger font sizes or improved color contrast.
  - Firefox -- You can still [add a `userContent.css` stylesheet](https://davidwalsh.name/firefox-user-stylesheet) to the `chrome` directory of your user profile. 
  - Chrome -- No longer allow custom user stylesheets, instead pushing browser extensions to approximate this functionality -- e.g., [Stylish](https://chrome.google.com/webstore/detail/stylish-custom-themes-for/fjnbnpbmkenffdnngjfgmeleoegfcffe).
4. Author stylesheets -- These are the styles your browser downloads when you visit a website. They are the styles that you, as a web developer, will be writing.

## Resets

A lot of default styles between user-agent stylesheets are now very similar. For example, most browsers apply the same `0.67em` vertical margin and `2em` font size to the `<h1>` element and `8px` margins to the `<body>` element. However, this consistency is relatively new and we still need to consider support for the older browsers that have less consistent default styling.

When an author builds a website, one of the common practices is to start with some sort of reset to improve the user's experience of the website across browsers. 

Broadly, there are three goals that a CSS reset tries to achieve. Some reset stylesheets combine these three goals, whereas others only try to solve one.

1. Correct user agent styling errors
2. Undo any "opinionated" user-agent styling
3. Create a consistent, largely opinionated, style base

Examples:

- [Normalize.css](https://github.com/csstools/normalize.css/blob/master/normalize.css)
- [Eric Meyer's Reset](https://meyerweb.com/eric/tools/css/reset/index.html)
- [Bootstrap Reboot](https://getbootstrap.com/docs/4.0/content/reboot/)

### Resets as Corrections

IE doesn't display `<main>` elements as blocks.

```
main {
  display: block;
}
```

Not all browsers, even modern ones, handle the `hidden` attribute properly.

```
[hidden] {
  display: none !important;
}
```

## Opinionated Resets

Eric Meyer’s CSS Reset returns margin, padding, borders, and font size on specific elements to `0` or `none`. And, it doesn't include elements where you would expect a padding. 

```
html, body, div, span, applet, object, iframe,
h1, h2, h3, h4, h5, h6, p, blockquote, pre,
a, abbr, acronym, address, big, cite, code,
del, dfn, em, img, ins, kbd, q, s, samp,
small, strike, strong, sub, sup, tt, var,
b, u, i, center,
dl, dt, dd, ol, ul, li,
fieldset, form, label, legend,
table, caption, tbody, tfoot, thead, tr, th, td,
article, aside, canvas, details, embed, 
figure, figcaption, footer, header, hgroup, 
menu, nav, output, ruby, section, summary,
time, mark, audio, video {
	margin: 0;
	padding: 0;
	border: 0;
	font-size: 100%;
	font: inherit;
	vertical-align: baseline;
}
```

Once you start having opinions about these different approaches, you might object to his setting `font-size: 100%` on all these elements but see the `font: inherit` rule, especially for forms, as something you would implement.

## Consistency

Because these sorts of resets are opinionated, you might want to roll your own. [Harry Roberts](https://csswizardry.com/2012/06/single-direction-margin-declarations/), for instance, recommends applying vertical margins to block elements in one direction only (usually the bottom margin). He likes a vertical rhythm of 1.5 on his pages, but using a Sass variable or a CSS custom property, you could change it across the board. 

```
:root {
  --vertical-rhythm: 1.3; 
}

html {
  font-size: 1rem; 
  line-height: var(--vertical-rhythm);
}

h1, h2, h3, h4, h5, h6, hgroup, 
ul, ol, dd, 
p, figure, 
pre, table, fieldset, hr {
  margin: 0;
  margin-bottom: calc(var(--vertical-rhythm) * 1rem);
}
```

CodePen: https://codepen.io/tomkeays/pen/oNboYgy

## For Further Reading: 

- [W3C CSS Cascading and Inheritance Level 4](https://drafts.csswg.org/css-cascade/)
- [CSS Initial and Inherit Keywords](https://medium.com/@elad/understanding-the-initial-inherit-and-unset-css-keywords-2d70b7121695)
- [User-Agent Stylesheets and CSS Resets](https://bitsofco.de/a-look-at-css-resets-in-2018/)
