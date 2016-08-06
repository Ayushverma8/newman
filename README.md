<a href="https://www.getpostman.com/"><img src="https://raw.githubusercontent.com/postmanlabs/postmanlabs.github.io/develop/global-artefacts/postman-logo%2Btext-320x132.png" /></a><br />
_Supercharge your API workflow<br/>Modern software is built on APIs. Postman helps you develop APIs faster._

# newman <sub>_the cli companion for postman_</sub>

Using Newman one can effortlessly run and test a Postman Collections directly from the command-line. It is built with
extensibility in mind so that you can easily integrate it with your continuous integration servers and build systems.

> To view documentation of current stable 2.x release of Newman, refer to the latest
> [Newman v2.x release](https://github.com/postmanlabs/newman/tree/release/2.x)

## Getting started

To run Newman, ensure that you have NodeJS >= v4. A copy of the NodeJS installable can be downloaded from [https://nodejs.org/en/download/package-manager/](https://nodejs.org/en/download/package-manager/).

The easiest way to install Newman is using NPM. If you have NodeJS installed, it is most likely that you have NPM
installed as well.

```terminal
$ npm install newman --global;
```

The `newman run` command allows you to specify a collection to be run. You can easily export your Postman
Collection as a json file from the [Postman App](https://www.getpostman.com/apps) and run it using Newman.

```terminal
$ newman run examples/sample-collection.json;
```

If your collection file is available as an URL (such as from our [Cloud API service](https://api.getpostman.com/)),
Newman can fetch your file and run it as well.

```terminal
$ newman run https://www.getpostman.com/collections/631643-f695cab7-6878-eb55-7943-ad88e1ccfd65-JsLv;
```

For the complete list of options, refer the [Commandline Options](#commandline-options) section below.

[![terminal-demo](https://asciinema.org/a/9sb9wrmy5v47j7msb7a7f3osv.png)](https://asciinema.org/a/9sb9wrmy5v47j7msb7a7f3osv?autoplay=1)

### Using Newman as a NodeJS module

Newman can be easily used within your JavaScript projects as a NodeJS module. The entire set of Newman CLI functionality is available for programmatic use as well.

The following example runs a collection by reading a JSON collection file stored on disk.

```javascript
var newman = require('newman'); // require newman in your project

// call newman.run to pass `options` object and wait for callback
newman.run({
    collection: require('./sample-collection.json'),
    reporters: 'cli'
}, function (err) {
	if (err) { throw err; }
    console.log('collection run complete!');
});
```

**Note:** The newman v2.x `.execute` function has been deprecated and will be discontinued in future.

## Commandline Options

### `newman run <collection-file-source> [options]`

- `-e <source>`, `--environment <source>`<br />
  Specify an environment file path or URL. Environments provide a set of variables that one can use within collections.
  [Read More](https://www.getpostman.com/docs/environments)

- `-g <source>`, `--globals <source>`<br />
  Specify file path or URL for global variables. Global variables are similar to environment variables but has a lower
  precedence and can be overridden by environment variables having same name.

- `-d <source>`, `--iteration-data <source>`<br />
  Specify a data source file (CSV) to be used for iteration as a path to a file or as a URL.
  [Read More](https://www.getpostman.com/docs/multiple_instances)

- `-n <number`, `--iteration-count <number>`<br />
  Specifies the number of times the collection has to be run when used in conjunction with iteration data file.

- `--folder <name>`<br />
  Run requests within a particular folder in a collection.

- `--export-environment <path>`<br />
  The path to the file where Newman will output the final environment variables file before completing a run.

- `--export-globals <path>`<br />
  The path to the file where Newman will output the final global variables file before completing a run.

- `--export-collection <path>`<br />
  The path to the file where Newman will output the final collection file before completing a run.

- `--timeout-request <ms>`<br />
  Specify the time (in milliseconds) to wait for requests to return a response.

- `-k --insecure`<br />
  Disables SSL verification checks and allows self-signed SSL certificates.

- `--ignore-redirects`<br />
  Prevents newman from automatically following 3XX redirect responses.

- `--delay-request`<br />
  Specify the extent of delay between requests (milliseconds).

- `--stop-on-error`<br />
  Specify whether or not to stop a collection run on encountering the first error.

- `-x --suppress-exit-code`<br />
  Specify whether or not to override the default exit code for the current run.

- `--no-color`<br />
  Newman attempts to automatically turn off color output to terminals when it detects the lack of color support. With
  this property, one can forcibly turn off the usage of color in terminal output for reporters and other parts of Newman
  that output to console.

#### Configuring Reporters

- `--reporters <name>`<br />
  Specify one reporter name as `string` or provide more than one reporter name as a comma separated list of reporter names. Available reporters are: `cli`, `json`, `html` and `junit`.

- `--reporter-{{reporter-name}}-{{reporter-options}}`<br />
  When multiple reporters are provided, if one needs to specifically override or provide an option to one reporter, this
  is achieved by prefixing the option with `--reporter-{{reporter-name}}-`.<br /><br />
  For example, `... --reporters cli,html --reporter-cli-silent` would silence the CLI reporter only.

- `--reporter-{{reporter-options}}`<br />
  If more than one reporter accepts the same option name, they can be provided using the commin reporter option syntax.
  <br /<br />
  For example, `... --reporters cli,html --reporter-silent` passes the `silent: true` option to both HTML and CLI
  reporter.

##### CLI reporter options
These options are supported by the CLI reporter, use them with appropriate argument switch prefix. For example, the
option `no-summary` can be passed as `--reporter-no-summary` or `--reporter-cli-no-summary`.

| Option      | Description     |
|-------------|-----------------|
| --reporter-cli-silent         | The CLI reporter is internally disabled and you see no output to terminal. |
| --reporter-cli-no-summary     | The statistical summary table is not shown. |
| --reporter-cli-no-failures    | This prevents the run failures from being separately printed. |
| --reporter-cli-no-assertions  | This turns off the request-wise output as they happen. |
| --reporter-cli-no-console     | This turns off the output of `console.log` (and other console calls) from collection's scripts. |

##### JSON reporter options
The built-in JSON reporter is useful in producing a comprehensive output of the run summary. The only option it takes is
the path to the file where to write the file. The content of this file is exactly same as the `summary` parameter sent
to the callback when `newman.run()` is executed programmatically.

- `--reporter-json-export <path>`<br />

##### HTML reporter options
The HTML reporter produces a barebone HTML of the Newman run.

- `--reporter-html-export <path>`<br />

<!--
| `--timeout-run <ms>` | TODO |
| `--timeout-script <ms>` | TODO |
-->

Older command line options are supported, but are deprecated in favour of the newer v3 options and will soon be discontinued. For documentation on the older command options, refer to [README.md for Newman v2.X](https://github.com/postmanlabs/newman/blob/release/2.x/README.md).

### `newman [options]`

- `-h`, `--help`

  Show commandline help, including a list of options, and sample use cases.

- `-v`, `--version`

  Displays the current Newman version, taken from [package.json](https://github.com/postmanlabs/newman/blob/master/package.json)


## API Reference

### newman.run(options: _object_ , callback: _function_) => run: EventEmitter
The `run` function executes a collection and returns the run result to a callback function provided as parameter. The
return of the `newman.run` function is a run instance, which emits run events that can be listened to.

| Parameter | Description   |
|-----------|---------------|
| options                   | This is a required argument and it contains all information pertaining to running a collection.<br /><br />_Required_<br />Type: `object` |
| options.collection        | The collection is a required property of the `options` argument. It accepts an object representation of a Postman Collection which should resemble the schema mentioned at [https://schema.getpostman.com/](https://schema.getpostman.com/). The value of this property could also be an istance of Collection Object from the [Postman Collection SDK](https://github.com/postmanlabs/postman-collection).<br /><br />As `string`, one can provide a URL where the Collection JSON can be found (e.g. [Postman Cloud API](https://api.getpostman.com/) service) or path to a local JSON file.<br /><br />_Required_<br />Type: `object|string|`[PostmanCollection](https://github.com/postmanlabs/postman-collection/wiki#Collection) |
| options.environment        | One can optionally pass an environment file path or URL as `string` to this property and that will be used to read Postman Environment Variables from. This property also accepts environment variables as an `object`. Environment files exported from Postman App can be directly used here.<br /><br />_Optional_<br />Type: `object|string` |
| options.globals           | Postman Global Variables can be optionally passed on to a collection run in form of path to a file or URL. It also accepts variables as an `object`.<br /><br />_Optional_<br />Type: `object|string` |
| options.iterationCount    | Specify the number of iterations to run on the collection. This is usually accompanied by providing a data file reference as `options.iterationData`.<br /><br />_Optional_<br />Type: `number` |
| options.iterationData     | Path to the JSON or CSV file or URL to be used as data source when running multiple iterations on a collection.<br /><br />_Optional_<br />Type: `string` |
| options.folder            | The name or ID of the folder (ItemGroup) in the collection which would be run instead of the entire collection.<br /><br />_Optional_<br />Type: `string` |
| options.timeoutRequest    | Specify the time (in milliseconds) to wait for requests to return a response.<br /><br />_Optional_<br />Type: `number` |
| options.ignoreRedirects   | This specifies whether newman would automatically follow 3xx responses from servers.<br /><br />_Optional_<br />Type: `boolean` |
| options.insecure          | Disables SSL verification checks and allows self-signed SSL certificates.<br /><br />_Optional_<br />Type: `boolean` |
| options.reporters         | Specify one reporter name as `string` or provide more than one reporter name as an `array`.<br /><br />Available reporters: `cli`, `html` and `junit`.<br /><br />_Optional_<br />Type: `string|array` |
| options.noColor           | Newman attempts to automatically turn off color output to terminals when it detects the lack of color support. With this property, one can forcibly turn off the usage of color in terminal output for reporters and other parts of Newman that output to console.<br /><br />_Optional_<br />Type: `boolean` |
| callback                  | Upon completion of the run, this callback is executed with the `error`, `summary` argument.<br /><br />_Required_<br />Type: `function` |

### newman.run~callback(error: _object_ , summary: _object_)

The `callback` parameter of the `newman.run` function receives two arguments: (1) `error` and (2) `summary`

| Argument  | Description   |
|-----------|---------------|
| error                     | In case newman faces an error during the run, the error is passed on to this argument of callback. By default, only fatal errors, such as the ones caused by any fault inside Newman is passed on to this argument. However, setting `abortOnError:true` or `abortOnFailure:true` as part of run options will cause newman to treat collection script syntax errors and test failures as fatal errors and be passed down here while stopping the run abruptly at that point.<br /><br />Type: `object` |
| summary                   | The run summary will contain information pertaining to the run.<br /><br />Type: `object` |
| summary.error             | <br /><br />Type: `object` |
| summary.collection        | <br /><br />Type: `object` |
| summary.environment       | <br /><br />Type: `object` |
| summary.globals           | <br /><br />Type: `object` |
| summary.run               | <br /><br />Type: `object` |
| summary.run.stats         | <br /><br />Type: `object` |
| summary.run.failures      | <br /><br />Type: `array.<object>` |
| summary.run.executions    | <br /><br />Type: `array.<object>` |

### newman.run~events

Newman triggers a whole bunch of events during the run.

```javascript
newman.run({
    collection: require('./sample-collection.json')
}).on('start', function (err, args) { // on start of run, log to console
    console.log('running a collection...');
}).on('done', function (err, summary) {
    if (error || summary.error) {
        console.error('collection run encountered an error.');
    }
    else {
        console.log('collection run completed.');
    }
});
```

All events receive two arguments (1) `error` and (2) `args`. **The list below describes the properties of the second
argument object.**

| Event     | Description   |
|-----------|---------------|
| start                     | This event is emitted at the start of a collection run |
| beforeIteration           | This event is emitted just before an iteration commences |
| beforeItem                |  |
| beforePrerequest          | This event is emitted right before a `prerequest` script is executed |
| prerequest                | This event is emitted right after a `prerequest` script has been run |
| beforeRequest             | This event is emitted right before a request is intiated |
| request                   | This event is emitted immediately after a request has completed |
| beforeTest                | This event is emitted before a test run |
| test                      | An event emitted after a test run has completed |
| beforeScript              |  |
| script                    |  |
| item                      |  |
| iteration                 | This event is emitted when the current collection run iteration ends  |
| assertion                 | This event is emitted whenever any assertion is made within test scripts for a collection request |
| console                   | This event handles console message bubbling to their appropriate layers |
| exception                 | An event emitted when a run time exception occurs |
| beforeDone                | A special event emitted to invoke all reporters and export directives |
| done                      | This event is emitted when a collection run has completed, with or without errors |

<!-- TODO: write about callback summary -->

## Community Support

<img src="https://www.getpostman.com/img/v2/icons/slack.svg" align="right" />
If you are interested in talking to the team and other Newman users, we are there on <a href="https://www.getpostman.com/slack-invite" target="_blank">Slack</a>. Feel free to drop by and say hello. Our upcoming features and beta releases are discussed here along with world peace.

Get your invitation for Postman Slack Community from: <a href="https://www.getpostman.com/slack-invite">https://www.getpostman.com/slack-invite</a>.<br />
Already member? Sign in at <a href="https://postmancommunity.slack.com">https://postmancommunity.slack.com</a>

## License
This software is licensed under Apache-2.0. Copyright Postdot Technologies, Inc. See the [LICENSE.md](LICENSE.md) file for more information.

[![Analytics](https://ga-beacon.appspot.com/UA-43979731-9/newman/readme)](https://www.getpostman.com)
