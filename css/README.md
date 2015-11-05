# CSS Structure Doc
This doc outlines css conventions and such.
Note: this is all just opinion, feel free to challenge this.

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

Sass is smart enough for you to omit the `_` of the file name when you call it

### Base directory
The base directory should contain 3 main files, `_variables.scss`, `_palette.scss` and `_normailise.scss`.
`_variables.scss` is the file in which you should define all of your brealpoints, gutters, fonts, etc. [example](/examples/_variables.scss)
`_palette.scss` should simply contain colors, a naming convention that is quite well used is `$brand-color%` where `%` is a value. [example](/examples/_palette.scss)

If you have to set a color to an element (lets say a button) and you have multiple buttond that will use this style,
it is best to create variables at the top of your file or at the top of  your element instantiation to state which colors
are being used here, this makes it easier to change the color of one element without changing the color in multiple places.

```scss
  $button-primary:          $brand-color-3;
  $button-primary-alt: darken($brand-color-3, 10%);
  .btn--primary {
    background-color: $button-primary;
    &:hover {
      background-color: $button-primary-alt;
    }
  }

  .btn--primary--inverse {
    border: 1px solid $button-primary-alt;
    background-color: $white;
    &:hover {
      background-color: $button-primary;
    }
  }
```

### Vendor directory
The vendor directory should never be commited to the repo, it should be managed by an external tool like bower
(or slam if I ever get rountd to finishing it). You can write a bower.json file like this:

```json
  {
  "name": "my-app",
  "version": "1.0.0",
  "authors": [
    "woJburgess <jburgess@whiteoctober.co.uk>"
  ],
  "description": "",
  "main": "www/index.html",
  "moduleType": [],
  "license": "MIT",
  "homepage": "",
  "ignore": [
    "**/.*",
    "node_modules",
    "bower_components",
    "scss/vendor",
    "test",
    "tests"
  ],
  "dependencies": {
    "susy": "~2.2.6",
    "compass-breakpoint": "breakpoint-sass#~2.6.1",
    "bourbon": "git@github.com:thoughtbot/bourbon.git"
  }
}
```

This includes some basic modules:
susy: a grid system which is highly flexible,
breakpoint: a cleaner way of defining media queries,
bourbon: a bucket of helpful mixins.

But we don't want this to install in the route of our project so we will also need a .bowerrc file

```json
  {
    "directory": "scss/vendor"
  }
```

Now our vendor directory will be created and populated whenever we run `bower install`.

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
  .nav-bar__link:hover {
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

* !IMPORTANT, don't use !important

## Layout

WRONG (space out your css, let minifiers do this part for you)
```css
  .link--primary {color: red}
```

WRONG (Use shorthand, this can be acheived in one line: `margin: 20px 10px;`)
```css
  .link--primary {
    margin-left: 10px;
    margin-top: 20px;
    margin-right: 10px;
    margin-bottom: 20px;
  }
```

WRONG (one space after identifier)
```css
  .link--primary{
    color: red;
  }
```

WRONG (semi-colon needed at the end of the style)
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

WRONG (Avoid setting heights on elements)
```css
  .element {
    height: 100px
  }
```
RIGHT
```css
  .element {
    min-height: 100%;
  }
```
