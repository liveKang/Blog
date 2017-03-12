## angularJs实践 之 vm大法
### 前言
讲到controllerAs不得不提下的就是[angularjs最佳实践](https://github.com/johnpapa/angular-styleguide/blob/master/a1/README.md)。这是总监极力推荐的最佳实践，还记得七月的某天，我和技术总监还在为前端架构规范争论，因为我坚持用自己的实践经验，那时的我们还在为精品活动app持续加班。混合app现如今转为了原生开发了，从混合app整理出一套wap端代码，依旧是ionic框架，去掉了cordova的部分。

比如controller的写法：

<pre>
(function() {
	'use strict';
    angular.module('app')
        //消息
        .controller('UserNotice', UserNotice);

    UserNotice.$inject = ['$scope', '$state'];

    function UserNotice($scope, $state) {
        var vm = this;
        vm.test = "hello";

        vm.goBack = goBack;
        vm.init = init; //获取中奖记录列表
        vm.doRefresh = doRefresh; //下拉刷新
        vm.loadMore = loadMore; //下拉加载更多

        activate();

        function activate() {
            init();
        }

        function goBack() {
            /////
        }

        function init() {
            /////
        }

        function doRefresh() {
            /////
        }

        function loadMore() {
            /////
        }

    }
})();
</pre>

这样的写法，其实还是比较清晰的，所有的函数在打开该文件时就能全部看到，易读性比较好，也不会有$scope 执行的顺序问题。

### controllerAs

这样的写法本来也没什么问题，问题在路由的里面。当时一致认为controllerAs起到的是一个父控制器的作用，架构搭建的时候，所有的路由都加上了`controllerAs：vm`，随着后面的开发人员更替，很多的路由中没有使用到父控制器，所以这段代码渐渐被删除或弃用，也是自那时开始，这个vm就被大家感觉不好使了，有各种各样的问题。还记得一个同事，用双向数据绑定证明向总监证明vm是有问题的，两人在哪里倒腾了好久，就是在controller中`vm.test = "hello"`，然后在view层页面中`{{vm.test}}`不能输出相应的数据。

#### controllerAs是什么？
[angularJS官方文档说明](https://docs.angularjs.org/api/ng/directive/ngController)

> * one binds methods and properties directly onto the controller using this: ng-controller="SettingsController1 as settings"
* one injects $scope into the controller: ng-controller="SettingsController2"
* Using controller as makes it obvious which controller you are accessing in the template when multiple controllers apply to an element.
* If you are writing your controllers as classes you have easier access to the properties and methods, which will appear on the scope, from inside the controller code.
* Since there is always a . in the bindings, you don't have to worry about prototypal inheritance masking primitives.

使用 controllerAs 时，可以将 Controller 定义成 Javascript 的原型类，在 HTML 视图中直接绑定原型类的属性和方法,这样的优点是可以使用 Javascript 的原型类，我们可以使用更加高级的 ES6 或者 TypeScript 来编写 Controller，避开了所谓的 child scope 原型继承带来的一些问题， 具体可以参考这里[Understanding Scopes](https://github.com/angular/angular.js/wiki/Understanding-Scopes)。

#### vm.test 再测试

* 在controller中准备好`vm.test = "hello";`
<pre>
vm.test = "hello";
vm.goBack = goBack;
vm.init = init; //获取中奖记录列表
vm.doRefresh = doRefresh; //下拉刷新
vm.loadMore = loadMore; //下拉加载更多
vm.setReaded = setReaded; //设为已读和已读信息跳转
vm.unReadedGo = unReadedGo; //未读信息跳转
vm.setAllReaded = setAllReaded; //一键已读
</pre>

* 在页面中准备好`{{vm.test}}`

`
<div class="clearfix bgWhite rn-notice-div2" ng-repeat="item in items track by $index">
		{{vm.test}}
		<div ng-if="item.status=='0' && item.content.indexOf('中奖了')>0" class="clearfix borderBottom" ng-click="vm.setReaded($index)">
			<p class="rn-notice-p3">{{item.content}}</p>
			<p class="rn-notice-p2">{{item.createTime | timeTypeNoS}}</p>
	    </div>
	    </div>
</div>
`

* 在路由中准备好`controllerAs: vm`

<pre>
//消息通知
    .state('notice', {
        url: '/notice',
        cache: false,
        templateUrl: 'user/userNotice.html',
        controller: 'UserNotice',
        controllerAs: 'vm'
    })
</pre>

* 结果

![hello](http://p1.bqimg.com/567571/28871e5634bed5b6.png)
