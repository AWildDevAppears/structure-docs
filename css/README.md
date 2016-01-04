# CSS Structure Doc
This doc outlines css conventions and such.
Note: this is all just opinion, feel free to challenge this.

## Notes

  * Throughout this doc I will refer to identifiers, by this I mean the chain of elements/class names and id's
    that you use to identify where you want to put your styles.

  ```css
    identifier {
      /* properties */
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
    "themes": {
      _admin.scss
    }
    "pages": {
      _home.scss
    },
    "utils": {
      _mixins.scss
    },
    "vendors": {
      _bootstrap.scss
    },
    main.scss
  }
}
```
This structure uses a twist on the 7-1 pattern, where all of the `_file.scss'` are imported into `main.scss`.

Sass is smart enough for you to omit the `_` of the file name when you call it.

You should always add a `_` to files that should not be compiled on their own, this signifies that this file is a partial.

### Base directory
The base directory should contain 4 main files, `_variables.scss`, `_palette.scss`, `_typography.scss` and `_normailise.scss`.

`_variables.scss` is the file in which you should define all of your breakpoints, gutters, etc. [example](/examples/_variables.scss)

`_palette.scss` should simply contain colors, a naming convention that is quite well used is `$brand-color%` where `%` is a value. [example](/examples/_palette.scss)

`_typography.scss` outlines all of your font sizes, it defines the diference between a `h1` and a `p`.
This file will outline font sizes and font families and how they differ between tags.

If you have to set a color to an element (lets say a button) and you have multiple buttons that will use this style,
it is best to create variables at the top of your file or at the top of  your element instantiation to state which colors
are being used here, this makes it easier to change the color of one element without changing the color in multiple places.

```scss
  $button-primary:     $brand-color-3;
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

### Utils
The utils directory should contain global mixins and global utility classes only. This directory should contain all
of your projects mixins, You should use partial placeholders for specific elements and these should be with the component they relate to. i.e.

```
{
  components: {
    "_button.scss"
    "_button_part.scss" // partial placeholders which are used for the button
  }
  utils: {
    "mixins.scss" // universal mixins only which each do very small jobs
    "utils.scss" // universal classes only which do very small jobs (like a class to set something to uppercase or a clearfix)
  }
}
```

### Components directory

The components directory is more or less self is explainitary, its a folder full of components, but what exactly *is* a component?

A component is a pattern for a single module of a website (buttons, navbars, forms, etc.). Your component should be in its own file named after the component itself (e.g. `_buttons.scss`).

If your component requires partial placeholders, you should put these in a seperate file next to your existing component file (e.g. `_buttons.scss` could have a file containing partial placeholders which would be called `_buttons_part.scss`). Try to follow a strict naming convention like this (`_<component-name>_part.scss`). this way you can see that these two relate as their names are very similar.

The approach I have outlined is a more modular approach, meaning that each module can coexist together and can be easily moved. A page should require a module, a module should not require another module (It can require a base mixin though, I will explain this later). If your code is built well then you should be able to pull out a component from your sass files and place it in a completly seperate project and have it work straight away (with variables defined base mixins plugged in).


```
  {
    // Component 1
    "_buttons.scss"
    "_buttons_part.scss"
    // Component 2
    "_nav.scss"
    // Component 3
    "_tabs.scss"
    "_tabs_part.scss"
  }
```

#### Universal base mixins

Universal base mixins are mixins that you either pull in as vendor dependencies or define yourself in the utils folder. These are your barebones files, a module can require these (say you need a breakpoint handler for setting up how your module transtions at different screen sizes)

#### Variables and naming conventions

Variables should follow a strict naming convention, If you follow the same naming convention across your sites/apps then your modules are more likely to work well with each other, and you don't end up naming variables twice because one component uses `$brand-color-1` and another uses `$brand-color1`.

Variables should be written in the American style (`color` over `colour`), we do this because css properties use the American writing style.
Colors should all use the same naming convention (I don't care if you choose `$brand-color1`, `$brand-color-1` or `$tone-assertive` just make sure all of your modules and projects are consistent to this standard).

#### Make modules not projects

Think of each part of a project, not as a component of a site, but as a component of the universal styleguide (I'll explain later). You should build a project, not as a project, but as a series of components that, when used together, create your project. Modules (Components) should be standalone and not require anything but the base files of your project. A module can always be used again, a website exists, it may get manipulated and changed, but for the most part it wont be used again. A module is also a lot easier to learn and understand than something that has been written specificly for its job. Also, a module can be maintained, upgraded, torn apart and built again. As the module has a small codebase (comparitive to a whole site) It's easier to see what is trying to be acheived.


### What is the Universal Styleguide

The Universal Styleguide (USG) is the place where your beautiful modules can strive and exist without fear of conflict (They are completely unique and can coexist and exist alone as none of their code conflicts or manipulates the other). The USG should be where you go before writing a line of code, withing the universal styleguide you can grab modules and stick them into your page, saving you time building a component which you have built a thousand times before. Once you have grabbed your component you can tweak it to suit the page. As no two websites are 100% the same, a component that has made its way to the universal styleguide should not contain any site specific changes, it should be as raw as possible and as adaptive as possible (You build a form module on a full width block, I should be able to grab that module and stick it in a box half the size and make it inline without too much trouble). Each module should be well documented on what it does. This way it's easier to make valid choices into which components you will and will not need, and what each component does.

A component can have modifiers (`form__input` can have a modifier class of `form__input--inline`) and you CAN package your modifiers into your component, but with great power comes great responsibity. Only add modifiers to your component that is not site specific and is likely to crop up again and again.

### Vendor directory
The vendor directory should never be commited to the repo, it should be managed by an external tool like bower
(or slam if I ever get rounnd to finishing it). You can write a bower.json file like this:

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

### main.scss
Your `main.scss` should be the only file you need to compile, all of your other files should be included in this file.

```scss
  // Vendor
  @include 'vendor/path/to/susy',
    'vendor/path/to/breakpoint',
    'vendor/path/to/bourbon';

  // Base
  @include 'base/palette',
    'base/variables',
    'base/normalise',
    'base/typography';

  // Utilities
  @include 'utils/mixins',
    'utils.scss';

  // Layout
  @include 'layout/grid-mixins',
    'layout/grid';

  // Components
  @include 'component/buttons',
    'component/nav';

  // Pages
  @include 'pages/home';
