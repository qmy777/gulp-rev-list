# gulp-rev-list

> Static asset revisioning by producing a js file to save recorded the reflection of the file name and the hash from file name
> `unicorn.css` → `{"unicorn.css" : "d41d8cd98f"}`

## Install

```
$ npm install --save-dev gulp-rev-list
```


## Usage

```js
var gulp = require('gulp');
var revList = require('gulp-rev-list');

gulp.task('default', function () {
	return gulp.src('src/*.css')
		.pipe(revList())
		.pipe(revList.menifest())
		.pipe(gulp.dest('dist'));
});
```


## API

### revList()

### revList.manifest([path], [options])

#### path

Type: `string`  
Default: `"rev-manifest.js"`

Manifest file path.

#### options

##### base

Type: `string`  
Default: `process.cwd()`

Override the `base` of the manifest file.

##### winval

Type: `string`  
Default: `jsonForHash`

Override the object of `window` of the manifest file.

##### cwd

Type: `string`  
Default: `process.cwd()`

Override the `cwd` (current working directory) of the manifest file.

### Original path

Original file paths are stored at `file.revOrigPath`. This could come in handy for things like rewriting references to the assets.


### Asset hash

The hash of each rev'd file is stored at `file.revHash`. You can use this for customizing the file renaming, or for building different manifest formats.


### Asset manifest

```js
var gulp = require('gulp');
var revList = require('gulp-rev-list');

gulp.task('default', function () {
	// by default, gulp would pick `assets/css` as the base,
	// so we need to set it explicitly:
	return gulp.src(['assets/css/*.css', 'assets/js/*.js'], {base: 'assets'})
		.pipe(gulp.dest('build/assets'))  // copy original assets to build dir
		.pipe(revList())
		.pipe(revList.manifest())
		.pipe(gulp.dest('build/assets')); // write manifest to build dir
});
```

An asset manifest, mapping the original paths to the revisioned paths, will be written to `build/assets/rev-manifest.js`:

```js
window.jsonForHash = {"css/unicorn.css": "d41d8cd98f","js/unicorn.js": "273c2cin3f"}
```

By default, `rev-manifest.js` will be replaced as a whole:

```js
var gulp = require('gulp');
var revList = require('gulp-rev-list');

gulp.task('default', function () {
	// by default, gulp would pick `assets/css` as the base,
	// so we need to set it explicitly:
	return gulp.src(['assets/css/*.css', 'assets/js/*.js'], {base: 'assets'})
		.pipe(gulp.dest('build/assets'))
		.pipe(revList())
		.pipe(revList.manifest({
			base: 'build/assets'
		}))
		.pipe(gulp.dest('build/assets'));
});
```

You can optionally call `revList.manifest('manifest.js')` to give it a different path or filename.

## License

MIT © PETKIT.COM
