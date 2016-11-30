---
title: My Blog Finally Created!!
date: 2016-11-29 22:24:27
tags: "hexo"
---
<style type="text/css">
	

</style>

# Initialize my Post
I was thinking about create my personal blog for a really long time...
However, as we all know, programmer normally lazy :(

Yesterday I found hexo, and... IT IS SO AWESOME, make a personal site so ez lol. For I can easily modify just like HTML :) such as:

{% bilibili 2414003 1 %}

Let's do like a coder!

``` python
	# this is a test code block here
	print("Hello World")
```
Here I am going to show you how we build the site
## Learn hexo
Do some google to learn Markdown, which is important
## Install hexo
Since hexo is on npm.....so that is how I installed
``` bash
	npm install hexo-cli -g
	hexo init <your blog Name>
```
Here we go! we got our site, Well I know you are wondering the theme looks different. All right, let's add a theme

## Add a theme
First of all, we need to find a theme....Peraonly, I highly recommanded using NexT,
<a href="https://github.com/iissnan/hexo-theme-next">Click to get NexT</a>
After we download NexT, the only thing we need to do is copy it to our theme folder, well, then you also need to config _config.yml, let hexo know, we are using next theme :)
``` bash
	theme: next
```

## Push to Github Page
Hope you already have the page, the only thing we need to do is

``` bash
	hexo generate
```
then copy all files in public into your github lol


Enjoy!
