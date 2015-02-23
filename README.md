# webpack simple example

## Script

Create a bundle file by compiling files using webpack.
```
$ npm run start
```

Serve static files on localhost:8080/webpack-dev-server/bundle by compiling files using webpack.
```
$ npm run serve
```

## Steps
```
$ npm init
This utility will walk you through creating a package.json file.
It only covers the most common items, and tries to guess sane defaults.

See `npm help json` for definitive documentation on these fields
and exactly what they do.

Use `npm install <pkg> --save` afterwards to install a package and
save it as a dependency in the package.json file.

Press ^C at any time to quit.
name: (simple)
version: (1.0.0)
description:
entry point: (index.js)
test command:
git repository:
keywords:
author:
license: (ISC)
About to write to /Users/stanleyn/Projects/stanleyhlng/knowledge/webpack-test/simple/package.json:

{
  "name": "simple",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC"
}


Is this ok? (yes)
```

```
$ npm install webpack --save-dev
npm WARN package.json simple@1.0.0 No repository field.
|
> fsevents@0.3.5 install /Users/stanleyn/Projects/stanleyhlng/knowledge/webpack-test/simple/node_modules/webpack/node_modules/watchpack/node_modules/chokidar/node_modules/fsevents
> node-gyp rebuild

  SOLINK_MODULE(target) Release/.node
ld: warning: directory not found for option '-L/opt/local/lib'
  SOLINK_MODULE(target) Release/.node: Finished
  CXX(target) Release/obj.target/fse/fsevents.o
  SOLINK_MODULE(target) Release/fse.node
ld: warning: directory not found for option '-L/opt/local/lib'
  SOLINK_MODULE(target) Release/fse.node: Finished
webpack@1.5.3 node_modules/webpack
├── clone@0.1.19
├── tapable@0.1.8
├── memory-fs@0.2.0
├── async@0.9.0
├── mkdirp@0.5.0 (minimist@0.0.8)
├── optimist@0.6.1 (wordwrap@0.0.2, minimist@0.0.10)
├── enhanced-resolve@0.8.4 (graceful-fs@3.0.5)
├── webpack-core@0.4.8 (source-map@0.1.43)
├── esprima@1.2.4
├── uglify-js@2.4.16 (uglify-to-browserify@1.0.2, async@0.2.10, optimist@0.3.7, source-map@0.1.34)
├── node-libs-browser@0.4.1 (https-browserify@0.0.0, tty-browserify@0.0.0, constants-browserify@0.0.1, path-browserify@0.0.0, process@0.8.0, os-browserify@0.1.2, string_decoder@0.10.31, punycode@1.3.2, domain-browser@1.1.4, querystring-es3@0.2.1, assert@1.3.0, url@0.10.2, timers-browserify@1.3.0, stream-browserify@1.0.0, events@1.0.2, vm-browserify@0.0.4, console-browserify@1.1.0, util@0.10.3, http-browserify@1.7.0, readable-stream@1.1.13, buffer@2.8.2, browserify-zlib@0.1.4, crypto-browserify@3.3.0)
└── watchpack@0.2.3 (graceful-fs@3.0.5, chokidar@1.0.0-rc3)
```

```
$ touch entry.js
$ cat entry.js
document.write("It works.");
```

```
$ touch index.html
$ cat index.html
<html>
    <head>
        <meta charset="UTF-8">
    </head>
    <body>
        <script src="bundle.js" charset="UTF-8"></script>
    </body>
</html>
```

```
$ ./node_module/.bin/webpack ./entry.js bundle.js
Hash: e97678c23acf8ee01956
Version: webpack 1.5.3
Time: 30ms
    Asset  Size  Chunks             Chunk Names
bundle.js  1525       0  [emitted]  main
   [0] ./entry.js 29 {0} [built]
```

```
$ open index.html
```

```
$ touch content.js
$ cat content.js
module.exports = "It works from content.js";
$ cat entry.js
//document.write("It works.");
document.write(require("./content.js"));
$ ./node_module/.bin/webpack ./entry.js bundle.js
Hash: 2b180bb01c4fd15e0e5d
Version: webpack 1.5.3
Time: 34ms
    Asset  Size  Chunks             Chunk Names
bundle.js  1689       0  [emitted]  main
   [0] ./entry.js 72 {0} [built]
   [1] ./content.js 45 {0} [built]
```

```
$ npm install --save-dev css-loader style-loader
pm WARN package.json simple@1.0.0 No repository field.
style-loader@0.8.3 node_modules/style-loader
└── loader-utils@0.2.6 (json5@0.1.0, big.js@2.5.1)

css-loader@0.9.1 node_modules/css-loader
├── source-map@0.1.43 (amdefine@0.1.0)
├── csso@1.3.11
└── loader-utils@0.2.6 (json5@0.1.0, big.js@2.5.1)
```

