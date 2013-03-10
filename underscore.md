### underscore的一些学习记录 ###

##### 对underscore全局的理解 #####
看看underscore的API， underscore提供的函数大多数都有函数式编程的特性. 闭包， 高阶函数， 无副作用基本都能体现出来. 不过underscore的实现和函数式编程相差甚远.

underscore的源代码总体:

    (function(obj){}).call(this)
	
有两个重点： closure and call function

    (function(obj){
	    var _ = new Object();
		this._ = _;
		_.funcA = function(){
		    console.log(this);
			}
	)).call(this)
	
对underscore源码细节的研究

    var _ = function(obj) {
    if (obj instanceof _) return obj;
    if (!(this instanceof _)) return new _(obj);
    this._wrapped = obj;
    };

_ function的很多地方都有用到， 特别是mixin，以及chain的实现

    var each = _.each = _.forEach = function(obj, iterator, context) {
    if (obj == null) return;
    if (nativeForEach && obj.forEach === nativeForEach) {
      obj.forEach(iterator, context);
    } else if (obj.length === +obj.length) {
      for (var i = 0, l = obj.length; i < l; i++) {
        if (iterator.call(context, obj[i], i, obj) === breaker) return;
      }
    } else {
      for (var key in obj) {
        if (_.has(obj, key)) {
          if (iterator.call(context, obj[key], key, obj) === breaker) return;
        }
      }
    }
   };

each函数估计是被其他函数用到最多的函数

为什么用

    nativeForEach && obj.forEach === nativeForEach
	
而不是

    obj.forEach === nativeForEach
	
我认为唯一合理的解释就是当它们都为false时， 会执行 

    obj.forEach(iterator, context)
	
error

js中没有判断一个属性是否存在的方法， 所以用到了+, 通过转化为number判断

在each函数中， breaker的使用似乎是多余的， 但是在后面的函数some中会调用each， 此时breaker会有作用

