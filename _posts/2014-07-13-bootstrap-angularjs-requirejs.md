---
layout: post
title:  Bootstrap AngularJs with RequireJs
date:   2014-07-13 12:08:26
categories: "develop"
disqus: true
keywords:
- AngularJs
- RequireJs
- Javascript
excerpt: How to bootstrap an angularjs app with requirejs.
tags:
- development
- javascript
---
------
## Background
Recently, I implemented all the javascript side of code with
Typescript, for a AngularJS [project](http://www.testlifeinuk.com). One tricky part of it is when I
compile all the Typescript files into AMD modules, I need to use
RequireJS to load them.

## The Solution
Here is an simple example on how this will work. Supports we have defined
a directive in 'app/directive.js' and it refer to the template in 'app/directive.html'.
I want to test the directive in a simple page.

{% highlight html linenos=table %}
<html>
<body ng-controller="qsCtrl">
<div class="container">
    <directive obj="obj">
    </directive>
</div>
</body>

<script src="bower_components/requirejs/require.js"></script>
<script src="bower_components/angular/angular.js"></script>
<script >
    require(['app/directive'], function(qs){

        var app = angular.module('testDirective', []);
        app.directive('questionDirective', qs.directive);
        app.controller('qsCtrl', function($scope){
                    $scope.obj='';
                });

        angular.element(document).ready(function() {
            angular.bootstrap(document, ['testDirective']);
        });
    });
</script>
</html>

{% endhighlight %}


## The Caveats
A few notes on the problems you might encounter (which I did encounter).

### Using the require(string) function

The RequireJS has a global function that can be called with a string parameter,
like in the following code:

{% highlight javascript linenos=table %}
var qs= require('app/directive');
var app = angular.module('testDirective', []);
app.directive('questionDirective', qs.directive);
{% endhighlight %}

It is very much like what we see in the Node.js (which use CommonJS module instead
  of AMD). As simple as I first thought, this will not work, as explained in the
[AMD specification](https://github.com/amdjs/amdjs-api/wiki/require).
Basically this is a synchronous call (which is what I expected) but will only
work when the required module is already loaded; otherwise it will throw exception.
More thought on this topic can be found in the
[stackoverflow discussion](http://stackoverflow.com/questions/13225245/require-js-synchronous)

### Direct loading a AMD module
Another attempt is not to use RequireJS at all to load the directive javascript file.

{% highlight javascript linenos=table %}
<script src="bower_components/requirejs/require.js"></script>
<script src="bower_components/angular/angular.js"></script>
<script src="app/directive.js"></script>
{% endhighlight %}

Because 'app/directive.js' is defined as AMD module, this just does not load with a
proper reference to it.
