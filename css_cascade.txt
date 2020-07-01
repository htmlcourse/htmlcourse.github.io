# CSS Cascade

According to [the W3C Level 4 Draft Specification](https://drafts.csswg.org/css-cascade/#cascade)

>The cascade takes an unordered list of declared values for a given property on a given element, sorts them by their declaration’s precedence ... The output of the cascade is a (potentially empty) sorted list of declared values for each property on each element.

The cascade sorts declarations according to the following criteria, in descending order of priority:

1. Origin and Importance -- The origin of a declaration is based on where it comes from and its importance is whether or not it is declared `!important`. Amelia Wattenberger's article separates these and that's the approach we'll take.
2. Scope -- According to the spec, "A declaration can be scoped to a subtree of the document so that it only affects its scoping element and that element’s descendants." 
3. Specificity 
4. Order of Appearance

## 1. Origin and Importance

According to [the W3C Level 4 Draft Specification](https://drafts.csswg.org/css-cascade/#cascade)

The precedence of the various origins is, in descending order:

1. Transition declarations [[css-transitions-1](https://www.w3.org/TR/css-transitions-1/)]
2. Important user-agent declarations
3. Important user declarations
4. Important author declarations
5. Animation declarations [[css-animations-1](https://www.w3.org/TR/css-animations-1/)]
6. Normal author declarations
7. Normal user declarations
8. Normal user-agent declarations

Declarations from origins earlier in this list win over declarations from later origins.

However, Amelia Wattenberger wrote an article, "[The CSS Cascade](https://wattenberger.com/blog/css-cascade)", that separates the concerns of origin and importance that makes things a little bit easier to understand.

### Importance

First, consider the importance rules. 

1. **transition** -- Rules that apply to an active transition take the utmost importance. Practically speaking, this will be an edge case, so don't worry about it for now.
2. **!important** -- When we add `!important` to the end of our declaration, it trumps all other equivalent declarations. Ideally, you reserve this level for Hail Marys, which are needed to override styles from third-party libraries.
3. **animation** -- Rules that apply to an active animation jump up a level in the Cascade. Similar to transitions, this will be an edge case.
4. **normal** -- This level is where the bulk of rules live.

### Origin

The second tier of the Cascade looks at where the rule was defined.

1. **author** -- These are the rules that you, as a web developer, write. 
2. **user** -- These are user-defined stylesheets. They are not much used and not much you can do about.
3. **user-agent ** -- These are the styles that come with your browser. We talked briefly about these in the previous unit. These styles are similar, although not identical, between browsers. Form elements have the most differences and often require remediation. 

Normally, the author styles will trump user styles which will trump user-agent styles. However, the hierarchy is reversed for `!important` rules, because, in this use case, you need user-agent and user styles to trump your author styles.

## 2. Scope

According to [the W3C Level 4 Draft Specification](https://drafts.csswg.org/css-cascade/#cascade)

> If the scoping elements of two declarations have an ancestor/descendant relationship, then for normal rules the declaration whose scoping element is the descendant wins, and for important rules the declaration whose scoping element is the ancestor wins.
>
>For the purpose of this step, all unscoped declarations are considered to be scoped to the `root` element. Normal declarations from `style` attributes are considered to be scoped to the element with the attribute, whereas `important` declarations from `style` attributes are considered to be scoped to the `root` element.

Here's an example of scoping from [Miriam Suzanne](https://css-tricks.com/using-custom-property-stacks-to-tame-the-cascade/). 

```
/* all links */
a { color: slateblue; }

/* only links inside a section */
section a { color: rebeccapurple; }

/* only links inside an article */
article a { color: deeppink; }
```

This is not the same as specificity, although once you start counting selectors, you might be tempted to think so. The two declarations `section a` and `article a` both have higher specificity than `a` by itself, that's true. However, `section a` and `article a` are scoping the rules to different parts of the HTML document. They are not competing with each other.

## 3. Specificity

We will spend more time on specificity in the next section, so I'll only touch briefly on the hierarchy now.

When we create a CSS declaration, we can target specific elements using selectors.

1. **inline** -- Styles declared within a style HTML property are the most specific. E.G., `<p style="color: sandybrown">...</p>`
2. **id** -- We can target elements based on their id, using the syntax `#id`. E.G., `#introduction { color: red; }`
3. **class | attribute | pseudo-class** -- We can target elements based on their class, using the syntax `.class`. This level also includes attribute selectors that target HTML attributes, like `[checked]` and `[href="https://wattenberger.com"]`. This level also includes pseudo-selectors, like `:hover` and `:first-of-type`. E.G., `.first-paragraph { color: purple; }`
4. **type | pseudo-element** --  We can target elements based on their tag type, using the syntax type. This level also includes pseudo-elements, like `:before` and `:selection`. E.G., `p { color: orchid; }`

Specificity is a counting and categorization game. According to [MDN's Specificity](https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity)

> Specificity is a weight that is applied to a given CSS declaration, determined by the number of each selector type in the matching selector. When multiple declarations have equal specificity, the last declaration found in the CSS is applied to the element. Specificity only applies when the same element is targeted by multiple declarations. As per CSS rules, directly targeted elements will always take precedence over rules which an element inherits from its ancestor.

See: https://specifishity.com/

## 4. Order of Appearance

The last declaration in document order wins. Imported and linked stylesheets are concatenated together in order they are referenced. 

This one makes a good case for defining style declarations using some sort of system. In an upcoming lecture, we'll explore a few strategies for keeping your code under control. 

## For Further Reading: 

- [W3C CSS Cascading and Inheritance Level 4](https://drafts.csswg.org/css-cascade/)
- [The CSS Cascade](https://wattenberger.com/blog/css-cascade)
- [Introducing the CSS Cascade](https://developer.mozilla.org/en-US/docs/Web/CSS/Cascade)
