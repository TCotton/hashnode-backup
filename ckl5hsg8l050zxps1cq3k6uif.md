## Using Backbone.js with the Parcel bundler

Just recently I revisited an old side-project created with [Backbone.js](https://backbonejs.org/). I was surprised about how clean the code was and how much I had written over one weekend nine years previous. Rather than refactoring the JavaScript into a new library, I decided to keep the Backbone.js and code use it as a base for a new project, with the two required dependencies,  [jQuery](https://jquery.com/) and  [Underscore](https://underscorejs.org/).

However, in 2021, it's better to use a module bundler. For this new project I used [Parcel](https://github.com/parcel-bundler/parcel). Getting a new generation of bundler to work with an older generation of JavaScript frameworks required some attention. Parcel doesn't work with global scope so it's necessary to import jQuery first and attach it to Backbone.js at the top of every module:


```
const jquery = require("jquery");
import Backbone from "backbone";
window.$ = window.jQuery = jquery;
Backbone.$ = window.$;
``` 

This is used when created view instances;

```
Backbone.$(function () {
    new LIGHTNING.View.AddImage({model: LIGHTNING.newModel});
    new LIGHTNING.View.Worker({model: LIGHTNING.newModel});
    new LIGHTNING.View.ImageDetails({model: LIGHTNING.newModel});
  });
``` 

Both Underscore and jQuery have to be loaded BEFORE Backbone.js

Using Bundle 2 (still beta phase) was causing errors with the template syntax, but version one (1.12.4) was fine. I would expect Bundle 2 incompatibility to have been resolved by the final release, or a template transformer to have been written.

The project because an  [online image compressor](https://pngjpeg.imagecompression.online) that utilises Web Assembly (WASM) to optimise JPEG or PNGs in the browser.

#javascript, #WASM, #backbone.js


