该函数跟｀once｀函数类似，但是当后面再次调用的时候就抛出异常。

	function onlyOnce(fn) {
	    return function() {
	        if (fn === null) throw new Error("Callback was already called.");
	        var callFn = fn;
	        fn = null;
	        callFn.apply(this, arguments);
	    };
	}


使用示例：

	function printHello() {
		console.log('hello there');
	}	

	var newFn = onlyOnce(printHello);

	newFn();
	// hello there 
	newFn();
	// Uncaught Error: Callback was already called