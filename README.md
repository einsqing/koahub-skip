# koahub skip

Conditionally skip a middleware when a condition is met.

## Install

	npm i koahub-skip --save

## Usage

With existing middlewares:

```javascript
var skip = require('koahub-skip');
var serve  = require('koa-static');

var static = serve(__dirname + '/public');
static.skip = skip;

app.use(static.skip({ method: 'OPTIONS' }));
```

If you are authoring a middleware you can support skip as follow:

```javascript
module.exports = function () {
  var mymid = function (ctx, next) {
	// Do something
  };

  mymid.skip = require('koahub-skip');

  return mymid;
};
```

## Current options

-  `method` it could be an string or an array of strings. If the request method match the middleware will not run.
-  `path` it could be an string, a regexp or an array of any of those. If the request path match, the middleware will not run.
-  `ext` it could be an string or an array of strings. If the request path ends with one of these extensions the middleware will not run.
-  `custom` it must be a function that returns `true` / `false`. If the function returns true for the given request, ithe middleware will not run. The function will have access to Koa's context via `this`
-  `useOriginalUrl` it should be `true` or `false`, default is `true`. if false, `path` will match against `ctx.url` instead of `ctx.originalUrl`.


## Examples

Require authentication for every request skip the path is index.html.

```javascript
app.use(requiresAuth().skip({ path: ['/index.html', '/'] }))
```

Avoid a fstat for request to routes doesnt end with a given extension.

```javascript
app.use(static.skip(function () {
  var ext = url.parse(this.originalUrl).pathname.substr(-4);
  return !~['.jpg', '.html', '.css', '.js'].indexOf(ext);
}));
```

官网：[http://js.koahub.com](http://js.koahub.com)

[![image](http://www.koahub.com/public/ad.jpg "koahub软件市场")](http://www.koahub.com)
