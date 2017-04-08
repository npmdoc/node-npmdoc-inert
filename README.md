# api documentation for  [inert (v4.2.0)](https://github.com/hapijs/inert#readme)  [![npm package](https://img.shields.io/npm/v/npmdoc-inert.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-inert) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-inert.svg)](https://travis-ci.org/npmdoc/node-npmdoc-inert)
#### Static file and directory handlers plugin for hapi.js

[![NPM](https://nodei.co/npm/inert.png?downloads=true)](https://www.npmjs.com/package/inert)

[![apidoc](https://npmdoc.github.io/node-npmdoc-inert/build/screenCapture.buildNpmdoc.browser.%252Fhome%252Ftravis%252Fbuild%252Fnpmdoc%252Fnode-npmdoc-inert%252Ftmp%252Fbuild%252Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-inert/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-inert/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-inert/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "bugs": {
        "url": "https://github.com/hapijs/inert/issues"
    },
    "dependencies": {
        "ammo": "2.x.x",
        "boom": "4.x.x",
        "hoek": "4.x.x",
        "items": "2.x.x",
        "joi": "10.x.x",
        "lru-cache": "4.0.x"
    },
    "description": "Static file and directory handlers plugin for hapi.js",
    "devDependencies": {
        "code": "4.x.x",
        "hapi": "16.x.x",
        "lab": "13.x.x"
    },
    "directories": {},
    "dist": {
        "shasum": "6aa5da8ce19982eb72aabef4bdda64fd6750b005",
        "tarball": "https://registry.npmjs.org/inert/-/inert-4.2.0.tgz"
    },
    "engines": {
        "node": ">=4.0.0"
    },
    "gitHead": "3ae1e8a3dc14f3695eeedb2bbbd7a5444819f03f",
    "homepage": "https://github.com/hapijs/inert#readme",
    "keywords": [
        "file",
        "directory",
        "handler",
        "hapi",
        "plugin"
    ],
    "license": "BSD-3-Clause",
    "main": "lib/index.js",
    "maintainers": [
        {
            "name": "hueniverse",
            "email": "eran@hammer.io"
        },
        {
            "name": "kanongil",
            "email": "gpdev@gpost.dk"
        },
        {
            "name": "marsup",
            "email": "nicolas@morel.io"
        },
        {
            "name": "nlf",
            "email": "quitlahok@gmail.com"
        },
        {
            "name": "wyatt",
            "email": "wpreul@gmail.com"
        }
    ],
    "name": "inert",
    "optionalDependencies": {},
    "readme": "ERROR: No README data found!",
    "repository": {
        "type": "git",
        "url": "git://github.com/hapijs/inert.git"
    },
    "scripts": {
        "test": "lab -f -a code -t 100 -L",
        "test-cov-html": "lab -f -a code -r html -o coverage.html"
    },
    "version": "4.2.0"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module inert](#apidoc.module.inert)
1.  [function <span class="apidocSignatureSpan">inert.</span>register (server, options, next)](#apidoc.element.inert.register)
1.  object <span class="apidocSignatureSpan">inert.</span>directory
1.  object <span class="apidocSignatureSpan">inert.</span>etag
1.  object <span class="apidocSignatureSpan">inert.</span>file
1.  object <span class="apidocSignatureSpan">inert.</span>fs

#### [module inert.directory](#apidoc.module.inert.directory)
1.  [function <span class="apidocSignatureSpan">inert.directory.</span>handler (route, options)](#apidoc.element.inert.directory.handler)

#### [module inert.etag](#apidoc.module.inert.etag)
1.  [function <span class="apidocSignatureSpan">inert.etag.</span>Cache (options)](#apidoc.element.inert.etag.Cache)
1.  [function <span class="apidocSignatureSpan">inert.etag.</span>apply (response, stat, next)](#apidoc.element.inert.etag.apply)

#### [module inert.file](#apidoc.module.inert.file)
1.  [function <span class="apidocSignatureSpan">inert.file.</span>handler (route, options)](#apidoc.element.inert.file.handler)
1.  [function <span class="apidocSignatureSpan">inert.file.</span>load (path, request, options, callback)](#apidoc.element.inert.file.load)
1.  [function <span class="apidocSignatureSpan">inert.file.</span>response (path, options, request, _preloaded)](#apidoc.element.inert.file.response)

#### [module inert.fs](#apidoc.module.inert.fs)
1.  [function <span class="apidocSignatureSpan">inert.fs.</span>File (path)](#apidoc.element.inert.fs.File)



# <a name="apidoc.module.inert"></a>[module inert](#apidoc.module.inert)

#### <a name="apidoc.element.inert.register"></a>[function <span class="apidocSignatureSpan">inert.</span>register (server, options, next)](#apidoc.element.inert.register)
- description and source-code
```javascript
register = function (server, options, next) {

    const settings = Hoek.applyToDefaults(internals.defaults, options);

    server.expose('_etags', settings.etagsCacheMaxSize ? new Etag.Cache(settings.etagsCacheMaxSize) : null);

    server.handler('file', File.handler);
    server.handler('directory', Directory.handler);

    server.decorate('reply', 'file', function (path, responseOptions) {

        // Set correct confine value

        responseOptions = responseOptions || {};

        if (typeof responseOptions.confine === 'undefined' || responseOptions.confine === true) {
            responseOptions.confine = '.';
        }

        Hoek.assert(responseOptions.end === undefined || +responseOptions.start <= +responseOptions.end, 'options.start must be
less than or equal to options.end');

        return this.response(File.response(path, responseOptions, this.request));
    });

    return next();
}
```
- example usage
```shell
...
            relativeTo: Path.join(__dirname, 'public')
        }
    }
}
});
server.connection({ port: 3000 });

server.register(Inert, () => {});

server.route({
method: 'GET',
path: '/{param*}',
handler: {
    directory: {
        path: '.',
...
```



# <a name="apidoc.module.inert.directory"></a>[module inert.directory](#apidoc.module.inert.directory)

#### <a name="apidoc.element.inert.directory.handler"></a>[function <span class="apidocSignatureSpan">inert.directory.</span>handler (route, options)](#apidoc.element.inert.directory.handler)
- description and source-code
```javascript
handler = function (route, options) {

    const settings = Joi.attempt(options, internals.schema, 'Invalid directory handler options (' + route.path + ')');
    Hoek.assert(route.path[route.path.length - 1] === '}', 'The route path for a directory handler must end with a parameter:',
route.path);

    const paramName = /\w+/.exec(route.path.slice(route.path.lastIndexOf('{')))[0];

    const normalize = (paths) => {

        const normalized = [];
        for (let i = 0; i < paths.length; ++i) {
            let path = paths[i];

            if (!Path.isAbsolute(path)) {
                path = Path.join(route.settings.files.relativeTo, path);
            }

            normalized.push(path);
        }

        return normalized;
    };

    const normalized = (Array.isArray(settings.path) ? normalize(settings.path) : []);            // Array or function

    const indexNames = (settings.index === true) ? ['index.html'] : (settings.index || []);

    // Declare handler

    const handler = (request, reply) => {

        let paths = normalized;
        if (typeof settings.path === 'function') {
            const result = settings.path.call(null, request);
            if (result instanceof Error) {
                return reply(result);
            }

            if (Array.isArray(result)) {
                paths = normalize(result);
            }
            else if (typeof result === 'string') {
                paths = normalize([result]);
            }
            else {
                return reply(Boom.badImplementation('Invalid path function'));
            }
        }

        // Append parameter

        const selection = request.params[paramName];
        if (selection &&
            !settings.showHidden &&
            internals.isFileHidden(selection)) {

            return reply(Boom.notFound());
        }

        // Generate response

        const resource = request.path;
        const hasTrailingSlash = resource.endsWith('/');
        const fileOptions = {
            confine: null,
            lookupCompressed: settings.lookupCompressed,
            lookupMap: settings.lookupMap,
            etagMethod: settings.etagMethod
        };

        Items.serial(paths, (baseDir, nextPath) => {

            fileOptions.confine = baseDir;

            let path = selection || '';

            File.load(path, request, fileOptions, (response) => {

                // File loaded successfully

                if (!response.isBoom) {
                    return reply(response);
                }

                // Not found

                const err = response;
                if (err.output.statusCode === 404) {
                    if (!settings.defaultExtension) {
                        return nextPath();
                    }

                    if (hasTrailingSlash) {
                        path = path.slice(0, -1);
                    }

                    return File.load(path + '.' + settings.defaultExtension, request, fileOptions, (extResponse) => {

                        if (!extResponse.isBoom) {
                            return reply(extResponse);
                        }

                        return nextPath();
                    });
                }

                // Propagate non-directory errors

                if (err.output.statusCode !== 403 || err.data !== 'EISDIR') {
                    return reply(err);
                }

                // Directory

                if (indexNames.length === 0 &&
                    !settings.listing) {

                    return reply(Boom.forbidden());
                }

                if (settings.redirectToSlash !== false &&                       // Defaults to true
                    !request.connection.settings.router.stripTrailingSlash &&
                    !hasTrailingSlash) {

                    return reply.redirect(resource + '/');
                }

                Items.serial(indexNames, (indexName, nextIndex) => {

                    const indexFile = Path.join(path, indexName);
                    File.load(indexFile, request, fileOption ...
```
- example usage
```shell
n/a
```



# <a name="apidoc.module.inert.etag"></a>[module inert.etag](#apidoc.module.inert.etag)

#### <a name="apidoc.element.inert.etag.Cache"></a>[function <span class="apidocSignatureSpan">inert.etag.</span>Cache (options)](#apidoc.element.inert.etag.Cache)
- description and source-code
```javascript
function LRUCache(options) {
  if (!(this instanceof LRUCache)) {
    return new LRUCache(options)
  }

  if (typeof options === 'number') {
    options = { max: options }
  }

  if (!options) {
    options = {}
  }

  var max = priv(this, 'max', options.max)
  // Kind of weird to have a default max of Infinity, but oh well.
  if (!max ||
      !(typeof max === 'number') ||
      max <= 0) {
    priv(this, 'max', Infinity)
  }

  var lc = options.length || naiveLength
  if (typeof lc !== 'function') {
    lc = naiveLength
  }
  priv(this, 'lengthCalculator', lc)

  priv(this, 'allowStale', options.stale || false)
  priv(this, 'maxAge', options.maxAge || 0)
  priv(this, 'dispose', options.dispose)
  this.reset()
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.inert.etag.apply"></a>[function <span class="apidocSignatureSpan">inert.etag.</span>apply (response, stat, next)](#apidoc.element.inert.etag.apply)
- description and source-code
```javascript
apply = function (response, stat, next) {

    const etagMethod = response.source.settings.etagMethod;
    if (etagMethod === false) {
        return next();
    }

    const applyEtag = (err, etag) => {

        if (err) {
            return next(err);
        }

        if (etag !== null) {
            response.etag(etag, { vary: true });
        }

        return next();
    };

    if (etagMethod === 'simple') {
        return internals.computeSimple(response, stat, applyEtag);
    }

    return internals.computeHashed(response, stat, applyEtag);
}
```
- example usage
```shell
...
        response.header('last-modified', stat.mtime.toUTCString());

        if (response.source.settings.mode) {
const fileName = response.source.settings.filename || Path.basename(path);
response.header('content-disposition', response.source.settings.mode + '; filename=' + encodeURIComponent(fileName));
        }

        Etag.apply(response, stat, (err) => {

if (err) {
    internals.close(response);
    return callback(err);
}

return callback(response);
...
```



# <a name="apidoc.module.inert.file"></a>[module inert.file](#apidoc.module.inert.file)

#### <a name="apidoc.element.inert.file.handler"></a>[function <span class="apidocSignatureSpan">inert.file.</span>handler (route, options)](#apidoc.element.inert.file.handler)
- description and source-code
```javascript
handler = function (route, options) {

    let settings = Joi.attempt(options, internals.schema, 'Invalid file handler options (' + route.path + ')');
    settings = (typeof options !== 'object' ? { path: options, confine: '.' } : settings);
    settings.confine = settings.confine === true ? '.' : settings.confine;
    Hoek.assert(typeof settings.path !== 'string' || settings.path[settings.path.length - 1] !== '/', 'File path cannot end with
 a\'/\':', route.path);

    const handler = (request, reply) => {

        const path = (typeof settings.path === 'function' ? settings.path(request) : settings.path);
        return reply(exports.response(path, settings, request));
    };

    return handler;
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.inert.file.load"></a>[function <span class="apidocSignatureSpan">inert.file.</span>load (path, request, options, callback)](#apidoc.element.inert.file.load)
- description and source-code
```javascript
load = function (path, request, options, callback) {

    const response = exports.response(path, options, request, true);
    return internals.prepare(response, callback);
}
```
- example usage
```shell
...

        Items.serial(paths, (baseDir, nextPath) => {

            fileOptions.confine = baseDir;

            let path = selection || '';

            File.load(path, request, fileOptions, (response) => {

// File loaded successfully

if (!response.isBoom) {
    return reply(response);
}
...
```

#### <a name="apidoc.element.inert.file.response"></a>[function <span class="apidocSignatureSpan">inert.file.</span>response (path, options, request, _preloaded)](#apidoc.element.inert.file.response)
- description and source-code
```javascript
response = function (path, options, request, _preloaded) {

    Hoek.assert(!options.mode || ['attachment', 'inline'].indexOf(options.mode) !== -1, 'options.mode must be either false, attachment
, or inline');

    if (options.confine) {
        const confineDir = Path.resolve(request.route.settings.files.relativeTo, options.confine);
        path = Path.isAbsolute(path) ? Path.normalize(path) : Path.join(confineDir, path);

        // Verify that resolved path is within confineDir
        if (path.lastIndexOf(confineDir, 0) !== 0) {
            path = null;
        }
    }
    else {
        path = Path.isAbsolute(path) ? Path.normalize(path) : Path.join(request.route.settings.files.relativeTo, path);
    }

    const source = {
        path,
        settings: options,
        stat: null,
        file: null
    };

    const prepare = _preloaded ? null : internals.prepare;

    return request.generateResponse(source, { variety: 'file', marshal: internals.marshal, prepare, close: internals.close });
}
```
- example usage
```shell
...
    settings = (typeof options !== 'object' ? { path: options, confine: '.' } : settings);
    settings.confine = settings.confine === true ? '.' : settings.confine;
    Hoek.assert(typeof settings.path !== 'string' || settings.path[settings.path.length - 1] !== '/', 'File path cannot end with
 a\'/\':', route.path);

    const handler = (request, reply) => {

        const path = (typeof settings.path === 'function' ? settings.path(request) : settings.path);
        return reply(exports.response(path, settings, request));
    };

    return handler;
};


exports.load = function (path, request, options, callback) {
...
```



# <a name="apidoc.module.inert.fs"></a>[module inert.fs](#apidoc.module.inert.fs)

#### <a name="apidoc.element.inert.fs.File"></a>[function <span class="apidocSignatureSpan">inert.fs.</span>File (path)](#apidoc.element.inert.fs.File)
- description and source-code
```javascript
File = function (path) {

    this.path = path;
    this.fd = null;
}
```
- example usage
```shell
...
    if (path === null) {
return process.nextTick(() => {

    return callback(Boom.forbidden(null, 'EACCES'));
});
    }

    const file = response.source.file = new Fs.File(path);

    file.openStat('r', (err, stat) => {

if (err) {
    return callback(err);
}
...
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
