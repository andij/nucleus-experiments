---
title: Spacing
layout: markdown
---

# Describing our current implementation
 
We have two spacing scales, and we use two relative units `em` and `%`.

The two spacing scales are  **Fractions** and **Modular scale**.

## Fractions

Using the relative values of `em` and `%` allows us to design in fractions.

```
1.0em    = one
0.75em   = three quarters
0.5em    = half
0.25em   = one quarter
0.125em  = one eighth
0.0625em = one sixteenth
```

### Using percentages

Move something by a third.

```
transform: translateX(33.3%);
```

## Use `em` to empower the browser to do the calculations for us

In a number of our components including `<ns-cta>` and `<ns-inputter>` we have used `em` with a CSS 'one-liner' to perform the scaling for us in the browser.

The design of our component is created for the smallest viewport using 1em (16px) as the base-scale. All CSS is written for the component outside of any media queries.

To adjust the scale of the component for the larger viewports we introduce a media query redefining the value of the base `em`. This will proportionally scale the component ensuring the integrity is retained and the relationships between font-size, border-radius, padding, margins etc.

### Example LESS

```less
// all the styles are defined outside of any media query
.component {
  font-size: 1em; // set the base font-size, we can call this small
  padding: 0.5em; // use the em unit for padding, margin, border-radius etc.

  // all the other styles required for the component
}

// within the media queries, there is a single line to specify the scale
.responsive-min(@basketball-court, {
  .component {
    font-size: 1.125em; // medium
  }
});

.responsive-min(@hockey-pitch, {
  .component {
    font-size: 1.25em; // large
  }
});

// we can add a media query and scale the component to be smaller than the base if we want
.responsive-max(@squash-court, {
  .component {
    font-size: 0.8em; // tiny
  }
});

```

### Example

Notice the relationship of the font-size, spacing, border-radius and icon are retained. Making this design very robust.

![demo](https://user-images.githubusercontent.com/28779/103764856-237e9b80-5014-11eb-8b22-3e2a801626c1.png)

### Benefits

There is one set of CSS defined for the component and minimal CSS for the different viewports.

It's highly scalable. Any size of the component can be produced at any desired point. It's even possible to have small / medium / large within the same viewport using this technique.

If any specific changes to the design are desired for a particular viewport they can be included in the the relevant media query. Doing this makes it very easy to identify responsive design decisions.

The weight of the compiled CSS file size will be smaller than defining absolute values for every component and each viewport, keeping our performance high.

If we were to introduce a new viewport and require each component to be scaled accordingly the task of scaling each component is simple and obvious. Say we want to provide tiny sizes for smart watches, it's potentially adding one line per component.

### Drawbacks

Browsers calculate relative values in slightly different ways, so there maybe a different result between browsers.

Nesting components within other components. Where we use this technique it can cause undesirable scaling issues. This can be mitigated if we leverage the `rem` unit for nested components.

This technique is limited to use on the web, native apps currently use only absolute units. This doesn't stop us from creating design tokens with absolute units for these native web apps to use.

## Modular scale

Modular scale is used predominately for margin and padding for boxy shapes like `ns-card`. It's also used for the size of assets like the illustrations.

Vertical and horizontal spacing in `.splash .triple` and `ns-panel`.

And of course Typography.

### LESS mixin

```less
.ms(@number, @bases: 2, @ratio: @major-second, @unit: em) when (isnumber(@number)) {
  @calcRatio: pow(@ratio, @number / @bases);
  @modular: unit((@calcRatio), @unit);

  @result: @modular;
};
```

### The three scales that have been chosen for British Gas

```less
@major-second: 1.125;
@minor-third: 1.2;
@major-third: 1.25;
```

### A current example of its use for margin

The value of **6** is constant for the margin-top across all viewports, the ratio (mulitplier) is different resulting in relevant spacing for small / medium / large displays.

```less
// squash-court and tennis-court
margin-top: .ms(6, @ratio: @major-second) [@result]; // Produces: margin-top: 1.42383em;

// basketball-court and hockey-pitch
margin-top: .ms(6, @ratio: @minor-third)[@result]; // Produces: margin-top: 1.728em;

// rugby-pitch and polo-field
margin-top: .ms(6, @ratio: @major-third)[@result]; // Produces: margin-top: 1.95312em;
```

### Dividing and multiplying the modular scale

In this example we calculate the size of the illustration on `<ns-card type="section">` by multiplying the scale by 4.

```less
width: ((.ms(6, @ratio: @ns-foundations-scale-small) [@result]) * 4);
height: ((.ms(6, @ratio: @ns-foundations-scale-small) [@result]) * 4);
```

### Step one towards using tokens for modular scale

```less
margin-top: .ms(6, @ratio: @ns-foundations-scale-small) [@result];

margin-top: .ms(6, @ratio: @ns-foundations-scale-medium)[@result];

margin-top: .ms(6, @ratio: @ns-foundations-scale-large)[@result];
```

### We can now map the design tokens for British Gas

```less
@ns-foundations-scale-small = @major-second

@ns-foundations-scale-medium = @minor-third

@ns-foundations-scale-large = @major-third
```

### And something else for another brand

```less
@ns-foundations-scale-small = 1.111

@ns-foundations-scale-medium = 1.222

@ns-foundations-scale-large = 1.333
```

## `variables/spacing.less`

Here is an example of existing spacing mixins and their output.

**These have not been used across any of our components.**

```less
@base-space: 1em;

.margin(@index) {
  margin: (@index * @base-space);
};

.margin(@index, @type) {
  margin-@{type}: (@index * @base-space);
};

.padding(@index) {
  padding: (@index * @base-space);
};

.padding-y(@index) {
  padding: (@index * @base-space) 0;
};

.padding-x(@index) {
  padding: 0 (@index * @base-space);
};

.padding(@index, @type) {
  padding-@{type}: (@index * @base-space);
};
```

## Results

When we set `@base-space: 1em;` the results are:

| Mixin | Output |
| :--- | :--- |
| `.margin(3);` | `margin: 3em;` |
| `.margin-x(3);` | `margin: 0px 3em;` |
| `.margin(3, left);` | `margin-left: 3em;` |
| `.margin(0);` | `margin: 0px;` |
| `.padding(3);` | `padding: 3em;` |
| `.padding-x(3);` | `padding: 0px 3em;` |
| `.padding(3, left);` | `padding-left: 3em;` |
| `.padding(0);` | `padding: 0px;` |

## Concepts

Very brief notes about potential future implementation of scale.

```less
margin-left: spacing(@large);

padding-right: getSpacing(3);
```
