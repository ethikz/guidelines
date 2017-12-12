The following document outlines a reasonable style guide for CSS development in all BB&T codebases. These guidelines strongly encourage the use of existing, common, sensible patterns.

This is a living document and new ideas are always welcome. Please contribute.

## General Principles
Only one style should exist across the entire source of your code-base. Always be consistent in your use of whitespace. Use whitespace to improve readability.

1. Don't try to prematurely optimize your code; keep it readable and understandable.
2. All code in any code-base should look like a single person typed it, even when many people are contributing to it.
3. Strictly enforce the agreed-upon style.
4. If in doubt when deciding upon a style use existing, common patterns.

### Use hyphens when naming mixins, classes, functions & variables
```scss
.span-columns {
  [...]
}
```

*NOT*

```scss
.span_columns {
  [...]
}

.spanColumns {
  [...]
}
```

### Do Not Use ID Selectors
Use generic, reusable classes
```scss
.table-selector {
  [...]
}
```

*NOT*

```scss
#all-bills-payments {
  [...]
}
```

---

## Formatting
When writing a css selector and declaration blocks, keep in mind the syntax for each code block.

### ** Use space between a selector and its opening brace.**
*Selectors should be on a single line, with a space after the last selector, followed by an opening brace. A selector block should end with a closing curly brace that is unindented and on a separate line*
```scss
selector {

}
```

### ** A blank line should be placed between each selector block.**
*Selectors should never be indented.*

```scss
selector {
  color: red;
}

selector2 {
  color: green;
}
```

*NOT*

```scss
selector {
  color: red;
}
selector2 {
  color: green;
}

  selector {
  color: red;
}
  selector2 {
  color: green;
}

  selector {
  color: red;
}

  selector2 {
  color: green;
}
```

### Multiple selectors should each be on its own line with no space after each comma
```scss
selector1,
selector2,
selector3,
selector4 {

}
```

### Add each property and value on new line with indentation

*Property-value pairs should be listed starting on the line after the opening curly brace. Each pair should:*

Each pair should:

1. Be on it's own line;
2. Be indented one level (*2 spaces*);
3. Have a single space after the colon that separates the property name from the property value;
4. End in a semicolon.

```scss
selector {
  color: red;
  text-align: center;
}
```

*NOT*

```scss
selector { color:red; text-align:center; }
```

### Use 3 digit hex colors when possible
*When denoting color using hexadecimal notation, use all lowercase letters. Both three-digit and six-digit hexadecimal notation are acceptable; if it’s possible to specify the desired color using three-digit hexadecimal notation, do so as you’ll save the end-user a few bytes of download time.  We use lowercase because it is easier to scan (e.g. `#607D8B`) vs (e.g. `#607d8b`)*

*Note: Use all lowercase when writing hexadecimal color values.*

```scss
selector {
  background: #000;    // good
  color: #fff;         // good
  border-color: #FFF;  // not good
}
```

### **Use two spaces per indentation level.**

```scss
selector {
  background: #000;
  color: #fff;
}
```

*NOT*

```scss
selector {
    background: #000;
    color: #fff;
}
```

