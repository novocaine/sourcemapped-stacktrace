This is a simple module for applying source maps to JS stack traces in the browser. 

## tldr; 

You have Error.stack() in JS (maybe you're logging a trace, or you're looking at
traces in Jasmine or Mocha), and you need to apply a sourcemap so you can
understand whats happening because you're using some fancy compiled-to-js thing
like coffeescript or traceur. This is the library you are looking for.

## Longer Explanation

Several modern browsers support sourcemaps when viewing stack traces from errors in their native console, but as of the time of writing there is no support for applying a sourcemap to the (highly non-standardised) [Error.prototype.stack](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Error/Stack). Error.prototype.stack can be used for logging errors and for displaying errors in test frameworks, and it is not very convenient to have unmapped traces in either of those use cases.

This module fetches all the scripts referenced by the stack trace, determines
whether they have an applicable sourcemap, fetches the sourcemap from the
server, then uses the [Mozilla source-map library](https://github.com/mozilla/source-map/) to do the mapping. Browsers that support sourcemaps don't offer a standardised sourcemap API, so we have to do all that work ourselves.

The nice part about doing it ourselves is that the library could be extended to
work in browsers that don't support sourcemaps, which could be good for
logging and debugging problems. Currently, only Chrome is supported, but it
would be easy to support those formats by ripping off [stacktrace.js](https://github.com/stacktracejs/stacktrace.js/).

## Demo

TODO

## Known issues

* Doesn't support exception formats of any browser other than Chrome
* Only supports JS containing //# sourceMappingURL= declarations (i.e. no
  support for the SourceMap: HTTP header (yet)
