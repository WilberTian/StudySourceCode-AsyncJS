

import eachOf from './eachOf';
import parallel from './internal/parallel';

export default function parallelLimit(tasks, callback) {
    parallel(eachOf, tasks, callback);
}