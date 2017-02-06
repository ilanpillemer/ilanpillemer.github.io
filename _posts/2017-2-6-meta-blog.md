---
layout: post
title: Jekyll, Gulp, Stylus, Browsersynch, aha!
---

This post is continuously updated as a reference point for myself when tooling this Jekyll blog.

```
readings:
  - name: Jekyll (documentation)
    url: https://jekyllrb.com/docs/home/
```

<tl;dr>

* _config.yml does not get picked up by the watcher. 
  * which means.. The only config you want in the _config.yml file is config that you don't change and you don’t want to change.

* browsersynch is nice. 
  * To use all the gulp stuff and also use the jeykll stuff then one needs to pwn the other.
  * iow, If gulp is calling jekyll, make sure you understand how they interact.
  * use the browsersync port when developing

---
Shout out to [victorvoid](https://github.com/victorvoid), as I forked his [repository](https://github.com/victorvoid/space-jekyll-template) - and then proceeded to "improve".. ;)

```javascript
gulp.task('default', ['js', 'stylus', 'browser-sync', 'watch']);
```


```javascript
gulp.task('js', function(){
    return gulp.src('src/js/**/*.js')
	.pipe(plumber())
	.pipe(concat('main.js'))
	.pipe(uglify())
	.pipe(gulp.dest('assets/js/'))
	.pipe(gulp.dest('_site/assets/js/'))
	.pipe(browserSync.reload({stream:true}))
});
```
* [gulp-plumber](https://github.com/floatdrop/gulp-plumber)  patches node streams. Essentially certain errors in a pipeline (that should not bork the pipeline) bork the pipeline. 
* [gulp-concat](https://github.com/contra/gulp-concat) concatenates files specified in `gulp.src`
* [gulp-uglify](https://github.com/terinjokes/gulp-uglify) minifies file, note it recommmends [pump](https://github.com/mafintosh/pump) instead of gulp-plumber.
* [browsersynch](https://browsersync.io/docs/gulp) is cool. Note, browsersync documentation recommends reloading stream after the return of the minification.. so might be timing issues. Have not seen any.

```javascript
gulp.task('stylus', function(){
    gulp.src('src/styl/main.styl')
	.pipe(plumber())
	.pipe(stylus({
	    use:[koutoSwiss(), prefixer(), jeet(),rupture()],
	    compress: true
	}))
	.pipe(gulp.dest('_site/assets/css/'))
	.pipe(gulp.dest('assets/css'))
	.pipe(browserSync.reload({stream:true}))

});
```
* [stylus](http://stylus-lang.com) a css compiler
* [kouto swiss](http://kouto-swiss.io) a mixin library/toolbox for stylus
* [autoprefixer-stylus](https://github.com/jescalan/autoprefixer-stylus) parses CSS and adds prefixes using values from [Can I Use](http://caniuse.com)
* [jeet](http://jeet.gs) a grid system for humans
* [rupture](http://jescalan.github.io/rupture/) a utility for working with media queries and breakpoints in stylus

```javascript
gulp.task('browser-sync', ['jekyll-build'], function() {
    browserSync({
	server: {
	    baseDir: '_site'
	}
    });
});
```

* gets it going after gulp is first called.

```javascript
/**
 * Watch stylus files for changes & recompile
 * Watch html/md files, run jekyll & reload BrowserSync
 */
gulp.task('watch', function () {
	gulp.watch('src/styl/**/*.styl', ['stylus']);
	gulp.watch('src/js/**/*.js', ['js']);
	gulp.watch('src/img/**/*.{jpg,png,gif}', ['imagemin']);
	gulp.watch(['*.html', '_includes/*.html', '_layouts/*.html', '_posts/*'], ['jekyll-rebuild']);
});
```

* runs stylus task when stylus files change
* run js task when js files change
* runs imagemin when images change
* runs jekyll rebuild when jekyll files change

```javascript
gulp.task('jekyll-build', function (done) {
    browserSync.notify(messages.jekyllBuild);
    return cp.spawn(jekyllCommand, ['build', '--baseurl='], {stdio: 'inherit'})
	.on('close', done);
});
```

* run jekyll to do jekyll things...

```javascript
gulp.task('jekyll-rebuild', ['jekyll-build'], function () {
    browserSync.reload();
});
```

* does what it says it does.


