# gulp-css-useref

[![NPM](https://nodei.co/npm/gulp-css-useref.png)](https://nodei.co/npm/gulp-useref/)

> Parse CSS files to add assets referenced by `url()`s to the gulp file stream, and optionally transform their path.

## Install

Install with [npm](https://npmjs.org/package/gulp-css-useref)

```
npm install --save-dev gulp-css-useref
```


## Usage

The following example will parse the CSS and pass the files referenced by `url()`s through.

```js
var gulp = require('gulp'),
    cssUseref = require('gulp-css-useref');

gulp.task('default', function () {
    return gulp.src('app/*.css')
        .pipe(cssUseref())
        .pipe(gulp.dest('dist'));
});
```

With options:

```js
var gulp = require('gulp'),
    cssUseref = require('gulp-css-useref');

gulp.task('default', function () {
    return gulp.src('app/*.css')
        .pipe(cssUseref({ base: 'assets' }))
        .pipe(gulp.dest('dist'));
});
```

## API

### cssUseref(options)

Returns a stream with the assets referenced by `url()`s in any CSS files added.


## Options

### `match`
Type: `string`
Default: `undefined`

Optional [micromatch](https://github.com/jonschlinkert/micromatch "micromatch") style glob used to limit which assets are processed.  If the path specified in `url()` doesn't match this glob, it won't be processed.

### `base`
Type: `string`
Default: `''`

Optional base path where the plugin will copy images, fonts, and other assets it finds in CSS `url()` declarations. Only `url()` declarations with relative paths are processed. Each asset's sub-directory hierarchy will be maintained under the base path. Basically, sub-directories after the last `../` in the path will be kept (or the whole path if no `../` exists). For example, if the plugin is called with `{ base: 'dist' }`, the image referred to by `url("../../images/icons/icon.jpg")` will be copied to `dist/images/icons/icon.jpg`.

By using a single `base` path, a build pipeline can output several built CSS files while organizing all their assets under one directory (e.g. under `dist/` in `dist/images/`, `dist/fonts/`, etc.).

If `base` is not specified, assets will be copied by default to the same directory relative to `gulp.dest` as the source asset from `gulp.src`, maintaining the assets' sub-directory hierarchy.  For example, if gulp is told to output to `dist` and `base` is not specified, the image referred to from `app/css/index.css` by `url("../../images/icons/icon.jpg")` (which resolves to `images/icons/icon.jpg`) will be copied to `dist/images/icons/icon.jpg`.

### `pathTransform`
Type: `function`
Default: `undefined`

Optional function that returns a transformed filesystem path to an asset file. Useful for adding revision hashes to filenames for cachebusting (e.g. `image-a7f234e8d4.jpg`), or handling special cases. The function is expected to be of the form given below:
```js
/**
 * Transforms the paths to which asset files will be copied
 *
 * @param {string} newAssetFile - Relative path to which asset would be copied by default
 * @param {string} cssFilePathRel - Relative path to the CSS file that referenced the asset
 * @param {string} urlMatch - The contents of the url() in the CSS file
 * @param {object} options - The options hash passed into gulp-css-useref
 * @returns {string} - Transformed Relative path to which asset will be copied
 */
pathTransform: function(newAssetFile, cssFilePathRel, urlMatch, options) {
    // ... transform newAssetFile ...
    return newAssetFile;
}
```

### `setWebroot`
Type: `string`
Default: `Undefined`

Optional absolute path where static assets reside, if they are referenced with absolute paths in CSS starting with a `/`. Paths to assets in CSS are unchanged.

## Credits

`gulp-css-useref` reuses code and ideas from
* [gulp-useref](https://github.com/jonkemp/gulp-useref "gulp-useref")
* [postcss-copy-assets](https://github.com/shutterstock/postcss-copy-assets "postcss-copy-assets")
 
## License

MIT
