# Module 1 - Rendering Logic 1

Browsers have default stylesheets that apply a number of default styles to html elements.

It shouldn't necessarily be overwritten, but Josh shared his recommended "global" stylesheet: https://courses.joshwcomeau.com/css-for-js/treasure-trove/010-global-styles

## Cascade

When multiple rules are applied to one element, the browser uses _specificity_ of a selector to determine which rules get applied.

The order from least to greatest specificity is:
- inherited styles (color property)
- tag styles (`tag`)
- class styles (`.class`)
- id styles (`#id`)
- important styles (`!important`)

## Block and Inline Directions

CSS has a **block direction (vertical)** and an **inline direction (horizontal)**.

Logical Properties `margin-block-start`, `margin-block-end`, `margin-inline-start`, and `margin-inline-end` utilize the logical direction of a browser in order to apply its value. In a browser with left-to-right language direction, `margin-inline-start` would add spacing to the right instead of the left.

Logical properties are important for internationalization such that a site could be viewed in both English and Arabic. 

## The Box Model

Four Aspects:
1. Content
2. Padding
3. Border
4. Margin

A helpful analogy is a person out for a winter walk in a big poofy coat:
- The _content_ is the person themselves, underneath the coat
- The _padding_ is the stuffing in the coat. The more stuffing there is, the more poofed-up the coat will be, and the more space the person will take up.
- The _border_ is the material of the coat. It has a thickness and a color, and it affects the person's appearance.
- The _margin_ is the person's "personal space".

### Box Sizing

The default value for `box-sizing` is `content-box` which only takes the inner content into account for size calculations.

Example:

```html
<style>
section {
    width: 150px;
}
p {
    width: 100%;
    padding: 16px;
    border: 2px solid;
}
</style>
<section>
    <p>Hello, World!</p>
</section>
```

In the code block, the resulting `p` total width is calculated as `186px` because the `width: 100%` declaration in conjunction with `box-sizing: content-box` default declaration, results in the _content_ of the `p` to be `150px` (100% the size of its parent). This value is then added to the remaining aspects of `p`, both the left and right side padding and border, to come up with final calculated size of `186px`. By changing the `box-sizing` value to `border-box`, the `p` total width is calculated as `150px` because it instructs the browser to include the _padding_ and _border_ aspects into the given calculation. The browser subtracts the left and right side padding and border from the `150px` of available space, and sets the content width to that value; thus, resulting in a total size of `150px`.

As shown in Josh's global stylesheet, the `box-sizing` model is a bit more intuitive, and so it can be made the new default using:

```css
*,
*::before,
*::after {
    box-sizing: border-box;
}
```

### Padding

The "inner space" of an element.

Can be assigned all at once using `padding`, individual directions with `padding-(top/bottom/left/right)`, and using logical properties `padding-(block/inline)-(start/end)`.

Can use a wide range of units, most common are `px`, `em`, and `rem`.

You can use percentages too, but its not intuitive. **Percentages are always calculated based on the element's available width**.

The `padding` property can be used to set asymmetric padding in a few different ways:
- The overall algorithm is that it applies values 1 through 4 in the order top, right, bottom, left (like a clock). When less than 4 values are passed, it will mirror values to the non-specified directions.
- One value, mirrored to all directions
- Two values, first value is top (and mirrored to bottom), second value is right (and mirrored to left)
- Three values, first is top, second right (mirrored to left), and third is bottom.
- Four values, first is top, second right, third bottom, forth left.

> This pattern is shared amongst other CSS properties like `margin`, `border-style`, and more.

#### Overwriting values

```css
.box {
    padding: 48px 48px 0;
}

/* can also be written as */
.box {
    padding: 48px;
    padding-bottom: 0;
}
```

"long-form" properties can overwrite the relevant value in shorthand properties. Order matters! Having `padding-bottom` first will not have any affect.

### Border

Border has three specific styles
1. Border width
2. Border style
3. Border color

They can be combined into a shorthand:

```css
.box {
    border: 3px solid hotpink;
}
```

The only **required** field is `border-style`. Without it, no border will be shown. Width defaults to `3px` and color to the font color.

```css
.box {
    border: solid;
}
```

Akin to `padding`, you can overwrite long form properties with specific ones. Order matters!

```css
.box {
    border: 10px solid;
}
.box.one {
    border-color: deeppink;
}
.box.two {
    border-color: gold;
}
```

Remember that border color defaults to the font's color. This can be useful when needing to synchronize styles

```css
.box {
    border: 4px solid;
}
.box.pink {
    color: hotpink; /* since `.box` does not specify the border-color, the .box.pink will get a border color of hotpink because it defaults to the font color */
}
```

To specify this explicitly, the `currentColor` keyword can be used.

#### Border radius

The `border-radius` property rounds an element, even if it has no border.

