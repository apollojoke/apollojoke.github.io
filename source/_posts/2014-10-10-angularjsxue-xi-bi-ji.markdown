---
layout: post
title: "AngularJS学习笔记"
date: 2014-10-10 16:49:10 +0800
comments: true
categories: AngularJS JavaScript summary
---

这两天被项目上拉去填坑，得恶补下AngularJS，这段时间持续更新这篇学习心得。

我随便找了个网上的快速入门，简单翻了下，感觉可以看看入门。作为一个很火的前端框架，AngularJS远不止教程中提到的这些。  [链接](http://www.ituring.com.cn/minibook/303)

---------------------

####Hello World
Hello World 例程：

	<!doctype html>
		<html ng-app>
    	<head>
        	<script src="http://code.angularjs.org/angular-1.0.1.min.js"></script>
    	</head>
    	<body>
        	Your name: <input type="text" ng-model="yourname" placeholder="World">
        	<hr>
        	Hello {{yourname || 'World'}}!
    	</body>
	</html>


总结：
	
1. 标记``ng-app``告诉AngularJS处理整个HTML页并引导应用；

	在``<html>``中添加``ng-app``属性即说明整个``<html>``都是AngularJS脚本作用域。开发者也可以在局部使用ng-app指令，如``<div ng-app>``，则AngularJS脚本仅在该``<div>``中运行。

2. 载入AngularJS脚本；

	``<script src="http://code.angularjs.org/angular-1.0.1.min.js"></script>``
	
	这行代码载入angular.js脚本，当浏览器将整个HTML页面载入完毕后将会执行该angular.js脚本，angular.js脚本运行后将会寻找含有ng-app指令的HTML标签，该标签即定义了AngularJS应用的作用域。
	
3. 指令``<input ng-model="yourname" />``绑定到一个叫yourname的模型变量；

4. AngularJS模板的核心功能——绑定。由双大括号``{{ }}``和变量yourname组成；
	
	AngularJS表达式仅在AngularJS的作用域中运行，而不是在整个DOM中运行。
	
	输入框的任何更改会立即反映到模型变量（一个方向），模型变量的任何更改都会立即反映到问候语文本中（另一方向），这就是AngularJS的双向绑定；
	


####MVC in AngularJS

以下的例子，用AngularJS添加一些模型-视图-控制器。

视图（可以理解为显示的页面）：

	<html lang="en" ng-app="phonecatApp">
	<head>
		<title>Google Phone Gallery</title>
		<script src="lib/angular/angular.js"></script>
		<script src="js/controllers.js"></script>
	</head>
	<body ng-controller="PhoneListCtrl">
		<ul>
    		<li ng-repeat="phone in phones">
      			<p>{{phone.name}}</p>
      			<p>{{phone.snippet}}</p>
    		</li>
    	</ul>
	</body>
	</html>
	

控制器和模型：

	var phonecatApp = angular.module('phonecatApp', []);
	phonecatApp.controller('PhoneListCtrl', function($scope) {
		$scope.phones = [
			{'name': 'Nexus S', 'snippet': 'Fast just got faster with Nexus S.'},
			{'name': 'Motorola XOOM™ with Wi-Fi', 'snippet': 'The Next, Next Generation tablet.'},
    		{'name': 'MOTOROLA XOOM™', 'snippet': 'The Next, Next Generation tablet.'}
    	];
	});

总结：

视图里

1. ``<html lang="en" ng-app="phonecatApp">``定义AngularJS作用域；
2. ``<script src="js/controllers.js"></script>`` 引入了包含controller的js文件；
3. ``<body ng-controller="PhoneListCtrl">``指定对应的controller；
4. ``<li>``里使用AngularJS迭代器``ng-repeat="phone in phones"``；
5. ``{{phone.name}}``和``{{phone.snippet}}``是数据模型引用，在控制器里可以找到模型的定义；

控制器里

1. 使用``angular.module()``绑定一个ng-app，在视图里声明，在这里绑定；
2. 为app里注册一个controller，我想这与在视图中使用同名的ng-app，应该是有相同的作用域之类的关联；PhoneListCtrl为控制器的名字，使用时需要与视图中ngController指令值相匹配；
3. ``phonecatApp.controller('PhoneListCtrl', function($scope){}``里的``$scope``是控制器的作用域，这个例子里就是``<body>``标签，所以在``$scope``里初始化的``phones``,在``<body>``标签中可用。这里值得注意的是controller定义在里ng-app里面，controller里面定义的ng-model在controller外无法使用；


####迭代器过滤

在上一个例子的视图代码中，迭代器部分修改为：
	
	<html lang="en" ng-app="phonecatApp">
	<head>
		<title>Google Phone Gallery</title>
		<script src="lib/angular/angular.js"></script>
		<script src="js/controllers.js"></script>
	</head>
	<body ng-controller="PhoneListCtrl">
		<div class="span2">
	      Search: <input ng-model="query">
    	</div>
		<ul>
    		<li ng-repeat="phone in phones" | filter:query>
      			<p>{{phone.name}}</p>
      			<p>{{phone.snippet}}</p>
    		</li>
    	</ul>
	</body>
	</html>
	
总结：

1. 使用AngularJS的``$filter``函数来处理ngRepeat指令的输入。过滤出包含{{query}}的内容，
2. ``ng-model="query"``只在controller的作用域，而不是ng-app的作用域，在title里就没法用query这个model
