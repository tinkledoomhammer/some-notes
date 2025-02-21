
# JavaScript Promises
[https://www.udacity.com/course/javascript-promises--ud898]

## Creating Promises
A promise can be in 4 stages
1. Fulfilled(resolved) : It worked
2. Rejected : It didn't work
3. Pending: Still waiting
4. Settled: something happened

A promise is a try-catch wrapper around code that will complete at a later time.
```
new Promise(function(resolve, reject){
	var img = document.createElement('img');
	img.src = 'img.jpg';
	img.onload = resolve;
	img.onerror = reject;
	document.body.appendChild(img);
})
.then(finishLoading)
.catch(showAlternateImage);
```
Values passed to `resolve` or `reject` are passed on to the arguments of `.then()` or `.catch()`. 
If the value that gets passed is a promse, then it will be settled and the result will be passed.

Bower has been depricated, and 



>npm WARN deprecated bower@1.8.4: We don't recommend using Bower for new projects. Please consider Yarn and Webpack or Parcel. You can read how to migrate legacy project here: https://bower.io/blog/2017/how-to-migrate-away-from-bower/
npm WARN deprecated gulp-util@3.0.8: gulp-util is deprecated - replace it, following the guidelines at https://medium.com/gulpjs/gulp-util-ca3b1f9f9ac5
npm WARN deprecated graceful-fs@3.0.11: please upgrade to graceful-fs 4 for compatibility with current and future versions of Node.js
npm WARN deprecated minimatch@2.0.10: Please update to minimatch 3.0.2 or higher to avoid a RegExp DoS issue
npm WARN deprecated minimatch@0.2.14: Please update to minimatch 3.0.2 or higher to avoid a RegExp DoS issue
npm WARN deprecated graceful-fs@1.2.3: please upgrade to graceful-fs 4 for compatibility with current and future versions of Node.js

It looks like everything works on linux. The easiest solution is probably to do the console tasks on a vm and use vscode-remote-workspace or something to edit files.

Instead of using code over ssh, could put the node in the shared folder and create links for node_modules back to `~` or something. That does require running vagrant as admin.

Running git on vm's is a bit of a pain.

The source folders could probably be linked from a project in the vm's home folder to the shared folder. Not sure how git and node work with symbolic links, though.  This option would not require admin privileges. 

Using SSH to connect code to the source folder seems like the best option. Could probably use git to push changes to the shared folder.

Just got it working (?)
After installing the plugin, its `F1` then select "remote workspace: open ..."
Then in the forst box "sftp://vagrant@localhost:2222/home/vagrant/?key=F:/VM/Vagrant/getting_started/node/.vagrant/machines/default/virtualbox/private_key"
and in the next box, a display name for the folder.

The .code-workspace file doesn't seem to work correctly.  Open folders were saved in the file, even when it was moved to another location. It did not automatically add the remote folders in the file at all.


## Chaining Promises
Promises provide two methods that return thenables:
* `.then(function(args){;})` - for resolved promises
* `.catch(function(args){;})` - for rejected promises
The functions that get passed in can values that are passed along.
The return values can have `.then()` attached to build chains.
The return from inside a `.then()` will be passed to an argument to the next `.then()`.


`.then(resolve, reject)` is also a thing.
* (Implying that the normal version is `.then(resolve)`)
* using `.then(resolve).catch(reject)` is recommended
	* The order is different. An error thrown from `resolve` will not get caught by `reject` in `.then(resolve, reject)`. It will in `.then(resolve).catch(reject)` however.

Resolve does not always mean success. For example `fetch` will `resolve` a header with an HTTP	 error code. The `resolve` handler can check the status code of the response object, and throw an error with `Error(object_or_message)`. In that case, the original promise has already `resolve`d and cannot `reject`, so `.then(resolve).catch(reject)` can catch errors thrown by `fetch` or by the `resolve`

### `Promise.all`
`Promise.all(arrayOfPromises).then(function(array of Values)){;})`
* `reject`s when the first promise rejects
* else, it `resolve`s after all of the promises have resolved.
* The results seem to be in the same order as the array that was passed. 

```
getJSON('../data/earth-like-results.json')
	.then((resp)=>{
		let  list=resp.results.map(getJSON);
		list[0]=list[0].then(createPlanetThumb);
		for(let  x=1; x<list.length; x++){
			list[x]=Promise.all([list[x-1], list[x]])
			.then((results)=>{
				createPlanetThumb(results[1]);
			});
		}
	})
```


## Decorating with promises
[https://innolitics.com/articles/javascript-decorators-for-promise-returning-functions/]

.
.
.
.
.
.
.
.
.



> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbNzE3NjY4NzUzXX0=
-->