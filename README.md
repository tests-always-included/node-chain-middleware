# Node Chain Middleware

*Middleware are like links in chain.*

[![Build Status][travis-image]][Travis CI]
[![Dependencies][dependencies-image]][Dependencies]
[![Dev Dependencies][devdependencies-image]][Dev Dependencies]
[![codecov.io][codecov-image]][Code Coverage]

About
-----

This makes it easier to use middleware together by "chaining" them to be single function which can be called.

How to Use
----------

To use this, you will need to require this module in your module, or inject using a form of dependency injection. [Dizzy] is a good JavaScript dependency injector. Once you can call it you will call the module with as many arguments as you pass in. You can call it with multiple arguments, arrays, arrays of arrays. It straightens out the lot and chains it into a single function.

    // Example using dependency injection
    "use strict";

    module.exports = (chainMiddleware, restifyPlugins, schema) => {
        return (schemaPath) => {
            return chainMiddleware(restifyPlugins.bodyParser({
                mapFiles: false,
                mapParams: false,
                maxBodySize: 4096,
                overrideParams: false,
                requestBodyOnGet: true,
                rejectUnknown: true
            }), (req, res, next) => {
                if (!req.body || schema.validate(req.body, schemaPath)) {
                    res.send(400);

                    return next(false);
                }

                return next();
            });
        };
    };

[Code Coverage]: https://codecov.io/github/tests-always-included/node-chain-middleware?branch=develop
[codecov-image]: https://codecov.io/github/tests-always-included/node-chain-middleware/coverage.svg?branch=develop
[Dev Dependencies]: https://david-dm.org/tests-always-included/jnode-chain-middleware/develop#info=devDependencies
[devdependencies-image]: https://david-dm.org/tests-always-included/node-chain-middleware/develop/dev-status.png
[Dependencies]: https://david-dm.org/tests-always-included/jnode-chain-middleware/develop
[dependencies-image]: https://david-dm.org/tests-always-included/node-chain-middleware/develop.png
[Dizzy]: https://github.com/tests-always-included/dizzy
[Node.js]: https://nodejs.org
[travis-image]: https://secure.travis-ci.org/tests-always-included/node-chain-middleware.png
[Travis CI]: http://travis-ci.org/tests-always-included/node-chain-middleware
