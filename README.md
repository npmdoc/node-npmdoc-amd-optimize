# api documentation for  [amd-optimize (v0.6.1)](https://github.com/scalableminds/amd-optimize#readme)  [![npm package](https://img.shields.io/npm/v/npmdoc-amd-optimize.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-amd-optimize) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-amd-optimize.svg)](https://travis-ci.org/npmdoc/node-npmdoc-amd-optimize)
#### An AMD (i.e. RequireJS) optimizer that's stream-friendly. Made for gulp. (WIP)

[![NPM](https://nodei.co/npm/amd-optimize.png?downloads=true)](https://www.npmjs.com/package/amd-optimize)

[![apidoc](https://npmdoc.github.io/node-npmdoc-amd-optimize/build/screenCapture.buildNpmdoc.browser._2Fhome_2Ftravis_2Fbuild_2Fnpmdoc_2Fnode-npmdoc-amd-optimize_2Ftmp_2Fbuild_2Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-amd-optimize/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-amd-optimize/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-amd-optimize/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "author": {
        "name": "Norman Rzepka",
        "email": "norman@scm.io",
        "url": "http://scm.io"
    },
    "bugs": {
        "url": "https://github.com/scalableminds/amd-optimize/issues"
    },
    "dependencies": {
        "acorn": "^0.9.0",
        "ast-types": "~0.3.16",
        "async": "~0.2.10",
        "escodegen": "^1.4.1",
        "lodash": "~2.4.1",
        "through2": "~0.4.1",
        "vinyl": "^0.2.3",
        "vinyl-fs": "^0.3.9",
        "vinyl-sourcemaps-apply": "^0.1.4"
    },
    "description": "An AMD (i.e. RequireJS) optimizer that's stream-friendly. Made for gulp. (WIP)",
    "devDependencies": {
        "coffee-script": "~1.7.1",
        "gulp": "^3.9.0",
        "gulp-coffee": "^2.3.1",
        "gulp-concat": "^2.3.4",
        "gulp-if": "0.0.5",
        "gulp-mocha": "^0.4.1",
        "gulp-plumber": "^0.6.3",
        "gulp-sourcemaps": "^1.1.1",
        "gulp-uglify": "~0.2.1",
        "gulp-util": "~2.2.14"
    },
    "directories": {},
    "dist": {
        "shasum": "97a9d57fb19cb50d749164628571a1778015681f",
        "tarball": "https://registry.npmjs.org/amd-optimize/-/amd-optimize-0.6.1.tgz"
    },
    "gitHead": "878e319e765672c774d975d45e8a3d3baf9b7904",
    "homepage": "https://github.com/scalableminds/amd-optimize#readme",
    "keywords": [
        "gulpplugin",
        "gulpfriendly"
    ],
    "license": "MIT",
    "main": "lib/index.js",
    "maintainers": [
        {
            "name": "normanrz",
            "email": "norman@scm.io"
        }
    ],
    "name": "amd-optimize",
    "optionalDependencies": {},
    "readme": "ERROR: No README data found!",
    "repository": {
        "type": "git",
        "url": "git://github.com/scalableminds/amd-optimize.git"
    },
    "scripts": {
        "test": "gulp test"
    },
    "version": "0.6.1"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module amd-optimize](#apidoc.module.amd-optimize)
1.  [function <span class="apidocSignatureSpan">amd-optimize.</span>loader (filenameResolver, pipe)](#apidoc.element.amd-optimize.loader)
1.  [function <span class="apidocSignatureSpan">amd-optimize.</span>src (moduleName, options)](#apidoc.element.amd-optimize.src)
1.  object <span class="apidocSignatureSpan">amd-optimize.</span>util

#### [module amd-optimize.util](#apidoc.module.amd-optimize.util)
1.  [function <span class="apidocSignatureSpan">amd-optimize.util.</span>fixModuleName (moduleName)](#apidoc.element.amd-optimize.util.fixModuleName)
1.  [function <span class="apidocSignatureSpan">amd-optimize.util.</span>logger ()](#apidoc.element.amd-optimize.util.logger)
1.  [function <span class="apidocSignatureSpan">amd-optimize.util.</span>printTree (currentModule, prefix)](#apidoc.element.amd-optimize.util.printTree)



# <a name="apidoc.module.amd-optimize"></a>[module amd-optimize](#apidoc.module.amd-optimize)

#### <a name="apidoc.element.amd-optimize.loader"></a>[function <span class="apidocSignatureSpan">amd-optimize.</span>loader (filenameResolver, pipe)](#apidoc.element.amd-optimize.loader)
- description and source-code
```javascript
loader = function (filenameResolver, pipe) {
  return function(moduleName, callback) {
    var filename, source;
    if (filenameResolver) {
      filename = filenameResolver(moduleName);
    } else {
      filename = moduleName;
    }
    source = vinylFs.src(filename).pipe(through.obj());
    if (pipe) {
      source = source.pipe(pipe());
    }
    firstChunk(source, callback);
  };
}
```
- example usage
```shell
...

#### options.loader
WIP. Subject to change.

'''js
amdOptimize.src(
  "index",
  loader : amdOptimize.loader(
    // Used for turning a moduleName into a filepath glob.
    function (moduleName) { return "src/scripts/" + moduleName + ".coffee" },
    // Returns a transform stream.
    function () { return coffee(); }
  )
)
'''
...
```

#### <a name="apidoc.element.amd-optimize.src"></a>[function <span class="apidocSignatureSpan">amd-optimize.</span>src (moduleName, options)](#apidoc.element.amd-optimize.src)
- description and source-code
```javascript
src = function (moduleName, options) {
  var source;
  source = rjs(moduleName, options);
  process.nextTick(function() {
    return source.end();
  });
  return source;
}
```
- example usage
```shell
...
'''js
var gulp = require("gulp");
var amdOptimize = require("amd-optimize");
var concat = require('gulp-concat');

gulp.task("scripts:index", function () {

  return gulp.src("src/scripts/**/*.js")
    // Traces all modules and outputs them in the correct order.
    .pipe(amdOptimize("main"))
    .pipe(concat("index.js"))
    .pipe(gulp.dest("dist/scripts"));

});
'''
...
```



# <a name="apidoc.module.amd-optimize.util"></a>[module amd-optimize.util](#apidoc.module.amd-optimize.util)

#### <a name="apidoc.element.amd-optimize.util.fixModuleName"></a>[function <span class="apidocSignatureSpan">amd-optimize.util.</span>fixModuleName (moduleName)](#apidoc.element.amd-optimize.util.fixModuleName)
- description and source-code
```javascript
fixModuleName = function (moduleName) {
  return moduleName.replace(/\\/g, '/');
}
```
- example usage
```shell
...
util = require("./util");

Module = (function() {
  function Module(name, file1, deps1) {
    this.name = name;
    this.file = file1;
    this.deps = deps1 != null ? deps1 : [];
    this.name = util.fixModuleName(this.name);
    this.isShallow = false;
    this.isShimmed = false;
    this.isAnonymous = false;
    this.isInline = false;
    this.hasDefine = false;
    this.astNodes = [];
  }
...
```

#### <a name="apidoc.element.amd-optimize.util.logger"></a>[function <span class="apidocSignatureSpan">amd-optimize.util.</span>logger ()](#apidoc.element.amd-optimize.util.logger)
- description and source-code
```javascript
logger = function () {
  return through.obj(function(file, enc, callback) {
    console.log(">>", path.relative(process.cwd(), file.path));
    return callback(null, file);
  });
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.amd-optimize.util.printTree"></a>[function <span class="apidocSignatureSpan">amd-optimize.util.</span>printTree (currentModule, prefix)](#apidoc.element.amd-optimize.util.printTree)
- description and source-code
```javascript
printTree = function (currentModule, prefix) {
  var depPrefix;
  if (prefix == null) {
    prefix = "";
  }
  console.log(prefix, currentModule.name, "(" + (path.relative(process.cwd(), currentModule.file.relative)) + ")");
  depPrefix = prefix.replace("├", "|").replace("└", " ").replace(/─/g, " ");
  return currentModule.deps.forEach(function(depModule, i) {
    if (i + 1 < currentModule.deps.length) {
      return printTree(depModule, depPrefix + " ├──");
    } else {
      return printTree(depModule, depPrefix + " └──");
    }
  });
}
```
- example usage
```shell
n/a
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
