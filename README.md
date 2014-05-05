# Welcome to stacktrace.js! [![Code Climate](https://codeclimate.com/github/eriwen/javascript-stacktrace.png)](https://codeclimate.com/github/eriwen/javascript-stacktrace)
A JavaScript tool that allows you to debug your JavaScript by giving you a [stack trace](http://en.wikipedia.org/wiki/Stack_trace) of function calls leading to an error (or any condition you specify)

# How do I use stacktrace.js? #

##### node.js or [webpack](https://github.com/webpack/webpack)

```javascript
var printStackTrace = require('stacktrace-js')
```

##### browser global (the global variable will be `printStackTrace`)

```html
<script src="https://rawgithub.com/stacktracejs/stacktrace.js/master/stacktrace.js"></script>
```

#### Example

```javascript
var trace = printStackTrace();
alert(trace.join('\n\n')); // Output however you want!

try {
    throw new Error('yourError')
} catch(e) {
    var trace = printStackTrace({e: e}); // pass in an Error object to get a stacktrace *not available in IE or Safari 5-* 
    alert('Error!\n' + 'Message: ' + e.message + '\nStack trace:\n' + trace.join('\n'));
    // do something else with error
}

// Function Instrumentation

function logStackTrace(stack) {
    console.log(stack.join('\n'));
}
var p = new printStackTrace.implementation();
p.instrumentFunction(this, 'baz', logStackTrace);

function foo() {
    var a = 1;
    bar();
}
function bar() {
    baz();
}
foo(); //Will log a stacktrace when 'baz()' is called containing 'foo()'!

p.deinstrumentFunction(this, 'baz'); //Remove function instrumentation
```

Bookmarklet available on the [project home page](http://stacktracejs.com).

# API #

`printStackTrace(options)` - Returns an array of stack-trace line strings (in browser-specific format). Note that error message is not included in stack trace. `options` is an object that can contain the following properties:
    
* `e` - (*optional*) The error to create a stacktrace from. If this is not passed in, a stacktrace will be created from a newly created exception inside `printStackTrace`.
* `guess` - If we should try to resolve the names of anonymous functions (*default: true*)

`new printStackTrace.implementation()` - Returns an object that can be used to instrument functions.

`p.instrumentFunction(context, functionName, callback)` - Calls `callback` when the function `context[functionName]` is called. When called, `callback` receives the stack array (in same form as `printStackTrace` returns). `p` is the object returned by `new printStackTrace.implementation()`.

`p.deinstrumentFunction(context, functionName)` - Removes instrumentation from the function `context[functionName]`.

# What browsers does stacktrace.js support? #
It is currently tested and working on:

 - Firefox (and Iceweasel) 0.9+
 - Google Chrome 1+
 - Safari 3.0+ (including iOS 1+)
 - Opera 7+
 - IE 5.5+
 - Konqueror 3.5+
 - Flock 1.0+
 - SeaMonkey 1.0+
 - K-Meleon 1.5.3+
 - Epiphany 2.28.0+
 - Iceape 1.1+

## Contributions [![Stories in Ready](http://badge.waffle.io/eriwen/javascript-stacktrace.png)](http://waffle.io/eriwen/javascript-stacktrace)  

This project is made possible due to the efforts of these fine people:

* [Eric Wendelin](http://eriwen.com)
* [Luke Smith](http://lucassmith.name/)
* Loic Dachary
* Johan Euphrosine
* Øyvind Sean Kinsey
* Victor Homyakov
