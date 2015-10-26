## Files

```
{
  "web/": {
    "css/",
    "assets",
    "js/",
    "lib/", // node modules
    "index.html / .php"
  },
  "scss/": {
    // 7 : 1 model
  },
  "node_modules/",
  "bin/",
  "src/",
  "gulpfile.js",
  "package.json"
}
```
### Web folder

The web folder should be independent from anything outside of it.
You should be able to push the contents of this folder up to the server without worrying about bringing down the server
(after compiling all of the scss into the css folder). The directories within the web folder (css/js) should for the most part be empty.
You can stick a .gitDontIgnore file into each directory, and just ignore the rest of the files.
Place your unconcatenated unminified JS into it's own folder outside of the web folder.

(this model may not work for a CMS)


### Outside of the web folder

Outside of the web folder should be all of your development tools that should never touch the server.
In the projects root you should put your scss folders, binaries, gulpfiles, config files etc.

### Node modules

Node modules that are app dependant should be installed in the web folder, modules that are not, running
`npm install` in the web directory should install the modules into the web folder from the `package.json`
one layer deeper

If you are using browserify, require in your node modules, if not, install non dev modules to the web/lib/ folder, this also includes bower modules

### scss files

// See the css structure doc

### assets

The assets folder should only contain optimised images and not any images that are not required for the app. If you have any
assets that you need to keep but they will not be displayed on your app / website, put them in their own folder outside of the web directory.
As a rule of thumb, if your assets folder only contains images, call the folder img, if it contains other assets (like audio), call it assets (or rescources)

### src

The src folder should contain all of your JS files. If your project requires a different web layout then you can change this, as long as what is in the web/js folder is minified and as close to a RC as possible, think of the web folder as your build folder. Try to have only one js file in the web/js folder, concatenated and minified, you can acheive this using browserify or something similar.
