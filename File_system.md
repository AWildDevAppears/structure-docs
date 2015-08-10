## Files

```
{
  "web/": {
    "css/",
    "node_modules/", // required for app
    "assets", // images
    "js/",
    "index.html / .php"
  },
  "scss/": {
    // 7 : 1 model
  },
  "node_modules/", // dev version
  "bin/",
  "gulpfile.js",
  "package.json"
}
```
### Web folder

The web folder should be independant from anything outside of it.
You should be able to push the contents of this folder up to the server without worrying about bringing down the server
(after compile-ing all of the scss into the css folder).

(this model may not work for a CMS)


### Outside of the web folder

Outside of the web folder should be all of your development tools that should never touch the server.
In the projects root you should put your scss folders, binaries, gulpfiles, config files etc.

### node modules

Node modules that are app dependant should be installed in the web folder, modules that are not, running
`npm install` in the web directory should install the modules into the web folder from the `package.json`
one layer deeper

### scss files

// See the css structure doc

### assets

The assets folder should only contain optimised images and not any images that are not required for the app. If you have any
assets that you need to keep but they will not be displayed on your app / website, put them in their own folder outside of the web directory.

### JS

The js folder should contain all of your JS folders, except in off cases (using an MVC like angular). If your project requires a different web layout
then you can change this, as long as what is in the web / app folder is minified and as close to a RC as possible, think of the web folder as your build folder.



