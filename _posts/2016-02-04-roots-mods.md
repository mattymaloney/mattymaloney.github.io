---
published: true
layout: post
title: Modifications to Roots.cx
---

Added `/usr/local/lib/node_modules/roots/node_modules/accord/lib/adapters/slm.js`:

```
(function() {
  var Adapter, Slm, W, fs, path,
    extend = function(child, parent) { for (var key in parent) { if (hasProp.call(parent, key)) child[key] = parent[key]; } function ctor() { this.constructor = child; } ctor.prototype = parent.prototype; child.prototype = new ctor(); child.__super__ = parent.prototype; return child; },
    hasProp = {}.hasOwnProperty;

  Adapter = require('../adapter_base');

  path = require('path');

  fs = require('fs');

  W = require('when');

  Slm = (function(superClass) {
    var compile;

    extend(Slm, superClass);

    function Slm() {
      return Slm.__super__.constructor.apply(this, arguments);
    }

    Slm.prototype.name = 'slm';

    Slm.prototype.extensions = ['slm'];

    Slm.prototype.output = 'html';

    Slm.prototype._render = function(str, options) {
      // Can I derive the basePath value from a variable created in
      // the roots app.coffe? 
      options.basePath = options.basePath || path.resolve('views');

      // for caching issue, see commit in this PR:
      // https://github.com/bradgreens/github-test-gulp-slm-partials/pull/1
      // I'd certainly like to be able to make this option dependent on
      // the roots environment state.
      options.useCache = false;

      return compile((function(_this) {
        return function() {
          return _this.engine.render(str, null, options);
        };
      })(this));
    };

    Slm.prototype._compile = function(str, options) {
      return compile((function(_this) {
        return function() {
          return _this.engine.compile(str, options);
        };
      })(this));
    };

    Slm.prototype._compileClient = function(str, options) {
      return compile((function(_this) {
        return function() {
          return _this.engine.compile(str, options).toString().trim() + '\n';
        };
      })(this));
    };

    compile = function(fn) {
      var err, res;
      try {
        res = fn();
      } catch (_error) {
        err = _error;
        return W.reject(err);
      }
      return W.resolve({
        result: res
      });
    };

    return Slm;

  })(Adapter);

  module.exports = Slm;

}).call(this);
```