### Avoid nesting selectors
*Nest no more than 1 or 2 deep* - Ideally, avoid nesting entirely to prevent the [CSS Specificity Wars](https://stuffandnonsense.co.uk/archives/css_specificity_wars.html).

### URLs should be valid and not contain protocols or domain names
URL links should be relative.

```scss
background: url('assets/image.png');
```
*NOT*
```scss
background: url('https://example.com/assets/image.png');
```
### ** Use single quotation marks.**
```scss
selector {
  background: url('path/to/file');
}
```

### **Avoid vendor prefixes. That is, don't write them yourself.**
```scss
selector {
  transform: all 0.3s ease;      // good
  -ms-transform: all 0.3s ease;  // not good
}
```

### ** Don't add a unit specification after 0 values, unless required by a mixin.**
*When denoting the dimensions — that is, the width or height — of an element or its margins, borders, or padding, specify the units in either em, px, or %. If the value of the width or height is 0, do not specify units.*
```scss
selector {
  width: 12px;  // good
  width: 12%;   // good
  width: 12em;  // good
  width: 12rem; // good
  width: 0;     // good
  width: 12;    // not good
  width: 0px;   // not good
}
```
### ** Use a leading zero in decimal numbers**
```scss
selector {
  font-size: 0.5em;
}
```
*NOT*
```scss
selector {
  font-size: .5em;
}
```

### ** Use space around operands**
```scss
selector {
  font-size: $variable * 1.5;
}
```
*NOT*
```scss
selector {
  font-size: $variable*1.5;
}
```
### ** Use parentheses around individual operations in shorthand declarations**
```scss
selector {
  padding: ($variable * 1.5) ($variable * 2);
}
```

### ** Use single colons for pseudo-elements**
*Although double colons are also ok to use, for best browser support, we will stick with single*

```scss
selector:before {
  padding: ($variable * 1.5) ($variable * 2);
}
```
### ** Use a % unit for the amount/weight when using SCSS's color functions**
```scss
selector {
  background: darken($color, 20%);
}
```
*NOT*
```scss
selector {
  background: darken($color, 20);
}
```

### ** When nesting, add a space above each nested selector**
```scss
selector {
  color: red;

  nested-selector {
    color: green;
  }

  nested-selector2 {
    color: green;
  }
}

```
*NOT*
```scss
selector {
  color: red;
  nested-selector {
    color: green;
  }
  nested-selector2 {
    color: green;
  }
}

```

### **Place @else statements on the same line as the preceding curly brace.**
```scss
@if {
  [...]
} @else {
  [...]
}
```

*NOT*
```scss
@if {
  [...]
}
@else {
  [...]
}
```

### **Files should always have a final newline. This results in better diffs when adding lines to the file.**

---

## Declaration Blocks
Declararion blocks are comprised of selectors and properties. Let's look at some property rules and how we should address them.

### **General Property Rule**
Do not have properties duplicated within the same declaration block.

```scss
selector {
  background: $bg-color;
  background: $content-block-color; // Not Good
  color: #fff;
}
```


### **Borders**
When removing `border:` styles from a selector, use `0;` instead of `none;`.
```scss
selector {
  border: 0;
}
```

*NOT*

```scss
selector {
  border: none;
}
```

### Colors
Colors should be defined by a [CSS Custom Property](https://developer.mozilla.org/en-US/docs/Web/CSS/--*), [Sass variable](http://sass-lang.com/documentation/file.SASS_REFERENCE.html#Variables_____variables_), or HEX and never a color name.

```scss
selector {
  color: #fff;
}

```

*NOT*

```scss
selector {
  color: white;
}
```

---

## Order
### Place `@extends` and `@mixins` at the top of your declaration list.
```scss
selector {
  @extend %placeholder;
  @include box-sixing(border-box);
  background: darken($color, 20%);
}
```

### Place media queries directly after the declaration list by smallest to largest
```scss
selector {
  @extend %placeholder;
  @include box-sixing(border-box);
  background: darken($color, 20%);

  @include tablet {
    [...]
  }

  @include desktop {
    [...]
  }
}
```

### Place concatenated selectors second
```scss
selector {
  @extend %placeholder;
  @include box-sixing(border-box);
  background: darken($color, 20%);

  @include tablet {
    [...]
  }

  @include desktop {
    [...]
  }

  &.concatenated-selector {
    background: $color;
  }
}
```

### Place pseudo states and elements third
```scss
selector {
  @extend %placeholder;
  @include box-sixing(border-box);
  background: darken($color, 20%);

  @include tablet {
    [...]
  }

  @include desktop {
    [...]
  }

  &.concatenated-selector {
    background: $color;
  }

  &:hover {
    background: $color;
  }
}
```

### Place nested selectors last
```scss
selector {
  @extend %placeholder;
  @include box-sixing(border-box);
  background: darken($color, 20%);

  @include tablet {
    [...]
  }

  @include desktop {
    [...]
  }

  &.concatenated-selector {
    background: $color;
  }

  &:hover {
    background: $color;
  }

  .nested-selector {
    [...]
  }
}
```

## Organization
### Use HTML structure for the ordering of selectors. Don't just put styles at the bottom of the SCSS file
*NOTE: If done right, Modules can easily be moved to different parts of the layout without breaking. Applying the CSS naming convention will help will the structure. [Learn More About the Naming Convention](css-naming-convention).*

### Structure media queries in element based on mobile first approach.
```scss
selector {
  @extend %placeholder;
  @include box-sixing(border-box);
  background: darken($color, 20%);

  @include tablet {
    [...]
  }

  @include desktop {
    [...]
  }
}
```

### Avoid having scss files longer than 100 lines
*Note: If your files are longer than 100 lines, try splitting the partial in multiple partials*

---

## Writing comments in .scss files
Comments are strongly encouraged. Concerned about performance? Don’t worry, scss comments are removed after compiling to css.

Sass supports standard multiline CSS comments with `/* */`, as well as single-line comments with `//`. Multiline comments are preserved in the CSS output and should only be used when needed, while the single-line comments are removed and should be used as a general way of commenting. For example:

Always refrain from using `/* ... */` comment tags when and instead use `// ...` with a single space before the ` // ` and after the ` // `.


For example:

```scss
.condition-element {
  display: none; // Please add this class when condition element is not to be seen
}

// Hey designers, we have a new background color
// and should be used for any element
// that will be using this color. Enjoy!!
.block-class {
  background: red;
}
```

Will render to:

```scss
.condition-element {
  display: none;
}

.block-class {
  background: red;
}
```

Comments that refer to selector blocks should be on a separate line immediately before the block to which they refer. Short inline comments may be added after a property-value pair, preceded with a space.

```scss
// Comment about this selector block
.selector-block {
  display: inline-block; // Comment about this property-value pair
  color: $color;
}

```

Reasons to use `/* ... */`.

1. We want to relay a message through comments to the dev team.
2. When using a third party plugin and credit is required.

Overarching titles for `.scss` partials are written using the following structure:

### Titling Sass partials using comments
Use the following format when adding titles to Sass partials.

```scss
/////////////////////
//  This Is A Title
/////////////////////
```

Subsections within those partials

```scss
// This is a Sub-Title //
```

---

### Using Mixins instead of Extends
Sass offers two ways of re-using chunks of CSS, one via `@mixin` another via `@extend` - both have their own advantages and disadvantages.

For the purposes of the Lexicon, **use mixins exclusively**. Do not use `@extend` either with [silent classes](https://csswizardry.com/2014/01/extending-silent-classes-in-sass/) or otherwise.

#### Reasoning
Mixins perform significantly better when the compiled CSS is compressed using gzip, according to [an article by Shay Howe with bellycard.com](https://tech.bellycard.com/blog/sass-mixins-vs-extends-the-data/). Though extends offer similar functionality, particularly when the mixin does not have any parameters, this performance improvement is significant enough to justify the consistent use of mixins instead of extends.

Furthermore, for consistency and predictability's sake, it will be easier to maintain the Lexicon code base if one route is chosen over the other.
