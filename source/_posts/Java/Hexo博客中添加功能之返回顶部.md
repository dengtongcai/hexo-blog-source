layout: '[layout]'
title: Hexo博客中添加功能之返回顶部
date: 2016-06-03 22:28:53
categories: 教程
tags: [Hexo,插件]
---
当浏览长博文时，可能要回到博文起始位置，所以需要在博客添加一个返回顶部的功能按钮，在Google之后找到了很多教程，在这里分享一下我用的返回顶部功能的实现过程。
hexo中实现返回顶部比较简单，因为hexo所搭建的博客都已经模块化，只需要写或者修改HTML文件、JS文件，然后网页进行引用即可。
## 添加HTML代码 #
新建totop.ejs文件 `/themes/yilia/layout/_partial/totop.ejs`，在此新建文件中添加如下HTML代码，给出返回顶部按钮的位置以及图片：
```
<div id="totop" style="position:fixed;bottom:150px;right:50px;cursor: pointer;">
<a title="返回顶部"><img src="/img/scrollup.png"/></a>
</div>
```
## 添加JS代码 #
新建totop.js文件 `/themes/yilia/source/js/totop.js`，在此新建文件中添加如下JS代码，给出了返回顶部按钮的显示位置(upperLimit)和响应速度(scrollSpeed)：
```
(function($) {
	// When to show the scroll link
	// higher number = scroll link appears further down the page   
	var upperLimit = 1000;

	// Our scroll link element
	var scrollElem = $('#totop');

	// Scroll to top speed
	var scrollSpeed = 500;

	// Show and hide the scroll to top link based on scroll position   
	scrollElem.hide();
	$(window).scroll(function() {
		var scrollTop = $(document).scrollTop();
		if (scrollTop > upperLimit) {
			$(scrollElem).stop().fadeTo(300, 1); // fade back in           
		} else {
			$(scrollElem).stop().fadeTo(300, 0); // fade out
		}
	});

	// Scroll to top animation on click
	$(scrollElem).click(function() {
		$('html, body').animate({
			scrollTop: 0
		},
		scrollSpeed);
		return false;
	});
})(jQuery);
```
## 添加对HTML和JS文件的引用 #

   修改 `themes/yilia/layout/_partial/after_footer.ejs` 文件，在文件末尾添加如下两行代码：
```
<%- partial('totop') %>
<script src="<%- config.root %>js/totop.js"></script>
 ```
## 添加按钮图片 #

   网上Google一个回到顶部图标，复制到 /themes/yilia/source/imgs 目录下，文件名为 scrollup.png。
可以自己找，可以找一个比较显眼的或者与界面色调比较搭配的，图标后缀记得是`.png`,效果就是滚动显示，停下就隐藏。
地址我给一个参考：http://www.lanrentuku.com/gif/a/top.html