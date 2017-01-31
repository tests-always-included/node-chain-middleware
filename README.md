# Node Chain Middleware

*Middleware are like links in chain.*

[![npm version][npm-badge]][npm-link]
[![Build Status][travis-badge]][travis-link]
[![Dependencies][dependencies-badge]][dependencies-link]
[![Dev Dependencies][devdependencies-badge]][devdependencies-link]
[![codecov.io][codecov-badge]][codecov-link]


About
-----

This makes it easier to use middleware together by "chaining" them to be single function which can then be called.

This module combines multiple middlewares together and "chains" them into a single function. Unlike normal middleware additions, you are able to merge arrays of middleware and even nested arrays of middleware. They all act as expected.

This module is currently in use for [Restiq] and [Restify] and should work for [Express] and similar servers using the same pattern for middleware.


How to Use
----------

To use this, you will need to `require` this module in your module, or inject using a form of dependency injection. [Dizzy] is a good JavaScript dependency injector. Once you can call it, you will call the module with as many arguments as you pass in. You can call it with multiple arguments, arrays, arrays of arrays and so on. It will straighten out the argument and chain them into a single function.

    // Example without using Node Chain Middleware
    "use strict"

    var helmet;

    helmet = require("helmet");

    // Server must be passed in, otherwise the middleware can't
    // be added to the server.
    function setUpMiddleware (server) {
        if (!server || !server.use) {
            throw new Error("Did not pass a server!");
        }

        server.use(helmet.ieNoOpen());
        server.use(helmet.noCache());
        server.use(helmet.noSniff());
        server.use(helmet.xssFilter());

        return server;
    }

    module.exports = setUpMiddleware;

    // Example using Node Chain Middleware
    "use strict";

    var chainMiddleware, helmet;

    chainMiddleware = require("chain-middleware");
    helmet = require("helmet");

    // Do not need the server passed in here as we can just
    // return the chain middleware function.
    function setUpMiddleware () {
        return chainMiddleware([
            helmet.ieNoOpen(),
            [
                helmet.noCache(),
                helmet.noSniff(),
                helmet.xssFilter()
            ]
        ]);
    }

    module.exports = setUpMiddleware;


[Dizzy]: https://github.com/tests-always-included/dizzy
[Express]: http://expressjs.com/
[Restify]: http://restify.com/
[Restiq]: https://github.com/andrasq/node-restiq
[codecov-badge]: https://img.shields.io/codecov/c/github/tests-always-included/node-chain-middleware/master.svg
[codecov-link]: https://codecov.io/github/tests-always-included/node-chain-middleware?branch=master
[dependencies-badge]: https://img.shields.io/david/tests-always-included/node-chain-middleware.svg
[dependencies-link]: https://david-dm.org/tests-always-included/node-chain-middleware
[devdependencies-badge]: https://img.shields.io/david/dev/tests-always-included/node-chain-middleware.svg
[devdependencies-link]: https://david-dm.org/tests-always-included/node-chain-middleware#info=devDependencies
[npm-badge]: https://img.shields.io/npm/v/node-chain-middleware.svg
[npm-link]: https://npmjs.org/package/node-chain-middleware
[travis-badge]: https://img.shields.io/travis/tests-always-included/node-chain-middleware/master.svg
[travis-link]: http://travis-ci.org/tests-always-included/node-chain-middleware