```
$ touch style.css
$ cat style.css
body {
    background: yellow;
}
$cat entry.js
//document.write("It works.");
require("!style!css!./style.css");
document.write(require("./content.js"));
$ ./node_modules/.bin/webpack ./entry.js ./bundle.js
Hash: d558f2fc834e4a8232eb
Version: webpack 1.5.3
Time: 93ms
    Asset  Size  Chunks             Chunk Names
bundle.js  9171       0  [emitted]  main
   [0] ./entry.js 107 {0} [built]
   [1] ./content.js 45 {0} [built]
    + 4 hidden modules
```

```
$ cat entry.js
//document.write("It works.");
require("./style.css");
document.write(require("./content.js"));
./node_modules/.bin/webpack ./entry.js bundle.js --module-bind 'css=style!css'
Hash: 7a9a80928754cae4dc76
Version: webpack 1.5.3
Time: 93ms
    Asset  Size  Chunks             Chunk Names
bundle.js  9171       0  [emitted]  main
   [0] ./entry.js 96 {0} [built]
   [1] ./content.js 45 {0} [built]
    + 4 hidden modules
```

```
$ touch webpack.config.js
$ cat webpack.config.js
module.exports = {
    entry: "./entry.js",
    output: {
        path: __dirname,
        filename: "bundle.js"
    },
    module: {
        loaders: [
            {
                test: /\.css$/, loader: "style!css"
            }
        ]
    }
};
$ ./node_modules/.bin/webpack
Hash: 7a9a80928754cae4dc76
Version: webpack 1.5.3
Time: 87ms
    Asset  Size  Chunks             Chunk Names
bundle.js  9171       0  [emitted]  main
   [0] ./entry.js 96 {0} [built]
   [1] ./content.js 45 {0} [built]
    + 4 hidden modules
```

```
$ ./node_modules/.bin/webpack --progress --colors
```

```
$ ./node_modules/.bin/webpack --progress --colors --watch
```

```
$ npm i webpack-dev-server --save-dev
npm WARN package.json simple@1.0.0 No repository field.
\
> ws@0.4.32 install /Users/stanleyn/Projects/stanleyhlng/knowledge/webpack-test/simple/node_modules/webpack-dev-server/node_modules/socket.io/node_modules/socket.io-client/node_modules/ws
> (node-gyp rebuild 2> builderror.log) || (exit 0)

  CXX(target) Release/obj.target/bufferutil/src/bufferutil.o
  SOLINK_MODULE(target) Release/bufferutil.node
  SOLINK_MODULE(target) Release/bufferutil.node: Finished
  CXX(target) Release/obj.target/validation/src/validation.o
  SOLINK_MODULE(target) Release/validation.node
  SOLINK_MODULE(target) Release/validation.node: Finished
webpack-dev-server@1.7.0 node_modules/webpack-dev-server
├── connect-history-api-fallback@0.0.5
├── http-proxy@1.8.1 (requires-port@0.0.0, eventemitter3@0.1.6)
├── stream-cache@0.0.2
├── optimist@0.6.1 (wordwrap@0.0.2, minimist@0.0.10)
├── webpack-dev-middleware@1.0.11 (memory-fs@0.1.1, mime@1.3.4, enhanced-resolve@0.7.6)
├── express@4.12.0 (merge-descriptors@0.0.2, utils-merge@1.0.0, cookie-signature@1.0.6, methods@1.1.1, fresh@0.2.4, cookie@0.1.2, escape-html@1.0.1, range-parser@1.0.2, finalhandler@0.3.3, content-type@1.0.1, vary@1.0.0, parseurl@1.3.0, serve-static@1.9.1, content-disposition@0.5.0, path-to-regexp@0.1.3, depd@1.0.0, qs@2.3.3, on-finished@2.2.0, debug@2.1.1, send@0.12.1, proxy-addr@1.0.6, etag@1.5.1, type-is@1.6.0, accepts@1.2.4)
├── serve-index@1.6.2 (parseurl@1.3.0, batch@0.5.2, debug@2.1.1, accepts@1.2.4, http-errors@1.3.1, mime-types@2.0.9)
└── socket.io@0.9.17 (base64id@0.1.0, policyfile@0.0.4, redis@0.7.3, socket.io-client@0.9.16)
```

```
$ ./node_modules/.bin/webpack-dev-server --progress --color
0% compilehttp://localhost:8080/webpack-dev-server/
webpack result is served from /
content is served from /Users/stanleyn/Projects/stanleyhlng/knowledge/webpack-test/simple
Hash: 7a9a80928754cae4dc76
Version: webpack 1.5.3
Time: 111ms
  Asset  Size  Chunks             Chunk Names
bundle.js  9171       0  [emitted]  main
chunk    {0} bundle.js (main) 7484 [rendered]
  [0] ./entry.js 96 {0} [built]
  [1] ./content.js 45 {0} [built]
  [2] ./style.css 1267 {0} [built]
  [3] ./~/css-loader!./style.css 217 {0} [built]
  [4] ./~/style-loader/addStyles.js 5507 {0} [built]
  [5] ./~/css-loader/cssToString.js 352 {0} [built]
webpack: bundle is now VALID.
```
```
open http://localhost:8080/webpack-dev-server/bundle
```


## References
* http://webpack.github.io/docs/tutorials/getting-started/
