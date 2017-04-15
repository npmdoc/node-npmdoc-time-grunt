# api documentation for  [time-grunt (v1.4.0)](https://github.com/sindresorhus/time-grunt#readme)  [![npm package](https://img.shields.io/npm/v/npmdoc-time-grunt.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-time-grunt) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-time-grunt.svg)](https://travis-ci.org/npmdoc/node-npmdoc-time-grunt)
#### Display the elapsed execution time of grunt tasks

[![NPM](https://nodei.co/npm/time-grunt.png?downloads=true&downloadRank=true&stars=true)](https://www.npmjs.com/package/time-grunt)

[![apidoc](https://npmdoc.github.io/node-npmdoc-time-grunt/build/screenCapture.buildCi.browser.apidoc.html.png)](https://npmdoc.github.io/node-npmdoc-time-grunt/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-time-grunt/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-time-grunt/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "author": {
        "name": "Sindre Sorhus",
        "url": "sindresorhus.com"
    },
    "bugs": {
        "url": "https://github.com/sindresorhus/time-grunt/issues"
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
    "description": "Display the elapsed execution time of grunt tasks",
    "devDependencies": {
        "grunt": "^1.0.1",
        "grunt-cli": "^1.2.0",
        "xo": "*"
    },
    "directories": {},
    "dist": {
        "shasum": "062213e660c907e86f440556c01ea6597b712420",
        "tarball": "https://registry.npmjs.org/time-grunt/-/time-grunt-1.4.0.tgz"
    },
    "engines": {
        "node": ">=0.10.0"
    },
    "files": [
        "index.js"
    ],
    "gitHead": "29dc47d8e5fd3074b5867fad48f1d644afb480ae",
    "homepage": "https://github.com/sindresorhus/time-grunt#readme",
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
    "maintainers": [
        {
            "name": "sindresorhus"
        }
    ],
    "name": "time-grunt",
    "optionalDependencies": {},
    "repository": {
        "type": "git",
        "url": "git+https://github.com/sindresorhus/time-grunt.git"
    },
    "scripts": {
        "test": "xo && grunt && grunt sigint"
    },
    "version": "1.4.0",
    "xo": {
        "rules": {
            "xo/no-process-exit": 0
        }
    }
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module time-grunt](#apidoc.module.time-grunt)
1.  [function <span class="apidocSignatureSpan"></span>time-grunt (grunt, cb)](#apidoc.element.time-grunt.time-grunt)
1.  [function <span class="apidocSignatureSpan">time-grunt.</span>toString ()](#apidoc.element.time-grunt.toString)



# <a name="apidoc.module.time-grunt"></a>[module time-grunt](#apidoc.module.time-grunt)

#### <a name="apidoc.element.time-grunt.time-grunt"></a>[function <span class="apidocSignatureSpan"></span>time-grunt (grunt, cb)](#apidoc.element.time-grunt.time-grunt)
- description and source-code
```javascript
time-grunt = function (grunt, cb) {
	var now = new Date();
	var startTimePretty = dateTime(new Date(), {local: true});
	var startTime = now.getTime();
	var prevTime = startTime;
	var prevTaskName = 'loading tasks';
	var tableData = [];

	if (argv.indexOf('--help') !== -1 ||
		argv.indexOf('-h') !== -1 ||
		// for 'quiet-grunt'
		argv.indexOf('--quiet') !== -1 ||
		argv.indexOf('-q') !== -1 ||
		argv.indexOf('--version') !== -1 ||
		argv.indexOf('-V') !== -1) {
		return;
	}

	// crazy hack to work around stupid node-exit
	// Can this be removed now that node-exit#4 has been resolved?
	// https://github.com/cowboy/node-exit/issues/4
	var originalExit = process.exit;

	var interval;

	var exit = function (exitCode) {
		clearInterval(interval);
		process.emit('timegruntexit', exitCode);
		exit = function () {};
	};

	interval = setInterval(function () {
		process.exit = exit;
	}, 100);

	process.exit = exit;

	hooker.hook(grunt.log, 'header', function () {
		var name = grunt.task.current.nameArgs;
		var diff = Date.now() - prevTime;

		if (prevTaskName && prevTaskName !== name) {
			tableData.push([prevTaskName, diff]);
		}

		prevTime = Date.now();
		prevTaskName = name;
	});

	function formatTable(tableData) {
		var totalTime = Date.now() - startTime;
		var longestTaskName = tableData.reduce(function (acc, row) {
			var avg = row[1] / totalTime;

			if (avg < 0.01 && !grunt.option('verbose')) {
				return acc;
			}

			return Math.max(acc, row[0].length);
		}, 0);

		var maxColumns = process.stdout.columns || 80;
		var maxBarWidth;

		if (longestTaskName > maxColumns / 2) {
			maxBarWidth = (maxColumns - 20) / 2;
		} else {
			maxBarWidth = maxColumns - (longestTaskName + 20);
		}
		maxBarWidth = Math.max(0, maxBarWidth);

		function shorten(taskName) {
			var nameLength = taskName.length;

			if (nameLength <= maxBarWidth) {
				return taskName;
			}

			var partLength = Math.floor((maxBarWidth - 3) / 2);
			var start = taskName.substr(0, partLength + 1);
			var end = taskName.substr(nameLength - partLength);

			return start.trim() + '...' + end.trim();
		}

		function createBar(percentage) {
			var rounded = Math.round(percentage * 100);

			if (rounded === 0) {
				return '0%';
			}

			var barLength = Math.ceil(maxBarWidth * percentage) + 1;
			var bar = new Array(barLength).join(barChar);

			return bar + ' ' + rounded + '%';
		}

		var tableDataProcessed = tableData.map(function (row) {
			var avg = row[1] / totalTime;

			// hide the watch task
			if (!grunt.option('verbose') && /^watch($|:)/.test(row[0])) {
				return null;
			}

			if (numberIsNan(avg) || (avg < 0.01 && !grunt.option('verbose'))) {
				return null;
			}

			return [shorten(row[0]), chalk.blue(prettyMs(row[1])), chalk.blue(createBar(avg))];
		}).reduce(function (acc, row) {
			if (row) {
				acc.push(row);
				return acc;
			}

			return acc;
		}, []);

		tableDataProcessed.push([chalk.magenta('Total', prettyMs(totalTime))]);

		return table(tableDataProcessed, {
			align: ['l', 'r', 'l'],
			stringLength: function (str) {
				return chalk.stripColor(str).length;
			}
		});
	}

	process.on('SIGINT', function () {
		process.exit();
	});

	process.once('timegruntexit', function (exitCode) {
		clearInterval(interval);

		process.exit = originalExit;
		hooker.unhook(grunt.log, 'header');

		var diff = Date.now() - prevTime;

		if (prevTaskName) {
			tableData.push([prevTaskName, diff]);
		}

		// 'grunt.log.header' should be unhooked above, but in some cases it's not
		log('\n\n' + chalk.underline('Execution Time') + chalk.gray(' (' + startTimePretty + ')'));
		log(formatTable(tableData) + '\n');

		if (cb) {
			cb(tableData, function () {
				process.exit(exitCode);
			});

			return;
		}

		process.exit(exitCode);
	});
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.time-grunt.toString"></a>[function <span class="apidocSignatureSpan">time-grunt.</span>toString ()](#apidoc.element.time-grunt.toString)
- description and source-code
```javascript
toString = function () {
    return toString;
}
```
- example usage
```shell
n/a
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
