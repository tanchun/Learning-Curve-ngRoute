# Learning-Curve-ngRoute
ngRoute 学习路程


ngRoute包括的内容
　　ng的路由机制是靠ngRoute提供的，通过hash和history两种方式实现了路由，可以检测浏览器是否支持history来灵活调用相应的方式。ng的路由(ngRoute)是一个单独的模块，包含以下内容：

服务$routeProvider用来定义一个路由表，即地址栏与视图模板的映射
服务$routeParams保存了地址栏中的参数，例如{id : 1, name : 'tom'}
服务$route完成路由匹配，并且提供路由相关的属性访问及事件，如访问当前路由对应的controller
指令ngView用来在主视图中指定加载子视图的区域
　　以上内容再加上$location服务，我们就可以实现一个单页面应用了。下面来看一下具体如何使用这些内容。

使用ng的路由机制
　　第一步：引入文件和依赖
　　ngRoute模块包含在一个单独的文件中，所以第一步需要在页面上引入这个文件，如下：
<script src="http://code.angularjs.org/1.2.5/angular.min.js"></script>
<script src="http://code.angularjs.org/1.2.5/angular-route.min.js"></script>
　　光引入还不够，我们还需在模块声明中注入对ngRoute的依赖，如下：

var app = angular.module('MyApp', ['ngRoute']);
　　完成了这些，我们就可以在模板或是controller中使用上面的服务和指令了。下面我们需要定义一个路由表。

　　第二步：定义路由表
　　$routeProvider提供了定义路由表的服务，它有两个核心方法，when(path,route)和otherwise(params)，先看一下核心中的核心when(path,route)方法。

　　when(path,route)方法接收两个参数，path是一个string类型，表示该条路由规则所匹配的路径，它将与地址栏的内容($location.path)值进行匹配。如果需要匹配参数，可以在path中使用冒号加名称的方式，如：path为/show/:name，如果地址栏是/show/tom，那么参数name和所对应的值tom便会被保存在$routeParams中，像这样：{name : tom}。我们也可以用*进行模糊匹配，如：/show*/:name将匹配/showInfo/tom。

　　route参数是一个object，用来指定当path匹配后所需的一系列配置项，包括以下内容：

复制代码
controller //function或string类型。在当前模板上执行的controller函数，生成新的scope
controllerAs //string类型，为controller指定别名
template //string或function类型，视图所用的模板，这部分内容将被ngView引用
templateUrl //string或function类型，当视图模板为单独的html文件或是使用了<script type="text/ng-template">定义模板时使用
resolve //指定当前controller所依赖的其他模块
redirectTo //重定向的地址
复制代码
　　最简单情况，我们定义一个html文件为模板，并初始化一个指定的controller：

复制代码
function emailRouteConfig($routeProvider){
    $routeProvider.
    when('/show', {
        controller: ShowController,
        templateUrl: 'show.html'
    }).
    when('/put/:name',{
       controller: PutController,
       templateUrl: 'put.html'
    });  
};
复制代码
　　otherwise(params)方法对应路径匹配不到时的情况，这时候我们可以配置一个redirectTo参数，让它重定向到404页面或者是首页。

　　第三步：在主视图模板中指定加载子视图的位置
　　我们的单页面程序都是局部刷新的，那这个“局部”是哪里呢，这就轮到ngView出马了，只需在模板中简单的使用此指令，在哪里用，哪里就是“局部”。例如：

<div ng-view></div>
　　或：

<ng-view></ng-view>
　　我们的子视图将会在此处被引入进来。完成这三步后，你的程序的路由就配置好了。
