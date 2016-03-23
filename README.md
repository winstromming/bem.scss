# BEM.scss

These helpers are designed to enable developers to well-structured and consistent BEM components with SCSS. BEM stands for `"block__element--modifier"` and describes a classname syntax structure designed to keep code modular.

Elements can be nested within other Elements and Modifiers; both which reside within a parent Block statement.

## Usage

Within your `.scss` files, import the `_bem.scss` file and use the mixin with `@include`:

```
@import 'bem';

@include block('block') {

  @include element('element') { ... }
  @include modifier('modifier') { ... }

}
```

*Output:*

```
.block {}
  .block__element {}
  .block--modifier {}
```

## Advanced Usage

Using mixin structures like these enable us to avoid the repetition common to writing BEM syntax. We could nest modifiers within elements and they will get parsed and inherited correctly by any modifiers in the parent block.

```
@include block('text') {

  padding-left: 1em;
  padding-right: 1em;

  @include element('content') {

    // Default
    font-size: 16px;
    line-height: 1.5;

    // Modifiers
    @include modifier('small') { font-size: 14px; }
    @include modifier('large') { font-size: 18px; }

  }

  @include modifier('left') { text-align: left; }
  @include modifier('right') { text-align: right; }

}
```

*Output:**

```
.text,
.text--left,
.text--right {
  padding-left: 1em;
  padding-right: 1em;
}
  .text__content {
    font-size: 16px;
    line-height: 1.5;
  }
    .text__content--small {
      font-size: 14px;
    }
    .text__content--large {
      font-size: 18px;
    }
  .text--left {
    text-align: left;
  }
  .text--right {
    text-align: right;
  }
```

## Media Queries and Inheritance

You can introduce media queries into any section of the syntax and proper inheritance will be exposed when compiling. For example:

```
@include block('text') {

  padding-left: 1em;
  padding-right: 1em;

  @include element('content') {

    // Default
    font-size: 16px;
    line-height: 1.5;

    // Adjust the text line height for different screen widths
    @media screen and (max-width: 600px) { line-height: 1.3; }
    @media screen and (max-width: 900px) { line-height: 1.4; }

    // Modifiers
    @include modifier('small') { font-size: 14px; }
    @include modifier('large') { font-size: 18px; }

  }

  @include modifier('left') { text-align: left; }
  @include modifier('right') { text-align: right; }

}
```

*Output*:

```
.text,
.text--left,
.text--right {
  padding-left: 1em;
  padding-right: 1em;
}
  .text__content,
  .text__content--small,
  .text__content--large {
    font-size: 16px;
    line-height: 1.5;
  }
    @media screen and (max-width: 600px) {
      .text__content,
      .text__content--small,
      .text__content--large {
        line-height: 1.3;
      }
    }
    @media screen and (max-width: 900px) {
      .text__content,
      .text__content--small,
      .text__content--large {
        line-height: 1.4;
      }
    }
    .text__content--small {
      font-size: 14px;
    }
    .text__content--large {
      font-size: 18px;
    }
  .text--left {
    text-align: left;
  }
  .text--right {
    text-align: right;
  }
```

## Nesting elements or modiiers

This mixin structure does not allow you to nest elements within other elements, or modifiers within other modifiers. That's not what BEM is for! You can absolutely put elements within modifiers, though, and inheritance will be correctly preserved:

```
@include block('text') {

  padding-left: 1em;
  padding-right: 1em;

  @include element('content') {

    // Default
    font-size: 16px;
    line-height: 1.5;

    // Modifiers
    @include modifier('small') { font-size: 14px; }
    @include modifier('large') { font-size: 18px; }

  }

  @include modifier('left') {
    @include element('content') {
      text-align: left;
    }
  }
  @include modifier('right') {
    @include element('content') {
      text-align: right;
    }
  }

}
```

*Output:*

```
.text,
.text--left,
.text--right {
  padding-left: 1em;
  padding-right: 1em;
}
  .text__content,
  .text__content--small,
  .text__content--large {
    font-size: 16px;
    line-height: 1.5;
  }
    .text__content--small {
      font-size: 14px;
    }
    .text__content--large {
      font-size: 18px;
    }
  .text--left .text__content,
  .text--left .text__content--small,
  .text--left .text__content--large {
    text-align: left;
  }
  .text--right .text__content,
  .text--right .text__content--small,
  .text--right .text__content--large {
    text-align: right;
  }
```