Like `padding`, the property accepts values for each direction (clock-like pattern), but unlike `padding` it's focussed on the corners not sides. So the order goes top-left, top-right, bottom-right, bottom-left.

#### Border vs Outline

`outline` property doesn't affect layout. It is a cosmetic effect draped over an element.

Outlines have a special `outline-offset` property that adds a gap between the element and the outline.

They are generally used as visual indicators for accessibility purposes. They shouldn't be modified for interactive elements like buttons or links.

### Margin

Margin increases the space _around_ an element. The syntax is exactly like padding.

```css
.spaced-box {
  margin: 20px;
}

.asymmetrically-spaced-box {
  margin: 20px 10px;
}

.individually-specified-box {
  margin-top: 10px;
  margin-left: 20px;
  margin-right: 30px;
  margin-bottom: 40px;
}
.logical-box {
  margin-block-start: 20px;
  margin-block-end: 40px;
  margin-inline-start: 60px;
  margin-inline-end: 80px;
}
```

With `padding` and `border`, only positive numbers are supported. With `margin`, negative numbers are also supported.

Margin is about changing the _gap between elements_.

Auto margins can be used to center a child in a container.

The `auto` value seeks to fill the **maximum available space**. It works the same way for the `width` property.

It only works for **horizontal margin**. Setting top/bottom margin to `auto` is equivalent to setting it to `0px`.

It also only works on elements with an explicit width. Block elements will naturally grow to fill the available horizontal space so we need to provide the element with a width in order to center it.

## Flow Layout

Every HTML element will have its layout calculated by a layout algorithm. Known as "layout modes", there are 7 distinct ones. Flow layout is the default layout mode. 

Flow layout is like the "Microsoft Word" layout algorithm. It's intended to be used for document type layouts.

Two main element types in Flow Layout:
- **block element**: things like headings, paragraphs, footers, asides. The chunks of content that make up a page.
- **inline elements**: things like links, bold text. Generally, inline elements are meant to highlight bits of text or elements within a block container.

Each HTML tag has a default type. `<div>` and `<header>` are block elements, `<span>` and `<a>` are inline elements.

The layout is controlled by the `display` property, with values `inline` and `block`.

In flow layout, `block` elements stack in the block direction (vertical), and `inline` elements stack in the inline direction (horizontal).

### Inline Elements

Inline elements don't like messing things up. They "go-with-the-flow". Inline elements can be shifted in the inline direction (`margin-left` and `margin-right` for example), but you can't give it a `width` or `height`.

There is an exception to this rule, _replaced elements_. A replaced element is one that embeds a "foreign" object. This includes `<img />`, `<video />`, `<canvas />`. They are all technically inline, but special. They can affect block layout. You can give them explicit dimensions and modify their block direction properties like `margin-top`. This is reconciled by pretending it's a foreign object **within an inline wrapper**. When you modify its dimensions, you are modifying the object itself, meanwhile the inline wrapper still goes with the flow.

Inline elements have "magic space" because browsers automatically treat it as typography (or rather default styling to support typography, the main inline element). This causes issues with images. There are workarounds though. Setting images to `display: block;` or setting `line-height: 0` on the wrapping div.

Furthermore, HTML is space-sensitive (most of the time). It can't differentiate between text and image inline elements. So when there are multiple `<img />` elements next to each other, literal whitespace characters will be rendered. This is a specific issue to flow layout and can be avoided by switching to another layout system like Flexbox.

Inline elements have the ability to line-wrap. This means they can create shapes other than boxes (block elements always are just rectangles). This helps explain why certain css properties aren't available for inline elements. How would you apply vertical margin to text that line wraps?

### Block Elements

Block level elements will greedily extends to fill the entire available horizontal space.

For example, a heading might only need 150px to contain its content, but if placed in an 800px container, it will expand to fill the 800px of width. There is a special width keyword, `fit-content`, that will shrink it down to the only the space necessary, but it will remain in the `block` layout mode where the next element will be stack below it instead of next (even though there appears to be space).

Block elements **do not** want to share any inline space.

### inline-block

There is one more `display` value, `inline-block`, to consider. It is a block-level element that can be placed in an inline context. In regards to layout, it is treated as an inline element. But internally, it acts like a block element. Normally, properties like `width` or `margin-top` don't work on inline elements, but inline-block elements can use these properties.

Inline-block elements **do not** line-wrap.

### Width Algorithms

When using percentage-based widths, the percentages are based on the parent element's content space. If the `body` tag makes 400px of space available, any child with 100% width will become 400px wide.

Block elements have a default `width` of `auto`, not `100%`. This works similar to `margin: auto`; it will grow as much as its able to. So when something like a `h1` has margin, its width will be calculated as `100% - margin`.

#### Keyword values

