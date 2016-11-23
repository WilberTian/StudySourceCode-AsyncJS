    

    import isArray from 'lodash/isArray';
    import noop from 'lodash/noop';
    import once from './internal/once';
    import rest from './internal/rest';
    import onlyOnce from './internal/onlyOnce';

    function(tasks, callback) {
        callback = once(callback || noop);
        if (!isArray(tasks)) return callback(new Error('First argument to waterfall must be an array of functions'));
        if (!tasks.length) return callback();
        var taskIndex = 0;
        function nextTask(args) {
            if (taskIndex === tasks.length) {
                return callback.apply(null, [null].concat(args));
            }
            var taskCallback = onlyOnce(rest(function(err, args) {
                if (err) {
                    return callback.apply(null, [err].concat(args));
                }
                nextTask(args);
            }));
            args.push(taskCallback);
            var task = tasks[taskIndex++];
            task.apply(null, args);
        }
        nextTask([]);
    }


使用示例：

    async.waterfall([
        function(callback) {
            callback(null, 'one', 'two');
        },
        function(arg1, arg2, callback) {
            // arg1 now equals 'one' and arg2 now equals 'two'
            callback(null, 'three');
        },
        function(arg1, callback) {
            // arg1 now equals 'three'
            callback(null, 'done');
        }
    ], function (err, result) {
        // result now equals 'done'
    });