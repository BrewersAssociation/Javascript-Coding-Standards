# Brewers Association Coding Standards: CSS

This document is based on [Reasonable Simple CSS](https://github.com/rstacruz/rscss), [Jonathan Snook's SMACCS](https://smacss.com/), and [CSS Guidelines](http://cssguidelin.es/)

This document will outline the best practices for organizing and styling CSS documents for Brewers Association Projects.
> ##Overview
> ###Write Clean Code
> - [Use tabs over spaces for indents](http://lea.verou.me/2012/01/why-tabs-are-clearly-superior/)
> - 80 character wide columns
> - multi-line CSS
> - indent ruleset to show relation
> - meaningful whitespace
> - comment your code

> ###Split CSS files into manageable sizes
> - Split discrete chunks of code into their own files, which are concatenated during a build step.

>###Separate Structure from design  (BEM like)

>**We are using a Component Element Variant Model**

>- Think in components, named with 2 words (.blog-post)
>- Components have elements, named with 1 word (.blog-post .title)
>- Name variants with a dash prefix (.title. -with-icon)
>

---
##Coding Styles

###Comments
The master CSS file should include a Table of Contents

```css
/**
* 0.0 Table of Contents
*
* Less document to be compiled on the client site. Contains all core CSS for theme. Uses Bootstrap for base styles
*
* 
* 1.0 Configuration & Reset
* 2.0 Typography
* 3.0 Structure
* 4.0 Navigation
* 5.0 Tags
* - 5.1 Tables
* - 5.2a Lists
* - 5.2b Infobox
* - 5.3 Forms
* - 5.4 UI Elements
* 6.0 Selectors (Classes)
* 7.0 Page Specific, Widgets
* 8.0 Phark
* 9.0 Media Queries
* 10.0 Plugins
**/
```

Begin every new major section of a CSS project with a title:

```css
/**
* 1.0 Configuration and Reset
*
**/
```

Explain major blocks of CSS with a Comment Block
```css
/**
 * The site’s main page-head can have two different states:
 *
 * 1) Regular page-head with no backgrounds or extra treatments; it just
 *    contains the logo and nav.
 * 2) A masthead that has a fluid-height (becoming fixed after a certain point)
 *    which has a large background image, and some supporting text.
 *
 * The regular page-head is incredibly simple, but the masthead version has some
 * slightly intermingled dependency with the wrapper that lives inside it.
 */
```

Subsections should have a unique comments style

```less
/* 5.1 Tables */

// A 3rd Level Heading
// ------------------------------------------------------------------ 

//-- Another comment style to help break up blocks of content --//
```

###Anatomy of a Ruleset
```css
.foo, .foo--bar,
.baz {
    display: block;
    background-color: green;
    color: red;
}
.foo { //example of alignment
    -webkit-border-radius: 3px;
       -moz-border-radius: 3px;
            border-radius: 3px;
}
```

Here you can see we have

- related selectors on the same line; unrelated selectors on new lines;
- a space before our opening brace ({);
- properties and values on the same line;
- a space after our property–value delimiting colon (:);
- each declaration on its own new line;
- the opening brace ({) on the same line as our last selector;
- our first declaration on a new line after our opening brace ({);
- our closing brace (}) on its own new line;
- each declaration indented by one tab
- a trailing semi-colon (;) on our last declaration.
- CSS should be written across multiple lines, except in very specific circumstances.
- Attempt to align common and related identical strings in declarations

###Meaningful Whitespace

As well as indentation, we can provide a lot of information through liberal and judicious use of whitespace between rulesets. We use:

One (1) empty line between closely related rulesets.
Two (2) empty lines between loosely related rulesets.
Five (5) empty lines between entirely new sections.

###Avoid over nesting
Use no more than 1 level of nesting. It's easy to get lost with too much nesting.

```css
.image-frame {
  > .description { /* ... */ }
  > .description > .icon { /* ... */ }
}
```
---
##Structure

### Think in components

![](https://raw.githubusercontent.com/BrewersAssociation/CSS-Standards/master/images/component-example.png)

Think of each piece of your UI is an individual "component." Components will be named with **at least two words**, separated by a dash. Examples of a component:

* A like button (`.like-button`)
* A search form (`.search-form`)
* A news article card (`.article-card`)

### Elements

![](https://raw.githubusercontent.com/BrewersAssociation/CSS-Standards/master/images/component-elements.png)

**Naming:** Each component may have elements. They should have classes that are only **one word**.

```css
.search-form {
  > .field { /* ... */ }
  > .action { /* ... */ }
}
```

**Selectors:** Prefer to use the `>` child selector whenever possible. This prevents bleeding through nested components, and performs better than descendant selectors.

```css
.article-card {
  .title     { /* okay */ }
  > .author  { /* ✓ better */ }
}
```

**On multiple words:** For those that need two or more words, concatenate them without dashes or underscores.

```scss
.profile-box {
  > .firstname { /* ... */ }
  > .lastname { /* ... */ }
  > .avatar { /* ... */ }
}
```

**Avoid tag selectors:** use classnames whenever possible. Tag selectors are fine, but they may come at a small performance penalty and may not be as descriptive.

```scss
.article-card {
  > h3    { /* ... */ }
  > .name { /* ✓ better */ }
}
```

### Variants

![](https://raw.githubusercontent.com/BrewersAssociation/CSS-Standards/master/images/component-modifiers.png)

Components may have variants. Their classes will be postfixed by a dash (`-`).

```css
.btn-wide { /* ... */ }
.btn-short { /* ... */ }
.btn-disabled { /* ... */ }
```

Elements may also have variants.

```css
.shopping-card {
  > .title { /* ... */ }
  > .title-small { /* ... */ }
}
```
---
##Layout

![](https://raw.githubusercontent.com/BrewersAssociation/CSS-Standards/master/images/layouts.png)

**Avoid positioning properties:** Components should be made in a way that they're reusable in different contexts. Avoid putting these properties in components:

* Positioning (`position`, `top`, `left`, `right`, `bottom`)
* Floats (`float`, `clear`)
* Margins (`margin`)
* Dimensions (`width`, `height`) *

**Fixed dimensions:** Exception to these would be elements that have fixed width/heights, such as avatars and logos.

**Define positioning in parents:** If you need to define these, try to define them in whatever context whey will be in. In this example below, notice that the widths and floats are applied on the *list* component, not the component itself.

**Separate Layout and Style**: Again, see how the layout is defined in the context of a *recipe list*. The individual *recipe items* are styled below.

```css
.recipe-list {

  > .recipe-item {
    width: 33.3%;
    float: left;
  }
}

.recipe-item {
  & { /* ... */ }
  > .image { /* ... */ }
  > .title { /* ... */ }
  > .category { /* ... */ }
}
```





> Written with [StackEdit](https://stackedit.io/).