- `min-content` - specifies the element to become as narrow as it can, based on the child contents. It will pick the smallest space necessary to contain the children and that is the width it will use (and will force text to line wrap).
- `max-content` - the element's width will be the smallest value that contains the content, without breaking it up (no line-breaks). It does not care about constraints of the parent. It sizes purely on the length of its unbroken children.
- `fit-content` - the width is based on the size of the children. if the width can fit within the parent container, it behaves like max-content, not adding any line-breaks. If the content is too wide, it will add line-breaks as-needed to ensure it never exceeds the available space. It behaves a lot like `width: auto`.
- `min-width` and `max-width` - are constraints for the minimum and maximum width of an element.

### Height Algorithms

The default `height` of a block element is the be as small as possible while fitting all the element's content.

Percentage-based height values sometimes seem to have no effect. This is because 100% of a parent that is trying to fit it's content is invalid. This can be fixed by setting `height: 100%` on every element before the main one (i.e. `html` and `body`). This then lets the height be calculated as the viewport height.

In some React apps, the app is loaded into a container `div`. It can be confusing, but to add a height of `100%` to these, use their document IDs:

```css
html, body, #root, #__next {
    height: 100%;
}
```

Fundamentally, **width looks up the tree**, and **height looks down the tree**. This can be overwritten by specifying an explicit value such as `height: 300px;`.

## Margin Collapse

It is possible for margins to share space. If elements need 20px of margin, the gap between them could be calculated as 40px (the margin below one element, and above the other combine). Using margin collapse, we can get two elements to share the same 20px of margin so there isn't a 40px gap. Luckily, in some circumstances margin collapse is enabled by default so it is important to review the rules.

### Vertical margins collapse

Given this html:
```html
<style>
  p {
    margin-top: 24px;
    margin-bottom: 24px;
  }
</style>

<p>Paragraph One</p>
<p>Paragraph Two</p>
```

The resulting paragraphs **will share** 24px of margin; otherwise said, they will be 24px apart instead of 48px apart. 

**Vertical margins collapse**, and **horizontal margins do not**.

So a similar example as above where its `margin-right: 24px;` and `margin-left: 24px;` for `p` elements (with a `display: inline-block` too), would be separated by 48px of space.

### Only in Flow

Margin collapse only occurs in Flow Layout. So by setting `display: flex;`, margin collapse will stop happening.

### Only adjacent element collapse

```html
<p>foo</p>
<br />
<p>bar</p>
```

In code like above, the foo and bar elements will no longer have margin collapse as they are no longer adjacent to each other in the markup.

### The bigger margin wins

Given two `<p>` that have different top and bottom margins, say 10px and 20px. The margin collapse will favor the greater one spacing the two 20px apart.

### Nesting doesn't prevent collapsing

```html
<style>
  p {
    margin-top: 48px;
    margin-bottom: 48px;
  }
</style>

<div>
  <p>Paragraph One</p>
</div>
<p>Paragraph Two</p>
```

Even though the paragraphs are not siblings, the margin will be transferred to the parent div and then margin collapse will occur. 

Margins only collapse when they're touching. They can be blocked in many ways:
- padding or border
- gap (such as giving a parent excess height. margins wont collapse through the extra whitespace)
- scroll container

### Margins collapse in the same direction

Given:

```html
<style>
  .parent {
    margin-top: 72px;
  }

  .child {
    margin-top: 24px;
  }
</style>

<div class="parent">
  <p class="child">Paragraph One</p>
</div>
```

The margins will combine, and the child's 24px will be absorbed into the parent's 72px margin

### More than two margins can collapse

For example you can combine the principles:
- siblings can combine adjacent margins
- a parent and child can combine margins in the same direction

to result in a multi-collapse!

```html
<style>
header {
  margin-bottom: 10px;
}

header h1 {
  margin-bottom: 20px;
}

section {
  margin-top: 30px;
}

section p {
  margin-top: 40px;
}
</style>

<header>
  <h1>My Project</h1>
</header>
<section>
  <p>Hello World</p>
</section>
```

- The header has a margin of `10px`, and its child `h1` a margin of `20px`. These collapse into `20px`
- The section has a margin of `30px`, and its child `p` a margin of `40px`. These collapse into `40px`
- The two then collapse together into `40px`.

### Negative margins collapse too!

Same idea as positive margins, the absolute greater margin will win. As in -75px vs. -25px will result in a collapse of -75px.

With positive margins, they are added together. So -75px and 25px will result in 50px collapse.

### Multiple positive and negative margins

😵‍💫 when dealing with multiple of each margin, follow this algorithm:
- All positive margins collapse together (10px and 50px collapse into 50px)
- All negative margins collapse together (-20px and -30px collapse into -30px)
- Add those two together (50px + -30px = 20px)