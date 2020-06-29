---
tags:
    - frontend
id: ""
created: 2020-03-21T12:20:36.320Z
modified: 2020-04-30T14:48:24.615Z
---
# What is AMD, CommonJS, and UMD?

[Source](https://www.davidbcalhoun.com/2014/what-is-amd-commonjs-and-umd/ "Permalink to What is AMD, CommonJS, and UMD?")

## Intro

Over the years there's been a steadily increasing ecosystem of JavaScript components to choose from. The sheer amount of choices is fantastic, but this also infamously presents a difficulty when components are mixed-and-matched. And it doesn't take too long for budding developers to find out that not all components are built to play nicely together.

To address these issues, the competing module specs AMD and CommonJS have appeared on the scene, allowing developers to write their code in an agreed-upon sandboxed and modularized way, so as not to “pollute the ecosystem”.

## AMD

Asynchronous Module Definition (AMD) has gained traction on the frontend, with RequireJS being the most popular implementation.

Here's module `foo` with a single dependency on `jquery`:

    1
    2
    3
    4
    5
    6
    7
    8

    //    filename: foo.js
    define(['jquery'], function ($) {
        //    methods
        function myFunc(){};

        //    exposed public methods
        return myFunc;
    });

And a little more complicated example with multiple dependencies and multiple exposed methods:

    //    filename: foo.js
    define(['jquery', 'underscore'], function ($, _) {
        //    methods
        function a(){};    //    private because it's not returned (see below)
        function b(){};    //    public because it's returned
        function c(){};    //    public because it's returned

        //    exposed public methods
        return {
            b: b,
            c: c
        }
    });

The first part of the define is an array of dependencies, while the second part is essentially the callback function which is only executed when the dependencies are available (script loaders like RequireJS take care of that part, including figuring out where the files are located).

Note that the dependency to variable order is important (ex. jquery-\>`$`, underscore-\>`_`).

Also note that we can map the dependencies to any arbitrary variables we want here. If we change `$` to `$$` in the code above, all jQuery references within our function block will be `$$` instead of `$`.

And note, most importantly, that you can't reference the variables `$` and `_` outside of the function, because it's sandboxed from other code. That's the goal here!

## CommonJS

CommonJS is a style you may be familiar with if you're written anything in Node (which uses a slight variant). It's also been gaining traction on the frontend with Browserify.

Using the same format as before, here's what our `foo` module looks like in CommonJS:

    //    filename: foo.js

    //    dependencies
    var $ = require('jquery');

    //    methods
    function myFunc(){};

    //    exposed public method (single)
    module.exports = myFunc;

And our more complicated example, with multiple dependencies and multiple exposed methods:

    //    filename: foo.js
    var $ = require('jquery');
    var _ = require('underscore');

    //    methods
    function a(){};    //    private because it's omitted from module.exports (see below)
    function b(){};    //    public because it's defined in module.exports
    function c(){};    //    public because it's defined in module.exports

    //    exposed public methods
    module.exports = {
        b: b,
        c: c
    };

## UMD: Universal Module Definition

Since CommonJS and AMD styles have both been equally popular, it seems there's yet no consensus. This has brought about the push for a “universal” pattern that supports both styles, which brings us to none other than the Universal Module Definition.

The pattern is admittedly ugly, but is both AMD and CommonJS compatible, as well as supporting the old-style “global” variable definition:

    (function (root, factory) {
        if (typeof define === 'function' && define.amd) {
            // AMD
            define(['jquery'], factory);
        } else if (typeof exports === 'object') {
            // Node, CommonJS-like
            module.exports = factory(require('jquery'));
        } else {
            // Browser globals (root is window)
            root.returnExports = factory(root.jQuery);
        }
    }(this, function ($) {
        //    methods
        function myFunc(){};

        //    exposed public method
        return myFunc;
    }));

And keeping in the same pattern as the above examples, the more complicated case with multiple dependencies and multiple exposed methods:

    (function (root, factory) {
        if (typeof define === 'function' && define.amd) {
            // AMD
            define(['jquery', 'underscore'], factory);
        } else if (typeof exports === 'object') {
            // Node, CommonJS-like
            module.exports = factory(require('jquery'), require('underscore'));
        } else {
            // Browser globals (root is window)
            root.returnExports = factory(root.jQuery, root._);
        }
    }(this, function ($, _) {
        //    methods
        function a(){};    //    private because it's not returned (see below)
        function b(){};    //    public because it's returned
        function c(){};    //    public because it's returned

        //    exposed public methods
        return {
            b: b,
            c: c
        }
    }));

(Sep 2014 edit: fixed syntax for CommonJS in the last example)
