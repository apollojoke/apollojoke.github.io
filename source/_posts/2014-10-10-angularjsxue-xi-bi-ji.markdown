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

###Hello World
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
	
---------------------

###MVC in AngularJS

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

---------------------

###迭代器过滤

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

-----------------

###双向绑定

模板代码：

	Search: <input ng-model="query">
	Sort by:
	<select ng-model="orderProp">
		<option value="name">Alphabetical</option>
		<option value="age">Newest</option>
	</select>
	<ul class="phones">
		<li ng-repeat="phone in phones | filter:query | orderBy:orderProp">
    		{{phone.name}}
    		<p>{{phone.snippet}}</p>
		</li>
	</ul>
	
控制器代码：

	function PhoneListCtrl($scope) {
		$scope.phones = [
    		{"name": "Nexus S", "snippet": "Fast just got faster with Nexus S.", "age": 0},
    		{"name": "Motorola XOOM™ with Wi-Fi", "snippet": "The Next, Next Generation tablet.", "age": 1},
    		{"name": "MOTOROLA XOOM™", "snippet": "The Next, Next Generation tablet.", "age": 2}
    	];
		$scope.orderProp = 'age';
	}

总结：

1. 在模板中，添加了一个名为``orderProp``的``<select>``标签，并在迭代器中加入了``orderBy:orderProp``过滤器。AngularJS在``select``元素和``orderProp``模型之间创建了一个双向绑定。而后，``orderProp``会被用作``orderBy``过滤器的输入；
2. 修改了``phones``模型，添加了``age``属性，排序用；
3. 在congtroller的作用域中初始化了``orderProp``，使其值为age。因为``orderProp``的值是双向绑定的，所以应用在浏览器中加载时，``Newes``在下拉菜单中默认被选中。这是因为我们在控制器中把``orderProp``设置成了``age``。所以绑定在从我们模型到用户界面的方向上起作用——即<font color='red'>数据从模型到视图的绑定</font>。
现在当你在下拉菜单中选择``Alphabetically``，数据模型会被同时更新，并且页面会被重新排序。这个时候数据绑定从另一个方向产生了作用——即<font color='red'>数据从视图到模型的绑定</font>。


-------------------
###XHR 和 依赖注入

修改controller代码为：

	phonecatApp.controller('PhoneListCtrl', function($scope, $http) {
		$http.get('phones/phones.json').success(function(data) {
    		$scope.phones = data;
    	});

		$scope.orderProp = 'age';
	});
	//PhoneListCtrl.$inject = ['$scope', '$http'];
	
原来的``phones``数据hard code在代码中，现在用一个ajax请求取回数据。

所谓的依赖注入，我理解为，这段代码中依赖``$http``，最原始的做法是``var http = require(*/*/http.js)``然后再使用``http``变量，这相当于在自己的代码里new一个依赖的组件，现在依赖注入，就是直接将依赖传入到function中。（``$http``是AngularJS众多内建服务中之一）。

有一个文章,很好的解释了依赖注入。  [链接](http://angularjs.cn/A00z)

``$``前缀命名习惯:

在AngularJS中你可以创建自己的服务。作为一个命名习惯，AngularJS内建服务，作用域方法，以及一些其他的AngularJS API都在名字前面使用一个‘$’前缀。不要使用‘$’前缀来命名你自己的服务和模型，否则可能会产生名字冲突。

关于JS压缩：

由于AngularJS是通过控制器构造函数的参数名字来推断依赖服务名称的。所以如果你要压缩``PhoneListCtrl``控制器的JS代码，它所有的参数也同时会被压缩，这时候依赖注入系统就不能正确的识别出服务了。

为了克服压缩引起的问题，只要在控制器函数里面给$inject属性赋值一个依赖服务标识符的数组，就像被注释掉那段最后一行那样

	PhoneListCtrl.$inject = ['$scope', '$http'];
	
另一种方法也可以用来指定依赖列表并且避免压缩问题——使用Javascript数组方式构造控制器：把要注入的服务放到一个字符串数组（代表依赖的名字）里，数组最后一个元素是控制器的方法函数：

	var PhoneListCtrl = ['$scope', '$http', function($scope, $http) { /* constructor body */ }];