```

### What about _shame.scss?
You can have a `_shame.scss` in your project pulled in last where you list all of the ugly fixes that really should not
be there, you can also list a shame section in your main.scss below all of your file includes.

Shame blocks should be well commented, explaining why you have committed such sins.

```scss
  /**
  * There are too many different nav link styles,
  * nav__link--super does not overwrite the styles applied to the li tags in this section
  */
  .block__nav--full-width .nav__links.nav__link--super {
    font-weight: 400 !important; // I am really sorry
  }
```

Of course, make sure your comment isn't half-baked and half arsed like mine.


### Mixins vs Placeholders

Sass provides a number of different ways to share code between CSS rules.
You can use mixins to insert new CSS properties and/or rules into your CSS and you can use @extend to share CSS properties between selectors.

Placeholders should be component specific and therefore pinned to the component and only used for that component.
Mixins should be completely unspecific and unrelated to any modules, they should perform a job that can be used again and again in
many different sites and scenarios.

### Order of identifier styles
Identifier styles should take the order: extends, includes, regulars then nests. This is what it should look like:

```scss
  .weather {
    @extend %module;
    @include transition(all 0.3s ease-out);
    background: red;
    &:hover {
      color: white;
    }
}
```

All of your styles should be grouped by type, i.e margin tags should be grouped together, colors should be grouped etc.

You should try to group properties under these headings.

* Positioning
* Display and Box Model
* Color
* Text
* Other

```scss
  .identifier {
    margin: 10px;
    margin-bottom: 5px;
    min-width: 50%;
    height: 100%;
    float: left;
    background-color: green;
    color: white;
  }
```

## Linting

Its always a good idea to have a linter set up, no matter what language you are writing in. SCSS is no exeption,
you should be using a linter like [scssLint](https://github.com/brigade/scss-lint), which can integrate with
[gulp](https://github.com/noahmiller/gulp-scsslint).

With this you can be assured that no matter what happens, your code will for the most part, look the same everywhere.
It even supports BEM syntax and can check to make sure you are using it, depending on your [config](/examples/scss-lint.yml)

It is quite easy to integrate into your gulpfile, as outlined [here](https://github.com/noahmiller/gulp-scsslint)

```js
  var gulp      = require('gulp');

  var sass      = require('gulp-ruby-sass');
  var minifyCss = require('gulp-minify-css');
  var scsslint  = require('gulp-scss-lint');


  var paths = {
    sassConfig: '.scss-lint.yml',
  };

  gulp.task('scss-lint', function() {
    return gulp.src(paths.sass)
      .pipe(scsslint({
        'config': paths.sassConfig,
      }))
      .on('error', function (err) {
        console.error(err);
      });
  });

  gulp.task('sass', function (done) {
      sass('./scss/main.scss')
      .pipe(gulp.dest('./web/css/'))
      .pipe(minifyCss({
        keepSpecialComments: 0
      }))
      .pipe(rename({ extname: '.min.css' }))
      .pipe(gulp.dest('./web/css/'))
      .on('end', done);
  });

  gulp.task('default', ['scss-lint', 'sass']);
```

## Code structure

HTML
* Wrap all of your class calls in double quotes

Even though this works
```html
<div class=box></div>
```

Please wrap them like this
```html
<div class="box"></div>
```
This way, if we ever add a second class to this element, we won't need to add in the double quotes before we do.

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

* !IMPORTANT, !important is not a fix, it's a promise.

```css
  .u-upper {
    text-transform: uppercase !important;
  }
```
`.u-upper` will ALWAYS convert something to uppercase, by using the `!important` tag you are promising this to the
code base that `.u-upper` will never text-transform anything but to uppercase. Although that doesn't mean you should use it
just because, If your styles are written well, just including classes like this last should be enough.

## Layout

WRONG (space out your css, let minifiers do this part for you)
```css
  .link--primary {color: red}
```

WRONG (Use shorthand, this can be achieved in one line: `margin: 20px 10px;`)
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

* Use DocBlock style comments for long comments.

```css
/**
 * The site’s main page-head can have two different states:
 *
 * 1) Regular page-head with no backgrounds or extra treatments; it just
 *    contains the logo and nav.
 * 2) A masthead that has a fluid-height (becoming fixed after a certain point)
 *    which has a large background image, and some supporting text.
 *
 * The regular page-head is incredibly simple, but the masthead version has some
 * slightly intermingled dependency with the wrapper that lives inside it.
 */
```

### What goes into the repo and what goes in my gitignore?
Things you should not include in your repo:

* Sass sourcemaps
* Compiled CSS
* Vendor dependencies
