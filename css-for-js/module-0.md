# Module 0

## Anatomy of a style rule

- **property** is the attribute you can specify values for, i.e. `color` or `font-size`
- **selector** is the descriptor that targets specific elements on the page, i.e. `.button`
- **declaration** - the combination of a property and a value, i.e. `padding: 32px;`
- **rule** - aka **style** is the collection of declarations and the selector(s). A stylesheet can have multiple rules.
- **unit** - a collection of special things that go with certain values, i.e. `px` `%` or `em`

## Media Queries

Allow us to apply different CSS styles in different scenarios - powers _responsive design_

Uses `@media` syntax:

```css
@media (condition) {
    /* Some CSS that'll run if the condition is met */
}
```

For the _condition_, `max-width` is used to add styles on small screens, and `min-width` to add styles on larger screens.

When used in a media query condition, `max-width` and `min-width` should not be confused with their similar CSS declaration. In this context, they are **media features**.

## Selectors

### Pseudo-classes

Pseudo-classes: `:hover`, `:focus`, `:checked`, `:first/last-child`, `:first/last-of-type`

Useful for applying styles based on state of a selected element

### Pseudo-elements

Targets "sub-elements" within an element

For example, `::placeholder` targets the placeholder text in a form input 

```html
<input type="text" placeholder="abc" />
```

```css
input::placeholder {
    color: red;
}
```

Other useful ones include `::before` and `::after` for adding content

```html
<p>Hello, World!</p>
```

```css
p::before {
    content: '→ ';
}
p::after {
    content: ' ←';
}
```

These pseudo-elements are added **inside the element**, right before and after the element's content. The above example would be rendered like:

```html
<p>
    <span>→ </span>
    Hello, World!
    <span>← </span>
</p>
```

### Combinators

Example:

```css
nav a {
    color: red;
}
```

This selector uses a space character to combine the `nav` and `a` selectors into a **descendant selector** meaning it will only match `a` elements within `nav` elements no matter their deepness.

The `>` character can be used to indicate **child selector**, so `nav > a` means to only match `a` elements direct children of `nav` tags.

```html
<nav>
    <a>I'm selected!</a>
    <div>
        <a>I'm not</a>
    </div>
</nav>
```

## Color

Common value format is hex code: `#FF0000` where each 2 character represent a hex digit (00-FF). Order goes Red Green Blue, so `FF0000` would be equivalent to `red`

`hsl()` format stands for hue saturation lightness. it takes 3 arguments (h, s, and l) where `h` uses the unit `deg` and `s` and `l` use unit `%`: `hsl(0deg 100% 50%)` would be `red`

Certain formats allow specifying the _alpha channel_ which represents transparency. `1` (default) is opaque, and `0` is invisible. Here is how to use it in hsl: `hsl(0deg 100% 50% / 0.5)` 

## Units

Most popular unit is the pixel `px`

`em` is a _relative unit_, equal to the font size of the current element.

An element with a font-size of 24px, and a padding of `2em`, would result in a padding of `48px` (2 x 24px).

`rem` is similar to `em` but it is **relative to the root element, the `<html>` tag**. By setting a font-size on the `html` tag, everything else can reference its size off of that value:

```css
html {
    font-size: 4px;
}

h1 {
    font-size: 2rem; /* = 8px */
}

h2 {
    font-size: 1rem; /* = 4px */
}
```

Percentages are often used with width and height, and will consume portions of the available space within a container. The container must have a fixed size in order for percentages to be used.

## Typography

### Font families

Can change font using `font-family` property.

Fonts come in different styles, most common are `Serif` and `Sans-serif`. Serifs are the little adornments at the edge of strokes of a character.

We can instruct systems to use their default value for a certain style category by specifying the value `serif`, `sans-serif`, or `monospace`.

### Web fonts

A custom font we load in from CSS. Allows for use of any font on a website. We can load fonts using html `<link>` tags and then specify the name of the font like so: `font-family: 'Roboto'`

### Typical Text Formatting

There are 3 main formatting options: bold, italic, and underline.

Bold text can be created using `font-weight` property. The values can be keywords like `bold` or `light`, or numbers 1 to 1000 like `400` or `800`.

Italic text can be created using `font-style: italic`

Underlined text can be created using `text-decoration: underline`

While CSS allows us to change the cosmetic presentation of text, it doesn't affect the semantic meaning (which is especially important for accessibility as they wont see the CSS styles necessarily).

`<strong>` tag is meant to convey that an element is critically important

`<em>` tag is used for emphasis, like within a paragraph of text

### Text Alignment

The `text-align` property allows us to shift characters horizontally. Values are `left`, `right`, `center`.

### Text Transforms

The `text-transform` property allows us to transform text into `uppercase` or `lowercase` or even `capitalize` (The First Letter Of Every Word)

### Spacing

`letter-spacing` property tweaks the horizontal gap between characters

`line-height` property tweaks the vertical gap between lines

> To comply with accessibility guidelines, body text should have a minimum line-height of 1.5
