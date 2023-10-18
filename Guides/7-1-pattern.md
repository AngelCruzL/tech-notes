---
tags:
  - guide
  - css
  - frontend
---
----

This pattern allows us to separate the styles of the application based on their layers across it.

We have 7 different layers:
1. abstracts
2. base
3. components
4. layout
5. utilities
6. vendor
7. main

This is a custom architecture based on the original, where every layer has a single responsibility:

## Abstracts

As we’ve talked about a fair deal so far, abstracts are things like *variables* and *mixins* that **aren’t directly compiled to CSS**, but instead are used in other places.

I tend to separate things quite a bit here, with separate files for colors, font related things, breakpoints, and so on. I do group all my mixins into a single mixins file, and the same with my functions though.

## Base

This folder contains my *reset*, my *custom properties* (inside the `_root.scss` file), and all the more generic styling for my `body` and other simple element selectors where we can set up some general styling and let the cascade take care of things.

## Components

Every component I make gets it’s own partial file, making it very easy to find what you’re looking for.

## Layout

I try to keep my layout classes fairly generic and separated from specific components.

The idea is layout classes create a layout, and then components live within those layouts.

In here I have things like a layout with a sidebar, equal column layouts, grid systems, etc. Generally, they tend to be as generic as possible.

Once again, every layout I create gets its own partial to make it easy to find.

We’ll add a few layout classes that I use in most of my projects a little later on in the course.

## Utilities

As with my components and layout, every utility I create gets its own partial, except for things like colors and font sizes, where all of the respective classes live in the one partial file.

I also include things like my `.container` here, and a few other things that you might feel like they could go under `layout/` instead.

We’ll add some useful utilities I use on most of my projects a little later on.

## Vendor

Vendor is for 3rd party CSS, so for plugins or other things you might be bringing in. I use [prism](https://prismjs.com/) for handling syntax highlighting, so I drop their CSS file in there. This could also be the home for some 3rd party lightbox, or even something as big as Boostrap.

## `main.scss`

This is where everything gets brought in, with the exception of my abstracts.

Each folder above with have an `_index.scss`, and then I’ll have:

```
@use 'base';
@use 'components';
@use 'layout';
@use 'utilities'
@use 'vendor';
```

**The order of the imports is important**, with the order you put them here being the order the styles will be in the compiled CSS. If you’re overwriting your vendor styles (say you’re using Boostrap, and then modifying it in your own styles), then you would want to import that first.


## My general rules of thumb

When I’m authoring SCSS, I stick to these general rules of thumb:

- Start with all the styling of that element itself

- Style nested elements, generally trying to avoid nesting more than one level deep

- Media queries at the _end_ of my parent selector, including for the nested selectors

Going back to the example we saw in the previous lesson, it shows us a simple sample of this in action:

```scss
.product-card {
  padding: 0.5rem;
  border-radius: 1rem;
  box-shadow: 0 0 1em rgb(0, 0, 0, 0.2);

  &__title {
    font-size: 1.5rem;
  }

  @media (min-width: 45em) {
    padding: 0.75rem;
  }

  @media (min-width: 60rem) {
    padding: 1rem;
    display: flex;

    &__title {
      font-size: 2.5rem;
    }
  }
}
```

This converts into:

```css
.product-card {
  padding: 0.5rem;
  border-radius: 1rem;
  box-shadow: 0 0 1em rgba(0, 0, 0, 0.2);
}
.product-card__title {
  font-size: 1.5rem;
}
@media (min-width: 45em) {
  .product-card {
    padding: 0.75rem;
  }
}
@media (min-width: 60rem) {
  .product-card {
    padding: 1rem;
    display: flex;
  }
  .product-card__title {
    font-size: 2.5rem;
  }
}
```


Use cases for variables

So far, all my examples have looked at using variables for colors.

That is a great use case for them, but you can use them for a lot more!

Any time you have values that you need to use more than once, variables are a great solution. Here is incomplete list of some of the things I use them for:

- Color values

- Font sizes

- Spacing values

- Box shadows

- Gradients

- Border radii


## Variables as breakpoints

```scss
$bp-medium: 55em;
$bp-large: 75em;

.main-grid {
  display: grid;
  grid-template-areas:
    'header'
    'main'
    'sidebar-one'
    'sidebar-two'
    'footer';

  @media (min-width: $bp-medium) {
    grid-template-areas:
      'header header'
      'main sidebar-one'
      'main sidebar-two'
      'footer footer';
  }

  @media (min-width: $bp-large) {
    grid-template-areas:
      'header header header'
      'main sidebar-one sidebar-two'
      'footer footer footer';
  }
}
```

This is converted into:
```css
.main-grid {
  display: grid;
  grid-template-areas: "header" "main" "sidebar-one" "sidebar-two" "footer";
}
@media (min-width: 55em) {
  .main-grid {
    grid-template-areas: "header header" "main sidebar-one" "main sidebar-two" "footer footer";
  }
}
@media (min-width: 75em) {
  .main-grid {
    grid-template-areas: "header header header" "main sidebar-one sidebar-two" "footer footer footer";
  }
}
```







