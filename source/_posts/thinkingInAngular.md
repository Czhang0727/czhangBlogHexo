---
title: Thinking in Angular
date: 2016-12-05 18:31:32
tags: ["code", "front-end"]
---

So far I used Angular for a while, the whole framework changed a lot, since the first time I played with it.
Now, Angular released 2.1, we should see Angular 2 is huge different from angular 1.4

{% img myclass angular2_logo.png 300 300 Angular JS %}

``` typescript
	// Some simple typescript
	class ClassA{
		msg : string;
		constructor(msg : string){
			this.msg = msg;
		}
		speak(){
			return this.msg;
		}
	}
```

well, my understand for typescript is.... It is still Javascript, for most function in Javascript we just can direct call the same name. Like our favorite console.log("HEY!").

Angular 2 begin to use component (they said this is a kind of directive). It sounds like react, combine with script and UI template, create our own tag for HTML template.

For me ,Angular 2 is far more than a framework, I can spend a whole day to tell some guy what is Angular, what did it do, and how it works. Because while I was in interview, some one ask me how we compare Angular to...JQuery. OK, what should I say....

Mean stack development is really popular now, thanks to the power of Node JS, I don't need to write two language in same Application!

Here is a simple way I can put Angular 2 with web-pack. 

``` bash
	npm install -g angular-cli
	ng new [PROJECT_NAME]
```

Enjoy your Angular 2 development lol.


