# PatternFly 5 HTML/CSS Code Guidelines

Please enforce these guidelines at all times. Small or large, call out what's incorrect.

> Every line of code should appear to be written by a single person, no matter the number of contributors.

This set of rules generate some constraints and conventions. If you run into instances where a convention isn’t obvious or a solution could be handled in a few different ways, contact the PatternFly community and have a conversation about how to handle it and update these guidelines when needed.


## Table of content (WIP)

- HTML
  - [Syntax](#syntax)
  - [Attribute order](#attribute-order)
  - [Reducing markup](#reducing-markup)

- CSS
  - [Syntax](#syntax-1)
  - [Declaration order](#declaration-order)
  - [Prefixed properties](#prefixed-properties)
  - [Single declarations](#single-declarations)
  - [Shorthand notation](#shorthand-notation)
  - [Comment and Organization](#comment-and-organization)
  - [Naming Selectors](#naming-selectors)
  - [Specificity](#specificity)
  - [Spacing](#spacing)
  - [Shadows](#shadows)
  - [Gradients](#gradients)
  - [Sass](#sass)
  - [Credits and references](#credits-and-references)

<!-- ============================================================ -->

# HTML

**Practicality over purity**. Strive to maintain HTML standards and semantics, but not at the expense of practicality. Use the least amount of markup with the fewest intricacies whenever possible.

<!-- ============================================================ -->

## Syntax

- Use soft tabs with two spaces.
- Nested elements should be indented once (two spaces).
- Always use double quotes, never single quotes, on attributes.
- Don’t include a trailing slash in self-closing elements.
- Don’t omit optional closing tags (e.g. </li> or </body>).
- Don't add a value to a boolean attribute e.g., `<input type="text" disabled>`.
- Lowercase tag name.
- Lowercase attribute name.
- Use HTML5 elements where appropriate, e.g., `article`, `aside`, `figure`, `figcaption`, `header`, `footer`, `main`, `nav`, `section`.

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Page title</title>
  </head>
  <body>
    <img src="images/company-logo.png" alt="Company">
    <h1 class="hello-world">Hello, world!</h1>
    <input type="text" disabled>
  </body>
</html>
```

<!-- ============================================================ -->

## Attribute order

HTML attributes should come in this particular order for easier reading of code.

- `class`
- `id`, `name`
- `data-*`
- `src`, `for`, `type`, `href`, `value`
- `title`, `alt`
- `role`, `aria-*`


```html
<a class="..." id="..." data-toggle="modal" href="#">
  Example link
</a>

<input class="form-control" type="text">

<img src="..." alt="...">
```

<!-- ============================================================ -->

## Reducing markup

Whenever possible, avoid superfluous parent elements when writing HTML. Take the following example:

```html
<!-- Not so good -->
<span class="avatar">
  <img src="...">
</span>

<!-- Better -->
<img class="avatar" src="...">
```

<!-- ============================================================ -->

# CSS

Before we discuss how we write out our rulesets, let’s first familiarize ourselves with relevant terminology:

```
[selector] {
  [property]: [value];
  [<--declaration--->]
}
[<------rule-------->]
```

For example:

```sass
.foo {
  display: block;
}
```

<!-- ============================================================ -->

## Syntax
Patternfly uses [stylelint](https://stylelint.io) to enforce the following formatting:

```sass
// Bad
.selector, .selector-secondary, .selector[type=text] {
  padding:15px;
  margin:0px 0px 15px;
  background-color:rgba(0, 0, 0, 0.5);
  box-shadow:0px 1px 2px #CCC,inset 0 1px 0 #FFFFFF
}

// Good
.selector,
.selector__element,
.selector[type="text"] {
  padding: 15px;
  margin-bottom: 15px;
  background-color: rgba(0,0,0,.5);
  box-shadow: 0 1px 2px #ccc, inset 0 1px 0 #fff;
}
```
To see a full list of the lint rules please see [linting.json](linting.json)

<!-- ============================================================ -->

## Declaration order

Related property declarations should be grouped together following a logical order:

1. Positioning
1. Box model
1. Typographic
1. Visual

Positioning comes first because it can remove an element from the normal flow of the document and override box model related styles. The box model comes next as it dictates a component's dimensions and placement.

Everything else takes place inside the component or without impacting the previous two sections, and thus they come last.

For a complete list of properties and their order, please see [Field CSS Manuals]( http://manuals.gravitydept.com/code/css/properties).

```sass
.declaration-order {
  // Positioning
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  z-index: 100;

  // Box-model
  display: block;
  float: right;
  width: 100px;
  height: 100px;

  // Typography
  font: normal 13px "Helvetica Neue", sans-serif;
  line-height: 1.5;
  color: #333;
  text-align: center;

  // Visual
  background-color: #f5f5f5;
  border: 1px solid #e5e5e5;
  border-radius: 3px;

  // Misc
  opacity: 1;
}
```

<!-- ============================================================ -->

## Prefixed properties

PatternFly uses [Autoprefixer](https://github.com/postcss/autoprefixer) to deal with CSS vendor prefixes.

**Do not add vendor prefixes**

```sass
// Bad
.selector {
  -webkit-box-shadow: 0 1px 2px $color;
          box-shadow: 0 1px 2px $color;
}

// Good
.selector {
  box-shadow: 0 1px 2px $color;
}
```

<!-- ============================================================ -->

## Single declarations

To improve error detection in instances where a rule set includes only one declaration, remove line breaks for readability and faster editing. Any rule set with multiple declarations should be split to separate lines.

```sass
// Single declarations on one line
.span1 { width: 60px; }
.span2 { width: 140px; }
.span3 { width: 220px; }

// Multiple declarations, one per line
.sprite {
  display: inline-block;
  width: 16px;
  height: 15px;
  background-image: url(../img/sprite.png);
}

.icon           { background-position: 0 0; }
.icon--home      { background-position: 0 -20px; }
.icon--account   { background-position: 0 -40px; }
```

<!-- ============================================================ -->

## Shorthand notation

Strive to limit use of shorthand declarations to instances where you must explicitly set all the available values. Common overused shorthand properties include:

- `padding`
- `margin`
- `font`
- `background`
- `border`
- `border-radius`

```sass
// Bad
.element {
  margin: 0 0 10px;
  background: red;
  background: url("image.jpg");
  border-radius: 3px 3px 0 0;
}

// Good
.element {
  margin-bottom: 10px;
  background-color: red;
  background-image: url("image.jpg");
  border-top-left-radius: 3px;
  border-top-right-radius: 3px;
}
```

The Mozilla Developer Network has a great article on [shorthand properties](https://developer.mozilla.org/en-US/docs/Web/CSS/Shorthand_properties) for those unfamiliar with notation and behavior.

<!-- ============================================================ -->

## Comment and Organization

Code is written and maintained by people. Ensure your code is descriptive, well commented, and approachable by others. Great code comments convey context or purpose. Do not simply reiterate a component or class name.

Be sure to write in complete sentences for larger comments and succinct phrases for general notes.

Follow this comment structure:

1. License header
1. DocBlock
1. Sections
1. Line


```sass
/**
* Copyright 2016 Red Hat, Inc
* Licensed under the Apache License, Version 2.0 (the "License");
* you may not use this file except in compliance with the License.
* You may obtain a copy of the License at
*
*     http://www.apache.org/licenses/LICENSE-2.0
*
* Unless required by applicable law or agreed to in writing, software
* distributed under the License is distributed on an "AS IS" BASIS,
* WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
* See the License for the specific language governing permissions and
* limitations under the License.
*/



//
// Component heading
// --------------------------------------------------
//  Sometimes you need to include optional context for the entire component. Do that up here if it's important enough.


// Section level Comment
.selector {
  line-height: 1.5; // Line level Comment
  color: #333;
}
```

### 1. License header

PatternFly is license under the [Apache License, Version 2.0](https://www.apache.org/licenses/LICENSE-2.0).

- Always add the Apache v2.0 license header at the top of each scss files.
- Leave three blank lines bellow.

### 2. DocBlock

- Includes the name of the component and an optional comment.
- Leave three blank lines bellow.

### 3. Section

The Section comment is a section divider or describes a block of code.

- Leave one blank lines above.

### 4. Line

Describes a specific line of code.


<!-- ============================================================ -->

## Naming Selectors

Create names with useful or specific meaning via a mechanism that will not inhibit your and your team’s ability to recycle and reuse CSS.


- Avoid excessive and arbitrary shorthand notation, e.g., `.btn` is useful for button, but `.s` doesn't mean anything.
- Keep classes as short and succinct as possible.
- Use meaningful names, use structural or purposeful names over presentational.
- Use `.js-*` classes to denote behavior (as opposed to style), but keep these classes out of your CSS.
- Apply these same rules when creating Sass variable names.

```sass
// Bad
.t { ... }
.red { ... }
.header { ... }

// Good
.tweet { ... }
.card { ... }
.twee__header { ... }
```

### Namespace

To avoid conflicts PatternFly prefixes all classes with “pf-”. This also helps differentiates PatternFly and Bootstrap class names.

```sass
// Bad
.selector {
  ...
}

// Good
.pf-selector {
  ...
}
```

### IDs and classes

Always use classes, avoid using an ID. Use classes over generic element tag for optimum rendering performance.

```sass
// Bad
.pf-card {
  color: #000;

  h2{
    font-size: 2em;
  }
}

// Good
.pf-card { color: #000; }

.pf-card--title{ font-size: 2em; }

```


### BEM

PatternFly follows the [BEM methodology](http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/). It's a way to decouple CSS from HTML, and modularise class names so they can be extended.

Class name structure follows the `{{pf-block}}__{{element}}--{{modifier}}` structure:

```sass
.pf-block__element--modifier {}
```

Example:

```html
<div class="pf-panel pf-panel--collapsible">
  <div class="pf-panel__title">
    <h1>Heading</h1>
  </div>

  <div class="pf-panel__content">
    <p>Lorem ipsum dolor sit amet.</p>
  </div>
</div>
```

```sass
.pf-panel {}                      // Block
.pf-panel--collapsible {}         // Modifier of block

.pf-panel__title {}               // Element

.pf-panel__content {}             // Element
.pf-panel__content--unpadded {}   // Modifier of element
```

**Why it’s better**

- All the selectors have same specificity.
- Every element is defined via a block.
- Every modifier is defined via a block or element.
- Each class name imparts structural info without binding to exact HTML.



<!-- ============================================================ -->

## Specificity

Always keep selectors as shallow as possible.

```sass
// Bad
.header .search-box input[type=search] + .button {}

.pf-search-box {
  .btn {}
}


// Good
.pf-search__button {}
```

### Nesting

Avoid Sass nesting, but if you must do it follow the [inception rule](http://thesassway.com/beginner/the-inception-rule) and never go more than three layers deep.

Limit nesting to the following use cases:

1. Media queries
1. Parent selectors
1. States, pseudo-classes and pseudo-elements
1. Overwrite Bootstrap


For longer style blocks don't nest the modifier code as it reduced the legibility of the code.

#### 2. Media queries

Component-specific media queries should be nested inside the component block. Use [Bootstrap responsive breakpoints mixins](http://v4-alpha.getbootstrap.com/layout/overview/#responsive-breakpoints) and remember that PatternFly is mobile first.

**Do progressive enhancement, not gracefully degrade.**


```sass
.pf-nav {
  ...

  // Styles for small view ports and up
  @include media-breakpoint-up(sm) { ... }
}
```

#### 3. Parent selectors

Make use of [Sass’s parent selector](https://css-tricks.com/the-sass-ampersand/ mechanism. This allows all rules for a given component to be maintained in one location.

```sass
.pf-primary-nav {
  ...

  // Nav appearing in header: Right-align navigation when it appears in the header
  .pf-header & {
    margin-left: auto;
  }
}
```

All styles for `.pf-primary-nav` should be found in one place, rather than scattered throughout multiple partial files.

#### 4. States, pseudo-classes and pseudo-elements

States of a component should be included as a nested element. This includes hover, focus, and active states:

```sass
.btn {
    background: blue;

    &:hover, &:focus {
        background: red;
    }
}
```

#### 5. Overwrite Bootstrap

Keep PatternFly code DRY. Reuse as much as you can from Bootstrap.

**To overwrite Bootstrap:**

- Create a new `pf-` block element to live beside the Bootstrap block instead of a modifier.
- Be precise and accurate, introduce as little modifications as possible to achieve the design.
- Base your styles on the original bootstrap scss file.
- Bootstrap uses a modified version of BEM: `{{block}}-{{element}}-{{modifier}}`.

```sass
// PatternFly card block
.pf-card {
  border: none;

  // Overwrite Bootstrap card header element
  .card-header{
    background: $pf-card-cap-bg;
    border-bottom-color: $pf-card-border-color;
  }

  // Overwrite Bootstrap card footer element
  .card-footer{
    background: $pf-card-footer-bg;
    border-top-color: $pf-card-border-color
  }
}

// PatternFly card modifier
.pf-card--accented {
  border-top: 2px solid $pf-color-blue-300;
}    
```

### !important

Never use `!important` to raise the specificity of a rule. In well architected CSS this should never be required. If you think it is, rewrite the rules being inherited with high specificity that are causing problems.

```sass
// Bad
.some-form {
  color: #000 !important;
}
```



<!-- ============================================================ -->

## Spaces

Always use PatternFly spacing variables to define `margin` and `padding`

```sass
$pf-spacer-xxxs   // ~3px
$pf-spacer-xxs    // 7px
$pf-spacer-xs     // ~10px
$pf-spacer-sm     // 14px
$pf-spacer-md     // ~17px
$pf-spacer-lg     // 21px
$pf-spacer-xl     // ~24px
$pf-spacer-xxl    // 28 px
$pf-spacer-xxxl   // 35 px
$pf-spacer-xxxxl  // 42 px
```

```sass
.nav-link {
  padding-bottom: $pf-spacer-xxxs;
}

.dropdown-menu {
  margin-left: $pf-spacer-sm;
}
```

You can also use [Bootstrap spacing utilities](http://v4-alpha.getbootstrap.com/utilities/spacing/) with PatternFly sizes:

```html
<p class="mb-lg">This paragraph has a large bottom margin</p>
```

<!-- ============================================================ -->

## Shadows

PatternFly has 6 types of shadows, always variables to apply `box-shadow`:

```sass
$pf-box-shadow-sm
$pf-box-shadow-md
$pf-box-shadow-lg
$pf-box-shadow-inset
$pf-box-shadow-left
$pf-box-shadow-right
```

```sass
.btn {
  box-shadow: $pf-box-shadow-sm;
}
```

You can also use box shadow utilities to add or remove shadows to any element:

```html
<p class="pf-box-shadow-md">This paragraph has a medium box shadow</p>

<button class="btn btn-primary pf-box-shadow-none">This button has no shadow</button>
```

<!-- ============================================================ -->

## Gradients
PatternFly has one slight gradient to define panel headers, table headers, secondary btns and other elements. Always use the PatternFly gradient mixin to define it.

```sass
thead th {
  @include pf-gradient;
}
```

<!-- ============================================================ -->


## Sass

PatternFly uses [Sass](http://sass-lang.com/) to preprocess CSS.

We limit our use of Sass to:

- variables
- mixins
- color functions
- math functions
- nesting

Please don't use loops or conditional statements unless absolutely necessary.

### Operators in Sass

For improved readability, wrap all math operations in parentheses with a single space between values, variables, and operators.

```sass
// Bad
.element {
  margin: 10px 0 @variable*2 10px;
}

// Good
.element {
  margin: 10px 0 (@variable * 2) 10px;
}
```

### Variables

A new variable should be created only when all of the following criteria are met:

- the value is repeated at least twice
- the value is likely to be updated at least once
- all occurrences of the value are tied to the variable (i.e. not by coincidence).
- variable format should follow the `$component-modifier-state-property` order.

There is no point declaring a variable that will never be updated or that is only being used at a single place.

**Don't reinvent the wheel:** Always use variables for spaces, colors, shadows and typography treatment.


### Mixins

The rule of thumb is that if you happen to spot a group of CSS properties that always appear together for a reason (i.e. not a coincidence), you can put them in a mixin instead.

Another valid example would be a mixin to size an element, defining both width and height at the same time. Not only would it make the code lighter to type, but also easier to read.

The keyword for using mixins is **simplicity**:

- Don't over-engineer
- Don’t over think your code
- above all keep it simple.

If a mixin ends up being longer than 20 lines or so, then it should be either split into smaller chunks or completely revised.


```sass
// Helper to size an element
// @param {Length} $width
// @param {Length} $height

@mixin size($width, $height: $width) {
  width: $width;
  height: $height;
}
```

### @extend

Treat @extend with respect. Use @extend only for maintaining relationships within selectors. If two selectors are characteristically similar, that is the perfect use-case for @extend. If they are unrelated but share some rules, a @mixin might suit you better.


- Stick to extending placeholders, not existing CSS selectors. Use extend on %placeholders primarily, not on actual selectors.
- Extend a placeholder as few times as possible in order to avoid side effects.
- When extending classes, only extend a class with another class, never a complex selector.
- Directly extend a %placeholder as few times as possible.
- Avoid extending general ancestor selectors (e.g. .foo .bar) or general sibling selectors (e.g. .foo ~ .bar). This is what causes selector explosion.

```sass
%button {
  display: inline-block;
  // … button styles

  // Relationship: a %button that is a child of a %modal
  %modal > & {
    display: block;
  }
}

.button {
  @extend %button;
}

// Good
.modal {
  @extend %modal;
}

// Bad
.modal {
  @extend %modal;

  > .button {
    @extend %button;
  }
}
```

# Credits and references

This guide is inspired by people we follow and respect:

- **[Mark Otto](http://markdotto.com/):** [Code Guideline](http://codeguide.co/)
- **[Robert Harris](http://csswizardry.com/):** [CSS Guidelines](http://cssguidelin.es/)
- **[Gravity Department](http://gravitydept.com/)**: [Style Guide Field Manual](http://manuals.gravitydept.com/code/css/style-guide)
- **[Hugo Giraudel](http://hugogiraudel.com/):** [SASS Guidelines](https://sass-guidelin.es/)

Thank you :heart:
