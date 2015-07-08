# CSS Structure Doc
This doc outlines css conventions and such.
Note: this is all just opinion.

## Notes

  * Throughout this doc I will refer to identifiers, by this I mean the chain of elements/class names and id's
    that you use to identify where you want to put your styles

    ```css
      identifier {
        style: style-value;
        style: style-value;
      }
    ```


# Pre-processors / Pre-parsers

## How should I structure my files?

The way I would suggest to structure your files is as follows.
I will use sass as an example here.

```
{
  "scss": {
    "base" : {
      _typography.scss
    },
    "components": {
      _buttons.scss
    },
    "layout": {
      _grid.scss
    },
    "pages": {
      _home.scss
    },
    "themes": {
      _main-theme.scss
    },
    "utils": {
      _variables.scss
    },
    "vendors": {
      _bootstrap.scss
    },
    main.scss
  }
}
```
This structure uses the 7-1 pattern, where all of the _file.scss' are imported into main.scss.

Sass is smart emough for you to omit the `_` of the file name when you call it

## Code structure

Ground rules
* Don't chain over 3 element identifiers

WRONG
```css
  .nav .nav-menu li a:hover {
    color: red;
  }
```

RIGHT
```css
  .nav-bar__link a:hover {
    color: red;
  }
```

If you think you can't avoid it, something else has gone wrong along the way. Specificity leads to greater specificity.
Stay away from the specificity void.

* Avoid ID's

IDs should only be used once, meaning that they are highly specific.

WRONG
```css
  #header .nav a {
    color: red;
  }
```
RIGHT
```css
  .header__nav a {
    color: red;
  }
```

* B.E.M is your friend

B.E.M reduces chaining by being specific in the class name


WRONG
```css
  .block {
    &.element {
      &.modifier {
        a {
          color: red;
        }
      }
    }
  }
```

RIGHT
```css
  .block__element--modifier a {
    color: red;
  }
```

* Never inline - inline styles are hard to maintain.

* !IMPORTANT, dont use !important

## Layout

WRONG (space out your css, let minification do this part for you)
```css
  .link--primary {color: red}
```

WRONG (Use shorthand, this can be acheived in one line: `margin: 20px 10px;`)
```css
  .link--primary {
    margin-left: 10px;
    margin-top: 20px;
    margin-right: 10px
    margin-bottom: 20px;
  }
```

WRONG (one space after identifier)
```css
  .link--primary{
    color: red;
  }
```

WRONG (seni-colon needed at the end of the style)
```css
  .link--primary {
    color: red
  }
```

WRONG (space needed between style and style value)
```css
  .link--primary {
    color:red;
  }
```

WRONG (avoid universal selectors)
```css
  .link--primary * {
    color: green;
  }
```
