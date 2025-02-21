
# Asynchronous JavaScript Requests
[https://classroom.udacity.com/courses/ud109]
## Ajax with XHR
### `XMLHttpRequest`
[https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest/open]
* Used as a constructor: `let myReqObj=XMLHttpRequest()`
###  `myReqObj.open(method, url, async=false, user, password);`
* `method` is an HTTP method, usually `'GET'` or `'PUT'`.
* [UD303 HTTP & Web Servers](https://classroom.udacity.com/courses/ud303)
* `url` is subject to same-origin policy
* [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)(Cross-Origin Resource Sharing)
	* Can accept 2-5 parameters.

###  `myReqObj.onload=handler;`
* Executed when a response is received
* `this.responseText` is the response

### `myReqObj.onError=handler;`
### `myReqObj.setRequestHeader(headerName,value);`
###  `myReqObj.send()`
### `JSON.parse(str)`
* Parses a text string containing JSON data and returns a JavaScript object (?)

### Lesson 1 project
`git clone https://github.com/udacity/course-ajax.git`
#### Create accounts
Unsplash
-   Create a developer account here -  [https://unsplash.com/developers]
-   Next, create an application here -  [https://unsplash.com/oauth/applications]
    -   this will give you an "Application ID" that you'll need to make requests

The New York Times
* Create a developer account here -  [https://developer.nytimes.com/]
* They'll email you your api-key (you'll need this to make requests)
* Here's your API Key: 5f3d460547af4321bf9b2c72277b5b2a
#### Unsplash request
```
function addImage(){}
const searchedForText = 'hippos';
const unsplashRequest = new XMLHttpRequest();
let url='https://api.unsplash.com/searhc/photos'
	+'?page=1&query='+searchedForText;
unsplashRequest.onload = addImage;
unsplashRequest.open('GET', url);
unsplash.send();
```

Adding an image as a search result:
```
form.addEventListener('submit', function (e) {
	e.preventDefault();
	responseContainer.innerHTML  =  '';
	searchedForText  =  searchField.value;
	function  addImage(){
		//console.log(this.responseText);
		const  data  =  JSON.parse(this.responseText);
		const  firstImage  =  data.results[0];
		let  htmlContent='';
		if(data  &&  data.results  &&  data.results[0])
		htmlContent  =  "<figure> <img src = '"+firstImage.urls.regular
		+"' alt='"+searchedForText+"'>"
		+"<figcaption>"+searchedForText+" by "+firstImage.user.name
		+"</figcaption></figure>";
		responseContainer.insertAdjacentHTML('afterbegin', htmlContent);
	}
	const  unsplash  =  new  XMLHttpRequest();
	let  url='https://api.unsplash.com/search/photos'
		+'?page=1&query='+searchedForText;
	unsplash.open('GET', url);
	unsplash.onload  =  addImage;
	unsplash.setRequestHeader('Authorization', 'Client-ID 28cdfcb20c1ffd1ee6b926f2d71d747c399bb5fe29f1d337a8790db9fc96a761');
	unsplash.send();
	console.log("Form submitted");
});
```

### New Features 
[HTML5Rocks article](http://www.html5rocks.com/en/tutorials/file/xhr2/) new tricks in XHR2

## Ajax with JQuery
Udacity's [UD245-Intro to jQuery](https://www.udacity.com/course/intro-to-jquery--ud245)
`$.ajax(url,config);`;
or
`$ajax(config);`

### `$.ajax(config)'` config properties
#### `.url: `
#### `.headers: {key: value}`
* To set the request's headers.
#### All of the parameters to `XMLHttpRequest.open()` can be included by name
* `type`, `url`, `async`, `username`, `password`
#### `.xhrFields`
* An object containing properties to set on the underlying XHR object
* They are set after opening but before sending.

* 
### `$.ajax().done(responseHandler);`
* The handler receives one parameter: the Parsed JSON of the response.
* The `.response` property contains the response body text.


### Debugging in Chrome
-   [Pause Your Code With Breakpoints](https://developers.google.com/web/tools/chrome-devtools/javascript/breakpoints)
-   [JavaScript Debugging Reference](https://developers.google.com/web/tools/chrome-devtools/javascript/reference)


## Ajax with Fetch API
* Fetch uses promises. [https://www.udacity.com/course/javascript-promises--ud898]
* `fetch(url,config).then(handler);`
*  `fetch(url,config).then( response => response.json()).then(realHandler);`
* or even 
```javascript 
	fetch(url,config)
	.then(response => response.json())
	.then(realHandler)
	.catch(e=>HandleError(e,stuff));
```
* In the above example, `stuff` is anything available in the lexical scope that should be passed to the error handler (i.e. an indicator of which request caused the error.)
* The config object seems to work a lot like the JQuery config object.
* headers can be included with the `headers` property
	* Can use the Headers API or just an object 


[ES6 course](https://classroom.udacity.com/courses/ud356)




.
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
eyJoaXN0b3J5IjpbMTk2MTY1Mzk3XX0=
-->