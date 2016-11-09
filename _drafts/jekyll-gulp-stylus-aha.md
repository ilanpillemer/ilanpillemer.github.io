---
layout: post
title: Jekyll, Gulp, Stylus, Browsersynch, aha!
---

Well…. It all started when I decided I wanted to make a minor change to the look and feel of this blog. Really minor as I added two lines. But it made something go wonky and I needed to dig a little deeper and the next minute there I was and suddenly everything went very weird.. and the next minute I was down the JavaScript rabbit hole.

So lets start at the beginning (always a very good place to start like do-re-mi and ay-be-cee)

```
readings:
  - name: Jekyll (documentation)
    url: https://jekyllrb.com/docs/home/
  - name: Game On! (gitbook)
    url: https://gameontext.gitbooks.io/gameon-gitbook/content/
  - name: MapReduce (paper)
    url: http://static.googleusercontent.com/media/research.google.com/en//archive/mapreduce-osdi04.pdf
```
Deciding to reboot my blog I first needed to make some decisions about what I needed to support and enable me.

My requirements were the following:

Simple to add posts and augment information. It is an absolute that when the tooling is in place adding a new post should just be dropping a text file into a folder.
The building and deployment of the site must be simple and and a pleasure
The site must work on all devices, especially browsers and smart phones.
The site itself must be elegant, simple and beautiful.
After poking around I decided to give Jekyll a go. Once up and running its a pleasure to add posts. It just using using the apparently simple markdown syntax and dropping the files into a folder when ready. You can run the system locally. The simplicity of deployment is provided by Github Pages, where the entire build and deployment process is made completely transparent and just happens on commit. And then a shout out to Mr Void for creating a beautiful and elegant template.

I have never used Jeykll before, so with my mind intact I decided to explore.

One of the first changes I made after I learnt about jekyll serve --baseurl= is that the _config.yml does not get picked up by the watcher. At this point I learnt that if you want to have yml files that you can change dynamically you need a _data directory for that config files. The only config you want in the _config.yml file is config that pretty much never changes and you don’t want to change. It just becomes noise once in the place.

A myriad of javascript libraries took me a journey through Javascript libraries, most which do not seem to have been updated for a good while now. So perhaps a tour through the stae of javascript a year or two ago.

The first of which was a library called gulp-plumber which is patch to deal with issues that existed at that time with node streams. Essentially certain errors in a pipeline (that should not bork the pipeline) were still borking the pipeline. This was a monkey patch to make pipelines work correctly in gulp. Last commit, 9 Feb 2016.

And then next I discovered browsersynch wich meant that if I run gulp instead of jekyll serve updates the stylus files happened realtime in the browser enabling me to finally start changing the styling to more of liking. And suddenly JavaScript development felt less evil, and much more fun. Last commit, approximately three weeks ago. And Stylus last updated 6 months ago.

Which the led me to jeet…

Now all very nice that a commit to remote got the blog to deploy correctly to github pages. Well at least I thought so; I was not down the rabbit hole and underground far enough yet to learn that only new posts or changes to the html had this effect. I was still to learn that any changes to the styles would have no effect at all.. But that will become later on when we pass by the March Hare. I just wanted to be able to test locally which was a good reason to enter into the documentation and start a proper RTFM journey.

After installing jekyll via brew and running jekyll serve it was not right as the css files were not where they were meant to be. Eventually I thought I solved it by running jekyll serve --baseurl= which worked. So thought that all was fixed and moved on…

But meanwhile the watching the browsersyncing was not working properly.. there were two different possible watch systems I had uncovered so far.. the jekyll serve and the gulp watch tasks. Now something was not working though properly. I could see what was meant to be happening. I was not meant to be using jekyll serve but only gulp which had a watch task as part of the default runner. It looked like this
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
… sadly not working properly as the jekyll-rebuild was not putting the css in the right place for some sad reason. The stylus changes were being picked up without issue, but the moment I updated a post the css stopped being what it should be. Garrrrrumph… and then exploring I discovered the cause was somehow related to the base-url thing from earlier. As when I looked at where it was looking for the css, I saw it was looking the WRONG place. Obviously of course it was doing that, seeing the css was not working. Eventually after a lot of puzzling and what is the correct way to run these commands so thet actually work, I noticed there are two gulp files… one in the obvious area and one in the generated folder under _site and so if I were to run gulp from there then all might actually suddenly come right.. and it did.. Yay! Woohoo! Now as I save these words I am seeing jekyll and browsersync working together so that the words appear in the browser synched with all necessary magic on save.. but my joy was shortlived as… Flip.. it was no longer working for the styl files anymore. For now the reverse became the case again and I was literally back where I started. Because now the updating the .md files synchs; but the styl now that goes wrong.

And I started researching again, again and again.

Eventually I reverted back to running gulp where it felt right (not in the _site directory) and then editing the gulpfile to make sure it generates the jekyll files correctly so it can be served locally and synced effectively; by adding the --baseurl= into the correct place. And finally it was working like really it should have been working “out of the box” when I first started. But perhaps, this was better for me as now I have a much better understanding of what’s going on. And it is kinda cool.

```javascript
gulp.task('jekyll-build', function (done) {
    browserSync.notify(messages.jekyllBuild);
    return cp.spawn(jekyllCommand, ['build', '--baseurl='], {stdio: 'inherit'})
	.on('close', done);
});
```
And now I am starting to stop hating the Queen of Hearts and beginning to love JavaScript.
