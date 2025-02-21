

# Web Tooling and Automation
[https://classroom.udacity.com/courses/ud892]
## Productive Editing
The course uses sublime 3 on mac

### Shortcuts 
#### Sublime
`Cmd+T` - fuzzy file finder
* Involves no mouse input or thinking about tab position
* `Cmd+ R` or `@` Symbol search function

`Cmd + Alt + G` - find the next instance of selected text
`TAB`
	`img [TAB]` for a complete image tab
`Alt+drag` column selection
`Cmd + D` - Keeps current selection and also selects the next instance of the same text
`Cmd + Ctrl + G` - selects all instances (of the current selection)
``` `Ctrl + ` ``` - or View -> Show Console 

`Cmd + Shift + P` - command palette
`Cmd+K Cmd+L` - converts the selection to lowercase. 

### Extensions
#### Sublime on Mac
`Cmd + Shift + P` to open the command pallet
`Package Control: Install Package`
This will bring up a list of all available packages.
#### Recommended packages
* Package Control - The Sublime Text package manager
* Emmit [emmit.io] Greatly improved tab completion.
* [SideBarEnhancements](https://packagecontrol.io/packages/SideBarEnhancements) - Enhances left-click options for the left side bar.
* ColorPicker
	* `Cmd+Shift+C` To open the color picker.
	* If the cursor is in a color value, selecting a new color will replace that value.
* ColorHighlighter
	* Works with ColorPicker
	* Underlines or outlines color values in that color for a preview.

## Powerful Builds (Gulp)
Gulp config files are approximately javascript.
It executes in parallel.
It is fast because it uses streams.
### Install instructions
[https://github.com/gulpjs/gulp/blob/master/docs/getting-started.md]
1. First, install node and npm.
2. `npm install --global gulp-cli`
3. `npm init`
4. `npm install --save-dev gulp@next`
5. Create a gulpfile: `gulpfile.js`
```
		var gulp = require('gulp');
		gulp.task('default, defaultTask);
		function defaultTask(done){
			//place code for your default task here
			done();
		}
```
6. Test : `gulp`
	* To run multiple tasks: `gulp <task1> <task2>`

### Api Docs
> .src, .watch, .dest, .parallel, .series, CLI args - How do I use these things?

Where do I go now?

-   [API Documentation](https://github.com/gulpjs/gulp/blob/master/docs/API.md)  - The programming interface, defined
-   [Recipes](https://github.com/gulpjs/gulp/blob/master/docs/recipes)  - Specific examples from the community
-   [In Depth Help](https://travismaynard.com/writing/getting-started-with-gulp)  - A tutorial from the guy who wrote the book
-   [Plugins](https://gulpjs.com/plugins/)  - Building blocks for your gulp file



Streams
* Gulp uses in-memory streams rather than temporary files to bypass filesystem overhead.
### Making CSS suck less with Sass and autoprefixer
[Sass](http://sass-lang.com/)
1. `npm install gulp-sass`
2. `mkdir sass`
3. `mv *.css sass/`
4. rename them to `.scss`
5. in `gulpfile.js`
	a. at the top: `var sass = require('gulp-sass');`
	b. add a 'styles' task 
	```
	gulp.task('styles', function(){
		gulp.src('sass/**/*.scss')
		.pipe(sass())
		.pipe(gulp.dest('./css');
	});	```
6.  To log errors without terminating the build, change `.pipe(sass())` to
	* `.pipe(sass().on('error',sass.logError))` 
7. For fixing browser compatibility:
	1. `npm install --save-dev gulp-autoprefixer ` 
	2. in gulpfile.js 
		* at the beginning `var autoprefixer = require('gulp-autoprefixer');`
		* Modify the 'styles' task:
		```
			gulp.task('styles',function(){
				gulp.src('sass/**/*.scss')
				.pipe(sass().on('error', sass.logError))
				.pipe(autoprefixer({
					browsers: ['last 2 versions']
					}))
				.pipe(gulp.dest('./css'));
			});
		```
		3. Now the compatibility prefixes can be removed from the `.scss` files.
			* They will be added into the `.css` files.
8. CSS can now be nested for easier reading.
9. Run `gulp styles` to convert the `.scss` files to `.css`	

### Automating tasks with `gulp.watch()`	
```	
	gulp.task('default', function(){
		gulp.watch('sass/**/*.scss',['styles']);
	});
	
```
* The second argument to `gulp.watch()` can be a callback or an array of tasks.

## Live Editing

In browser [brackets.io].
[Chrome Dev Workspaces](https://developer.chrome.com/devtools/docs/workspaces)
[Takana](https://packagecontrol.io/packages/Takana) plugin for Sublime
### [Browser Sync](http://www.browsersync.io/)
* live editing assisted by the build tool
	* uses a watch task(?)
	* Creates/proxies a local web server
	* Compatible with gulp and other node-based tools
`npm install -g browser-sync`
* started in a few different ways
	* `browser-sync start --server --files "css/*.css"` for static sites
	* `browser-sync start --proxy "myproject.dev" --files "css/*.css"`
		* will wrap the vhost with a proxy URL
	* With Gulp
		```
		var browserSync = require('browser-sync').create();
		browserSync.init({
			server: "./"
		});
		browserSync.stream();
		```
The complete `gulpfile.js`
```
var  gulp  =  require('gulp');
var  sass  =  require('gulp-sass');
var  autoprefixer  =  require('gulp-autoprefixer');
var  browserSync  =  require('browser-sync').create();
  
gulp.task('default', ['styles'], function() {
	gulp.watch('sass/**/*.scss', ['styles']);  
	browserSync.init({
		server:  './'
	});
});
  
gulp.task('styles', function() {
	gulp.src('sass/**/*.scss')
		.pipe(sass().on('error', sass.logError))
		.pipe(autoprefixer({
			browsers: ['last 2 versions']
		}))
		.pipe(gulp.dest('./css'))
		.pipe(browserSync.stream());
});
```

## Preventing Disasters
[Comparison of JavaScript linting tools](http://www.sitepoint.com/comparison-javascript-linting-tools/)
[ESLint](http://eslint.org/)

Linting comes in two varieties:
1. Syntax (unreachable code, etc)
2. Coding Style (bracket placement, etc)

### [ESLint](http://eslint.org/) Setup
1. `npm install -g eslint`
2. Install editor plugins
	* for Sublime "Sublime Linter" and "SublimeLinter-eslint"
	* For Code: "ESLint" - or that's what code recommended.
3. `eslint --init` in the project directory
	* Creates `.eslintrc` file
#### [gulp-eslint](https://www.npmjs.com/package/gulp-eslint)

1. `npm install gulp-eslint`
2. Example `gulpfile.js`
```
const  {src,  task}  =  require('gulp');
const  eslint  =  require('gulp-eslint');
task('default',  ()  =>  {
	 return  src(['scripts/*.js'])
	 // eslint() attaches the lint output to the "eslint" property
	 // of the file object so it can be used by other modules.
	 .pipe(eslint())
	 // eslint.format() outputs the lint results to the console.
	 // Alternatively use eslint.formatEach() (see Docs).
	 .pipe(eslint.format())
	 // To have the process exit with an error code (1) on
	 // lint error, return the stream and pipe to failAfterError last.
	 .pipe(eslint.failAfterError());
});
```

```
var  eslint  =  require('gulp-eslint');
gulp.task('lint', function () {
	return  gulp.src(['js/**/*.js'])
	// eslint() attaches the lint output to the eslint property
	// of the file object so it can be used by other modules.
	.pipe(eslint())
	// eslint.format() outputs the lint results to the console.
	// Alternatively use eslint.formatEach() (see Docs).
	.pipe(eslint.format())
	// To have the process exit with an error code (1) on
	// lint error, return the stream and pipe to failOnError last.
	.pipe(eslint.failOnError());
});
//Updated default task:
gulp.task('default', ['styles', 'lint'], function(){
	gulp.watch('sass/**/*.scss', ['styles]);
	gulp.watch('js/**/*.js', [lint]);
	browserSync.init({server:'./'});
});
```

### TODO: PreCommit hooks
### Unit Testing in Gulp
Udacity [JavaScript Testing course](https://www.udacity.com/course/javascript-testing--ud549)

PhantomJs
* a small web-kit based (virtual browser?)
* Must be installed outside of node before installing the gulp plugin
* [The installer](http://phantomjs.org/download.html) is a [zip file](https://bitbucket.org/ariya/phantomjs/downloads/phantomjs-2.1.1-windows.zip)
* It's bin folder needs to be added to the command path
* See also [minijasminenode](https://www.npmjs.com/package/minijasminenode2)
	* `npm install -g minijasminenode2` (the 2 is optional)
* `npm install --save-dev gulp-jasmine-phantom`
	*	Works with either PhantomJS or minijasminenode2
*	In `gulpfile.js`
	*	`var jasmine = require('gulp-jasmine-phantom')`
		```
		gulp.task('tests', function(){
			gulp.src('tests/pece/extraSpec.js')
				.pipe(jasmine({
					integration: true, // false=run in node
					vendor: 'js/**/*.js'
				}));
		});
		```
	* Testing is usually too slow to run with a `gulp.watch()`.
		* [Continuous Integration](https://classroom.udacity.com/courses/ud611/lessons/4225318865/concepts/44585989490923) in Udacity Intro to DevOps course
		* The basic solution is to run the tests in the cloud after every commit.


## Optimizations
### Separating Dist and Dev files
1. `mkdir dist`
2. Fix the 'styles' task: set the output (dest) pipe to the correct folder
	* `.pipe(gulp.dest('dist/css'))`
3. Create new tasks to copy the HTML and images.
```
gulp.task('copy-html',function(){
	gulp.src('./index.html')
		.pipe(gulp.dest('./dist'));
});
gulp.task('copy-images', function(){
	gulp.src('img/*')
		.pipe(gulp.dest('dist/img'));
});
```
4. Update the 'default' task
```
gulp.task('default',['copy-html', 'copy-images', 'styles', 'lint'...
	...
	gulp.watch('./index.html', ['copy-html']);
	...
	browserSync.init({server: './dist'});
```
5. Could also add a `gulp.watch()` for 'copy-images' but those don't change very often.
6. adding a watch for 'index.html' to update browserSync.
	* `gulp.watch('./build/index.html').on('change', browserSync.reload)`

### Concatenation and minification
1. Sass does both for CSS
	* use `@import` in CSS and sass will combine the files into a single stream
	* change the pipe: `.pipe(sass({outputStyle: 'compressed'}))`
2. Gulp-concat can concatenate the JavaScript
	* `npm install gulp-concat`
	* create a 'scripts' task or two (i.e. 'scripts-dist')
		```
		gulp.task('scripts', function(){
			gulp.src('js/**/*.js')
				.pipe(concat('all.js'))
				.pipe(gulp.dest('dist/js');
		});
		```
	* Update the references in the source file (`<script>` tags)
3.  Uglify minifies the Javascript
	* Add a pipe in the 'scripts-dist' task:
	* ` pipe(uglify())`
4. Create a 'dist' task to run all of the tasks except 'scripts'. (use 'scripts-dist' instead).
TODO: [The Difference Between Minification and GZipping](https://css-tricks.com/the-difference-between-minification-and-gzipping/)
5. Babel.js can transpile
	* `npm install gulp-babel`
	* `var babel = require('gulp-babel');`
	* `.pipe(babel())` after the `gulp.src` but before the concatenation. In `gulp.task('script*`
6. `'gulp-sourcemaps'` [plugin](https://www.npmjs.com/package/gulp-sourcemaps)
	* 		
		```
		var sourcemaps = require('gulp-sourcemaps');

		 gulp.task('scripts-dist', function() {
		   gulp.src('js/**/*.js')
		    .pipe(sourcemaps.init())
		    .pipe(concat('all.js'))
		    .pipe(uglify())
		    .pipe(sourcemaps.write())
		    .pipe(gulp.dest('dist/js'));
		});
		```

>Source map Support for other languages
In addition to things like concatenation and minification, source maps also support some languages/extensions that transpile to JavaScript like Typescript, CoffeeScript and ES6 / JSX.
You can read more some of the technical aspects of Source Maps on  [HTML5Rocks](http://www.html5rocks.com/en/tutorials/developertools/sourcemaps/).

### Additional optimization info
[Browser Rendering Optimization](https://www.udacity.com/course/browser-rendering-optimization--ud860)
[Web Performance Optimization](https://www.udacity.com/course/website-performance-optimization--ud884)
[Responsive Web Design](https://www.udacity.com/course/responsive-web-design-fundamentals--ud893)
[Responsive Images](https://www.udacity.com/course/responsive-images--ud882)

### Image Compression
`gulp-imagemin` performs lossless compression.
* just import it in the gulp file and add a `.pipe(imagemin())`

`imagemin-pngquant` is a plugin for `gulp-imagemin`

```
	gulp.task('default', function() {
	    return gulp.src('src/images/*')
	        .pipe(imagemin({
	            progressive: true,
	            use: [pngquant()]
	        }))
	        .pipe(gulp.dest('dist/images'));
	});
```
Other options
* Use more aggressive compression for smaller images.
* Use SVG where applicable.
* Advanced technique
	* Automatically resizing images, inlining images into CSS or a sprite

## Scaffolding
HTML5 Boilerplate - Simplest
[Web Starter Kit](https://developers.google.com/web/tools/starter-kit/) - By Google, opinionated.
[Yeoman](http://yeoman.io/) - more advanced.




.
.
.
.
.
.



> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTExOTQwNDA0NzFdfQ==
-->