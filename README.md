Heroku Buildpack for Node.js, gulp.js, and bower
========================================

Usage
-----

- Set your Heroku app's buildpack URL to `https://github.com/davidmfoley/heroku-buildpack-nodejs-gulp-bower.git`. To be safe, you should really fork this and use your fork's URL.
- Run `heroku labs:enable user-env-compile` to enable environment variable support
- Run `heroku config:set NODE_ENV=production` to set your environment to `production` (or any other name)
- Add a Gulp task called `heroku:production` that builds your app
- Add a single line `Procfile` to the root to serve the app via node:

```
web: node server.js
```

> You can find an example for a `server.js` in [the article that inspired this buildpack](http://www.sitepoint.com/deploying-yeomanangular-app-heroku/).


*Original README follows*

---

How it Works
------------

Here's an overview of what this buildpack does:

- Uses the [semver.io](https://semver.io) webservice to find the latest version of node that satisfies the [engines.node semver range](https://npmjs.org/doc/json.html#engines) in your package.json.
- Allows any recent version of node to be used, including [pre-release versions](https://semver.io/node.json).
- Uses an [S3 caching proxy](https://github.com/heroku/s3pository#readme) of nodejs.org for faster downloads of the node binary.
- Discourages use of dangerous semver ranges like `*` and `>0.10`.
- Uses the version of `npm` that comes bundled with `node`.
- Puts `node` and `npm` on the `PATH` so they can be executed with [heroku run](https://devcenter.heroku.com/articles/one-off-dynos#an-example-one-off-dyno).
- Caches the `node_modules` directory across builds for fast deploys.
- Doesn't use the cache if `node_modules` is checked into version control.
- Runs `npm rebuild` if `node_modules` is checked into version control.
- Always runs `npm install` to ensure [npm script hooks](https://npmjs.org/doc/misc/npm-scripts.html) are executed.
- Always runs `npm prune` after restoring cached modules to ensure cleanup of unused dependencies.

For more technical details, see the [heavily-commented compile script](https://github.com/heroku/heroku-buildpack-nodejs/blob/master/bin/compile).


Documentation
-------------

For more information about using Node.js and buildpacks on Heroku, see these Dev Center articles:

- [Heroku Node.js Support](https://devcenter.heroku.com/articles/nodejs-support)
- [Getting Started with Node.js on Heroku](https://devcenter.heroku.com/articles/nodejs)
- [10 Habits of a Happy Node Hacker](https://blog.heroku.com/archives/2014/3/11/node-habits)
- [Buildpacks](https://devcenter.heroku.com/articles/buildpacks)
- [Buildpack API](https://devcenter.heroku.com/articles/buildpack-api)


Credits
-------

Inspired by [Deploying a Yeoman/Angular app to Heroku](http://www.sitepoint.com/deploying-yeomanangular-app-heroku/).

Heavily based on [heroku-buildpack-nodejs-grunt](https://github.com/mbuchetics/heroku-buildpack-nodejs-grunt).
