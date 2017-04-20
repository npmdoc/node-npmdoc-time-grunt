# npmdoc-time-grunt

#### api documentation for  time-grunt (v1.4.0)  [![npm package](https://img.shields.io/npm/v/npmdoc-time-grunt.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-time-grunt) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-time-grunt.svg)](https://travis-ci.org/npmdoc/node-npmdoc-time-grunt)

#### Display the elapsed execution time of grunt tasks

[![NPM](https://nodei.co/npm/time-grunt.png?downloads=true&downloadRank=true&stars=true)](https://www.npmjs.com/package/time-grunt)

- [https://npmdoc.github.io/node-npmdoc-time-grunt/build/apidoc.html](https://npmdoc.github.io/node-npmdoc-time-grunt/build/apidoc.html)

[![apidoc](https://npmdoc.github.io/node-npmdoc-time-grunt/build/screenCapture.buildCi.browser.%252Ftmp%252Fbuild%252Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-time-grunt/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-time-grunt/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-time-grunt/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "name": "time-grunt",
    "version": "1.4.0",
    "description": "Display the elapsed execution time of grunt tasks",
    "keywords": [
        "grunt",
        "tasks",
        "measure",
        "time",
        "profile",
        "stat",
        "stats",
        "perf",
        "performance",
        "tasks"
    ],
    "license": "MIT",
    "author": {
        "name": "Sindre Sorhus",
        "url": "sindresorhus.com"
    },
    "files": [
        "index.js"
    ],
    "repository": "sindresorhus/time-grunt",
    "scripts": {
        "test": "xo && grunt && grunt sigint"
    },
    "engines": {
        "node": ">=0.10.0"
    },
    "xo": {
        "rules": {
            "xo/no-process-exit": 0
        }
    },
    "dependencies": {
        "chalk": "^1.0.0",
        "date-time": "^1.1.0",
        "figures": "^1.0.0",
        "hooker": "^0.2.3",
        "number-is-nan": "^1.0.0",
        "pretty-ms": "^2.1.0",
        "text-table": "^0.2.0"
    },
    "devDependencies": {
        "grunt": "^1.0.1",
        "grunt-cli": "^1.2.0",
        "xo": "*"
    }
}
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
