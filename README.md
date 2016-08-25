# Node Chain Middleware

*Middleware are like links in chain.*

[![Build Status][travis-image]][Travis CI]
[![Dependencies][dependencies-image]][Dependencies]
[![Dev Dependencies][devdependencies-image]][Dev Dependencies]
[![codecov.io][codecov-image]][Code Coverage]

About
-----

This makes it easier to use middleware together by "chaining" them to be single function which can then be called.

How to Use
----------

To use this, you will need to `require` this module in your module, or inject using a form of dependency injection. [Dizzy] is a good JavaScript dependency injector. Once you can call it, you will call the module with as many arguments as you pass in. You can call it with multiple arguments, arrays, arrays of arrays and so on. It will straighten out the argument and chain them into a single function.

    // Example without using Node Chain Middleware
    "use strict"

    var helmet;

    helmet = require("helmet");

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

    / Example using Node Chain Middleware with dependency injection
    "use strict";

    module.exports = (chainMiddleware, helmet) => {
        function setUpMiddleware () {
            var middleware;

            return chainMiddleware([
                helmet.ieNoOpen(),
                [
                    helmet.noCache(),
                    helmet.noSniff(),
                    helmet.xssFilter()
                ]
            ]);
        }

        return {
            setUpMiddleware
        };
    };

[Code Coverage]: https://codecov.io/github/tests-always-included/node-chain-middleware?branch=develop
[codecov-image]: https://codecov.io/github/tests-always-included/node-chain-middleware/coverage.svg?branch=develop
[Dev Dependencies]: https://david-dm.org/tests-always-included/node-chain-middleware/develop#info=devDependencies
[devdependencies-image]: https://david-dm.org/tests-always-included/node-chain-middleware/develop/dev-status.png
[Dependencies]: https://david-dm.org/tests-always-included/node-chain-middleware/develop
[dependencies-image]: https://david-dm.org/tests-always-included/node-chain-middleware/develop.png
[Dizzy]: https://github.com/tests-always-included/dizzy
[travis-image]: https://secure.travis-ci.org/tests-always-included/node-chain-middleware.png
[Travis CI]: http://travis-ci.org/tests-always-included/node-chain-middleware
