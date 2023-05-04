# Rendering Logic II

Alternative to Flow Layout is **Positioned Layout**

It is opted-into using the `position` property.

There are 4 values: `relative`, `absolute`, `fixed`, and `sticky`.

The default value for `position` is `static`. Which means the element is _not_ using positioned layout, and instead is using Flow, Flexbox, or Grid.

## Relative Positioning

`position: relative`

This value does two things:
1. Constrains certain children
2. Enables additional CSS properties to be used

When opting into positioned layout, we enable some new CSS properties like `top`, `bottom`, `left`, and `right`.

These directional values shift an element around _relative to its natural position_.

`top` and `left` are most common for pushing elements along vertical and horizontal axes respectively. `left: 10px` pushes the element 10px to the right.

Next values also work, `left: -10px` is equivalent to `right: 10px`.

**position doesn't impact layout**, unlike properties such as `margin`.

Relative positioning can be applied to both _block and inline_ elements. 

## Absolute Positioning

Absolute positioning, `position: absolute` takes an element out of flow and positions it based on its bounding container using the positional css properties from relative positioning. 

The `inset` property can be used to set all four directional properties simultaneously. It is useful for the absolute positioning centering trick. 

## Containing Blocks

Absolute elements can only be contained by other elements using positioned layout.

If an absolute element does'nt have a positioned ancestor, then it is positioned according to the initial containing block. This is the box the size of the viewport

Absolute elements ignore the padding of their containing block.

## Stacking Contexts

In flow layout content always shows up on top, so if elements are overlapping, content will be written over other elements background details. But in positional layout, things are layered correctly.

Positioned elements always render on top of non-positioned ones. 

Positioned elements are rendered in DOM order.

`z-index` can be used to manipulate DOM order when elements are positionally overlapping.

Stacks have contexts though that is determined by a couple of rules:
- Setting `opacity` to a value less than `1`
- Setting `position` to `fixed` or `sticky`
- Applying `mix-blend-mode` other than `normal`
- Adding a `z-index` to a child inside of `display: flex` or `display: grid` container
- Using `transfer`, `filter`, `clip-path`, or `perspective`
- Explicitly creating a context with `isolation: isolate`

## Fixed Positioning

Fixed positioning can only be contained by the viewport. It does not care about containing blocks. They are immune to scrolling.

Properties `transform`, `filter`, and `will-change` all will lock fixed elements within them.

## Overflow



## Sticky Positioning

## Hidden Content
