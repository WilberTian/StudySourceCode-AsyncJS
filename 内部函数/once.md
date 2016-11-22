该函数接收一个函数，并返回一个新的函数；主要作用就是保证被传入的函数只运行一次。

该函数的实现也比较简单，通过闭包来维护参数｀fn｀，在第一次运行的时候保存｀fn｀到一个新的变量，然后暴力的将｀fn｀赋值为｀null｀；  
后面再运行的时候，就判断｀fn｀为｀null｀，然后直接返回。

	function once(fn) {
	    return function () {
	        if (fn === null) return;
	        var callFn = fn;
	        fn = null;
	        callFn.apply(this, arguments);
	    };
	}


使用示例：

	function printHello() {
		console.log('hello there');
	}	

	var newFn = once(printHello);

	newFn();
	// hello there 
	newFn();
	// undefined