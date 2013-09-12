# resource-embedder [![Build Status](https://secure.travis-ci.org/callumlocke/resource-embedder.png?branch=master)](http://travis-ci.org/callumlocke/resource-embedder)

Node module for automatically embedding the contents of external scripts and stylesheets into HTML markup.

Takes an HTML file path and generates a string of modified markup. Any small external scripts and stylesheets are replaced with inline `<script>...</script>` or `<style>...</style>` elements.

This reduces the number of HTTP requests in return for inflating your HTML. It's up to you to decide whether this is a good trade-off in your situation, and to configure this module optimally.

**Also available as a Grunt plugin: [grunt-embed](https://github.com/callumlocke/grunt-embed)**

## Usage

    npm install resource-embedder

```javascript
var ResourceEmbedder = require('resource-embedder');

var embedder = new ResourceEmbedder('./app/page.html');

embedder.get(function (markup) {
  fs.writeFileSync('./dist/page.html', markup);
});
```

### Choosing which files to embed

By default, **any scripts or stylesheets below 5KB in size** will be embedded. You can change this setting in the options.

You can also manually decide the threshold for each resource using a `data-embed` attribute.

To always embed a particular resource, regardless of filesize, just include the attribute:

```html
<script src="foo.js" data-embed></script>
<link rel="stylesheet" href="foo.css" data-embed>
```

To prevent a particular script from ever being embedded, set it to `false` (or `0`):

```html
<script src="foo.js" data-embed="false"></script>
<link rel="stylesheet" href="foo.css" data-embed="false">
```

To embed a particular resource only if it is below a certain size, specify the maximum number of bytes, or something like `5KB`:

```html
<script src="foo.js" data-embed="2000"></script>
<link rel="stylesheet" href="foo.css" data-embed="5KB">
```

### Options

To specify options:

`new ResourceEmbedder('./file.html', options);`

...or just: `new ResourceEmbedder(options);`

* `htmlFile` — only required if you don't pass a file path as the first argument to the constructor.
* `assetRoot` — (optional) – use this if you need to specify the directory that the relative resource URLs will be considered relative to. (By default, it's the same directory as the HTML file.)
* `threshold` (default: `"5KB"`) — all resources below this filesize will be embedded. NB: you can set this to `0` if you want, in which case nothing will be embedded except those resources you mark with a `data-embed` attribute (see above).
* `stylesheets` (default: `true`) — whether to embed stylesheets.
* `scripts` (default: `true`) — whether to embed scripts.


## Contributing
In lieu of a formal styleguide, take care to maintain the existing coding style. Add unit tests for any new or changed functionality. Test your code using [Grunt](http://gruntjs.com/).


## Release History
_(Nothing yet)_


## License
Copyright (c) 2013 Callum Locke. Licensed under the MIT license.


## Wishlist

* Grunt plugin
* Connect/Express middleware
* for stylesheet `link`s with `media` attributes, the embedded CSS should be automatically wrapped in a media query.
* ability to specify root for relative paths
* non-blocking file reads
* remove `data-embed` attributes from elements that don't get embedded.