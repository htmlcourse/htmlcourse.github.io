# CSS Specificity

Specificity is a counting and categorization game. According to [MDN's Specificity](https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity)

>Specificity is the means by which browsers decide which CSS property values are the most relevant to an element and, therefore, will be applied. Specificity is based on the matching rules which are composed of different sorts of CSS selectors.
>
>Specificity is a weight that is applied to a given CSS declaration, determined by the number of each selector type in the matching selector. When multiple declarations have equal specificity, the last declaration found in the CSS is applied to the element. Specificity only applies when the same element is targeted by multiple declarations. As per CSS rules, directly targeted elements will always take precedence over rules which an element inherits from its ancestor.

We've defined some of this previously, but in this module we'll take a deeper look at how HTML elements are constructed and how CSS references them. The following list of selector types is arranged from higher to lower specificity. We'll discuss each of them in more detail.

1. ID selectors (e.g., `#example`).
2. Class selectors (e.g., `.example`), attributes selectors (e.g., `[type="radio"]`) and pseudo-classes (e.g., `:hover`).
3. Type selectors (e.g., `h1`) and pseudo-elements (e.g., `::before`).

When you are tallying up the specificity, one convention is to order them left to right and count how many of each each selector types the rule contains.

E.G., this declaration would have a specificity of `1 0 1` or 1 ID selector, no class/attribute/pseudo-class selectors, and 1 type/pseudo-element selector.

```
#someElement p {
  color: blue;
}
```

You can also tag on the following specificity exceptions or extensions.

4. Inline style exception (e.g., `style="font-weight: bold;"`)
5. The !important exception (e.g. `!important` added to any rule)

See: https://specifishity.com/

## 1. ID selectors


In this example, the ID selector will always have higher specificity, which makes it almost impossible to apply the `color: red` to the paragraph.

```
#someElement p {
  color: blue;
}

p.awesome {
  color: red;
}
```

However, you can rewrite the ID selector rule as an attribute selector. 

```
[id="someElement"] p {
  color: blue;
}

p.awesome {
  color: red;
}
```

Because attribute selectors and class selectors have the same weight, and because they both have the same type selector, they have exactly the same specificity: `0 1 1`. Therefore, it is the order of the rules rather than the specificity that determines whether the paragraph color will be red or blue.

CodePen: https://codepen.io/tomkeays/pen/RwrjJyz


## 2. Class selectors, attributes selectors and pseudo-classes

The negation pseudo-class (`:not()`) have no effect on specificity. (The selectors declared inside `:not()` do, however.)


## 3. Type selectors and pseudo-elements

Universal selector (`*`), combinators (`+`, `>`, `~`, ' ', `||`).


## Star Wars Specificity Game

Paulina Hetman created a Star Wars themed specificity game to help you get the hang of counting specificity.

<div class="rules">

![Darth Vader](https://s3-us-west-2.amazonaws.com/s.cdpn.io/74321/persos-darth-vader.svg) -- count the number of ID selectors **strength value = 1 0 0**

![Boba Fett](https://s3-us-west-2.amazonaws.com/s.cdpn.io/74321/persos-boba-fett.svg) -- count the number of class selectors, attributes selectors, and pseudo-classes strength value = 1 0**

![Storm Trooper](https://s3-us-west-2.amazonaws.com/s.cdpn.io/74321/persos-stormtrooper.svg) -- count the number of type selectors and pseudo-elements **strength value = 1**

</div>
<style>
.rules img {
  height: 2rem;
  margin: 0 .5rem 0 0;
  vertical-align: top;
  float: left;
}
</style>

Game: https://codepen.io/pehaa/live/OYLwGW


## 4. Inline style exception

Inline styles added to an element (e.g., `style="font-weight: bold;"`) always overwrite any styles in external stylesheets, and thus can be thought of as having the highest specificity.


## 5. The !important exception

When an `!important` rule is used on a style declaration, this declaration overrides any other declarations. Although technically `!important` has nothing to do with specificity, it interacts directly with it. 

Caveats / Rules of thumb

- Always look for a way to use specificity before even considering `!important`
- Only use `!important` on page-specific CSS that overrides foreign CSS (from external libraries, like Bootstrap or normalize.css).
- Never use `!important` on site-wide CSS.






## For Further Reading: 

- [MDN Web Docs: Specificity](https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity)
- [On How I Approach Teaching CSS, CodePen Quizzes and Playing Cards](https://pepsized.com/on-how-i-approach-teaching-css-codepen-quizzes-and-playing-cards/)
- [The Specificity Graph](https://csswizardry.com/2014/10/the-specificity-graph/)
- [Semantic CSS With Intelligent Selectors](https://www.smashingmagazine.com/2013/08/semantic-css-with-intelligent-selectors/)


- [CSS Inheritance, The Cascade And Global Scope: Your New Old Worst Best Friends](https://www.smashingmagazine.com/2016/11/css-inheritance-cascade-global-scope-new-old-worst-best-friends/)