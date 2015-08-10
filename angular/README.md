# Angular

## Web directory (small app)
```
{
  "controllers/": {
    "aController.js",
    "bController.js"
  },
  "directives/": {

  },
  "services/": {

  },
  "js/": {
    "bootstrap.min.js",
    "jquery.min.js"
  },
  "app.js"
},
"views/": {
  "components/": {
    "nav.html",
    "header.html",
    "footer.html"
  },
  "page1.html",
  "page2.html"
},
"index.html"
```

## Web directory (large app)
```
"app/": {
  "shared/": {
    "sidebar/": {
      "sidebar-directive.js",
      "sidebar-view.html"
    },
    "article/": {
      "article-directive.js",
      "article-view.html"
    }
  }
  "components/": {
    "home/": {
      "home-controller.js",
      "home-service.js",
      "home-view.html"
    },
    "blog/": {
      "blog-controller.js",
      "blog-service.js",
      "blog-view.html"
    }
  },
  "app.module.js",
  "app.routes.js"
},
"lib/": {
  "jquery.min.js"
},
"js/": {
  "js-file-not-for-angular.min.js"
},
"css/" : {
  "style.css"
},
"assets": {
  "place-cat.png"
},
"index.html"
```
